# Service-specific guidance: Amazon DocumentDB


This document outlines service-specific guidance for implementing a data perimeter for Amazon DocumentDB.

Amazon DocumentDB is a fast, reliable, and fully managed database service. Amazon DocumentDB makes it easy to set up, operate, and scale MongoDB-compatible databases in the cloud.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | Y |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | Y |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | N |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | N |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | N |

*Y - Additional considerations apply. N - No additional considerations apply.

## ModifyDBClusterSnapshotAttribute
### Service sharing mechanism

**Perimeter type applicability**: identity perimeter applied on resource; resource perimeter applied on identity.

**Description**: [ModifyDBClusterSnapshotAttribute](https://docs.aws.amazon.com/documentdb/latest/APIReference/API_ModifyDBClusterSnapshotAttribute.html) allows you to share a cluster snapshot with another account.

**Additional controls**:

If you want to restrict access so that only trusted identities can view information about your resources, consider implementing these additional controls:
* Preventative control example: Consider restricting [ModifyDBClusterSnapshotAttribute](https://docs.aws.amazon.com/documentdb/latest/APIReference/API_ModifyDBClusterSnapshotAttribute.html) permissions to select principals using an SCP. See [data_perimeter_governance_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/data_perimeter_governance_scp.json) for an example policy and ["Sid":"PreventExternalResourceShare"](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies#sidpreventexternalresourceshare) for a list of resources that can be granted cross-account access.
* Detective control example: Consider using CloudTrail management events to monitor the [ModifyDBClusterSnapshotAttribute](https://docs.aws.amazon.com/documentdb/latest/APIReference/API_ModifyDBClusterSnapshotAttribute.html) API calls in your environment (specifically, the [ValuesToAdd](https://docs.aws.amazon.com/documentdb/latest/APIReference/API_ModifyDBClusterSnapshotAttribute.html#API_ModifyDBClusterSnapshotAttribute_RequestParameters) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access so that your identities cannot view resources that were shared with your accounts by untrusted entities, consider implementing this additional control:
* Detective control example: Consider using [DescribeDBClusterSnapshots](https://docs.aws.amazon.com/documentdb/latest/APIReference/API_DescribeDBClusterSnapshots.html) to monitor the cluster snapshots shared with your accounts (specifically, the [DBClusterSnapshots](https://docs.aws.amazon.com/documentdb/latest/APIReference/API_DBClusterSnapshot.html) response parameter). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

* AddSourceIdentifierToSubscription
* AddTagsToResource
* CopyDBClusterParameterGroup
* CopyDBClusterSnapshot
* CreateDBCluster
* CreateDBClusterParameterGroup
* CreateDBClusterSnapshot
* CreateDBInstance
* CreateDBSubnetGroup
* CreateEventSubscription
* CreateGlobalCluster
* DeleteDBCluster
* DeleteDBClusterParameterGroup
* DeleteDBClusterSnapshot
* DeleteDBInstance
* DeleteDBSubnetGroup
* DeleteEventSubscription
* DeleteGlobalCluster
* DescribeCertificates
* DescribeDBClusterParameterGroups
* DescribeDBClusterParameters
* DescribeDBClusterSnapshotAttributes
* DescribeDBClusterSnapshots
* DescribeDBClusters
* DescribeDBEngineVersions
* DescribeDBInstances
* DescribeDBSubnetGroups
* DescribeEngineDefaultClusterParameters
* DescribeEventCategories
* DescribeEventSubscriptions
* DescribeEvents
* DescribeGlobalClusters
* DescribeOrderableDBInstanceOptions
* DescribePendingMaintenanceActions
* FailoverGlobalCluster
* ListTagsForResource
* ModifyDBCluster
* ModifyDBClusterParameterGroup
* ModifyDBClusterSnapshotAttribute
* ModifyDBInstance
* ModifyDBSubnetGroup
* ModifyEventSubscription
* ModifyGlobalCluster
* RemoveSourceIdentifierFromSubscription
* RemoveTagsFromResource
* ResetDBClusterParameterGroup
* RestoreDBClusterFromSnapshot
* RestoreDBClusterToPointInTime
* SwitchoverGlobalCluster
