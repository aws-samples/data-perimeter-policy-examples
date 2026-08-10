# Service-specific guidance: Amazon Relational Database Service


This document outlines service-specific guidance for implementing a data perimeter for Amazon Relational Database Service.

Amazon Relational Database Service (Amazon RDS) is a web service that makes it easier to set up, operate, and scale a relational database in the AWS Cloud. It provides cost-efficient, resizable capacity for an industry-standard relational database and manages common database administration tasks.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | Y |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | Y |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | N |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | Y |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | Y |

*Y - Additional considerations apply. N - No additional considerations apply.

## Connect
### IAM authentication in VPCs

**Perimeter type applicability**: network perimeter.

**Description**: [Connect](https://docs.aws.amazon.com/service-authorization/latest/reference/list_rds-db.html#list_rds-db-actions-as-permissions) is used to access databases in your VPCs with IAM authentication.

**Additional controls**:

If you want to restrict access to expected networks for your identities:
* Preventative control example: Consider implementing the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json), which lists [Connect](https://docs.aws.amazon.com/service-authorization/latest/reference/list_rds-db.html#list_rds-db-actions-as-permissions) in the `NotAction` element. See [Services and actions that require an exception to the network perimeter](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies#services-and-actions-that-require-an-exception-to-the-network-perimeter) for other IAM actions that should be listed in the `NotAction` when enforcing network perimeter controls across a broad set of services.

If you want to restrict access to your resources to expected networks, consider implementing this additional control:
* Preventative control example: Consider implementing standard infrastructure security controls to restrict which networks and IP addresses can access databases. These controls include [security groups](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html), [network access control lists](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html), and firewalls such as [AWS Network Firewall](https://aws.amazon.com/network-firewall/).

## ModifyDBClusterSnapshotAttribute
### Service sharing mechanism

**Perimeter type applicability**: identity perimeter applied on resource; resource perimeter applied on identity.

**Description**: [ModifyDBClusterSnapshotAttribute](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_ModifyDBClusterSnapshotAttribute.html) allows you to share a DB cluster snapshot with another account.

**Additional controls**:

If you want to restrict access so that only trusted identities can view information about your resources, consider implementing these additional controls:
* Preventative control example: Consider restricting [ModifyDBClusterSnapshotAttribute](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_ModifyDBClusterSnapshotAttribute.html) permissions to select principals using an SCP. See [data_perimeter_governance_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/data_perimeter_governance_scp.json) for an example policy and ["Sid":"PreventExternalResourceShare"](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies#sidpreventexternalresourceshare) for a list of resources that can be granted cross-account access.
* Detective control example: Consider using CloudTrail management events to monitor the [ModifyDBClusterSnapshotAttribute](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_ModifyDBClusterSnapshotAttribute.html) API calls in your environment (specifically, the [ValuesToAdd](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_ModifyDBClusterSnapshotAttribute.html#API_ModifyDBClusterSnapshotAttribute_RequestParameters) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access so that your identities cannot view resources that were shared with your accounts by untrusted entities, consider implementing this additional control:
* Detective control example: Consider using [DescribeDBClusterSnapshots](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_DescribeDBClusterSnapshots.html) to monitor the DB cluster snapshots shared with your accounts (specifically, the [DBClusterSnapshots](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_DBClusterSnapshot.html) response parameter). If necessary, remediate with the responsive controls of your choice.

## ModifyDBSnapshotAttribute
### Service sharing mechanism

**Perimeter type applicability**: identity perimeter applied on resource; resource perimeter applied on identity.

**Description**: [ModifyDBSnapshotAttribute](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_ModifyDBSnapshotAttribute.html) allows you to share a DB snapshot with another account.

**Additional controls**:

If you want to restrict access so that only trusted identities can view information about your resources, consider implementing these additional controls:
* Preventative control example: Consider restricting [ModifyDBSnapshotAttribute](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_ModifyDBSnapshotAttribute.html) permissions to select principals using an SCP. See [data_perimeter_governance_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/data_perimeter_governance_scp.json) for an example policy and ["Sid":"PreventExternalResourceShare"](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies#sidpreventexternalresourceshare) for a list of resources that can be granted cross-account access.
* Detective control example: Consider using CloudTrail management events to monitor the [ModifyDBSnapshotAttribute](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_ModifyDBSnapshotAttribute.html) API calls in your environment (specifically, the [ValuesToAdd](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_ModifyDBSnapshotAttribute.html#API_ModifyDBSnapshotAttribute_RequestParameters) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access so that your identities cannot view resources that were shared with your accounts by untrusted entities, consider implementing this additional control:
* Detective control example: Consider using [DescribeDBSnapshots](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_DescribeDBSnapshots.html) to monitor the DB snapshots shared with your accounts (specifically, the [DBSnapshots](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_DBSnapshot.html) response parameter). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

* AddRoleToDBCluster
* AddSourceIdentifierToSubscription
* AddTagsToResource
* BacktrackDBCluster
* CancelExportTask
* CopyDBClusterParameterGroup
* CopyDBClusterSnapshot
* CopyDBParameterGroup
* CopyDBSnapshot
* CopyOptionGroup
* CreateBlueGreenDeployment
* CreateDBCluster
* CreateDBClusterEndpoint
* CreateDBClusterParameterGroup
* CreateDBClusterSnapshot
* CreateDBInstance
* CreateDBInstanceReadReplica
* CreateDBParameterGroup
* CreateDBProxy
* CreateDBProxyEndpoint
* CreateDBShardGroup
* CreateDBSnapshot
* CreateDBSubnetGroup
* CreateEventSubscription
* CreateGlobalCluster
* CreateIntegration
* CreateOptionGroup
* DeleteBlueGreenDeployment
* DeleteDBCluster
* DeleteDBClusterEndpoint
* DeleteDBClusterParameterGroup
* DeleteDBClusterSnapshot
* DeleteDBInstance
* DeleteDBParameterGroup
* DeleteDBProxy
* DeleteDBProxyEndpoint
* DeleteDBShardGroup
* DeleteDBSnapshot
* DeleteDBSubnetGroup
* DeleteEventSubscription
* DeleteGlobalCluster
* DeleteIntegration
* DeleteOptionGroup
* DeregisterDBProxyTargets
* DescribeAccountAttributes
* DescribeBlueGreenDeployments
* DescribeCertificates
* DescribeDBClusterAutomatedBackups
* DescribeDBClusterBacktracks
* DescribeDBClusterEndpoints
* DescribeDBClusterParameterGroups
* DescribeDBClusterParameters
* DescribeDBClusterSnapshotAttributes
* DescribeDBClusterSnapshots
* DescribeDBClusters
* DescribeDBEngineVersions
* DescribeDBInstanceAutomatedBackups
* DescribeDBInstances
* DescribeDBLogFiles
* DescribeDBMajorEngineVersions
* DescribeDBParameterGroups
* DescribeDBParameters
* DescribeDBProxies
* DescribeDBProxyEndpoints
* DescribeDBProxyTargetGroups
* DescribeDBProxyTargets
* DescribeDBRecommendations
* DescribeDBSecurityGroups
* DescribeDBShardGroups
* DescribeDBSnapshotAttributes
* DescribeDBSnapshotTenantDatabases
* DescribeDBSnapshots
* DescribeDBSubnetGroups
* DescribeEngineDefaultClusterParameters
* DescribeEngineDefaultParameters
* DescribeEventCategories
* DescribeEventSubscriptions
* DescribeEvents
* DescribeExportTasks
* DescribeGlobalClusters
* DescribeIntegrations
* DescribeOptionGroupOptions
* DescribeOptionGroups
* DescribeOrderableDBInstanceOptions
* DescribePendingMaintenanceActions
* DescribeReservedDBInstances
* DescribeReservedDBInstancesOfferings
* DescribeSourceRegions
* DescribeTenantDatabases
* DescribeValidDBInstanceModifications
* DisableHttpEndpoint
* DownloadDBLogFilePortion
* EnableHttpEndpoint
* FailoverDBCluster
* ListTagsForResource
* ModifyCertificates
* ModifyDBCluster
* ModifyDBClusterEndpoint
* ModifyDBClusterParameterGroup
* ModifyDBClusterSnapshotAttribute
* ModifyDBInstance
* ModifyDBParameterGroup
* ModifyDBProxy
* ModifyDBProxyEndpoint
* ModifyDBProxyTargetGroup
* ModifyDBRecommendation
* ModifyDBShardGroup
* ModifyDBSnapshot
* ModifyDBSnapshotAttribute
* ModifyDBSubnetGroup
* ModifyEventSubscription
* ModifyGlobalCluster
* ModifyIntegration
* ModifyOptionGroup
* RebootDBCluster
* RebootDBInstance
* RebootDBShardGroup
* RegisterDBProxyTargets
* RemoveRoleFromDBCluster
* RemoveSourceIdentifierFromSubscription
* RemoveTagsFromResource
* ResetDBClusterParameterGroup
* ResetDBParameterGroup
* RestoreDBClusterFromSnapshot
* RestoreDBClusterToPointInTime
* RestoreDBInstanceFromDBSnapshot
* RestoreDBInstanceToPointInTime
* StartDBCluster
* StartDBInstance
* StartExportTask
* StopDBCluster
* StopDBInstance
