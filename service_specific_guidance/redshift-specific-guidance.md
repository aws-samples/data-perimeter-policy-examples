# Service-specific guidance: Amazon Redshift


This document outlines service-specific guidance for implementing a data perimeter for Amazon Redshift.

Amazon Redshift is a fully managed, petabyte-scale data warehouse service in the cloud. Amazon Redshift Serverless lets you access and analyze data without all of the configurations of a provisioned data warehouse.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | Y |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | Y |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | N |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | N |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | Y |

*Y - Additional considerations apply. N - No additional considerations apply.

## AuthorizeDataShare
### Service sharing mechanism

**Perimeter type applicability**: identity perimeter applied on resource; resource perimeter applied on identity.

**Description**: [AuthorizeDataShare](https://docs.aws.amazon.com/redshift/latest/APIReference/API_AuthorizeDataShare.html) allows you to share a datashare with another account.

**Additional controls**:

If you want to restrict access so that only trusted identities can view information about your resources, consider implementing these additional controls:
* Preventative control example: Consider implementing the `redshift:ConsumerIdentifier` condition key in an SCP to help prevent sharing with untrusted identities. See [data_perimeter_governance_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/data_perimeter_governance_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [AuthorizeDataShare](https://docs.aws.amazon.com/redshift/latest/APIReference/API_AuthorizeDataShare.html) API calls in your environment (specifically, the [ConsumerIdentifier](https://docs.aws.amazon.com/redshift/latest/APIReference/API_AuthorizeDataShare.html#API_AuthorizeDataShare_RequestParameters) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access so that your identities cannot view resources that were shared with your accounts by untrusted entities, consider implementing this additional control:
* Detective control example: Consider using [DescribeDataSharesForConsumer](https://docs.aws.amazon.com/redshift/latest/APIReference/API_DescribeDataSharesForConsumer.html) to monitor the datashares shared with your accounts (specifically, the [DataShares](https://docs.aws.amazon.com/redshift/latest/APIReference/API_DataShare.html) response parameter). If necessary, remediate with the responsive controls of your choice.

## AuthorizeEndpointAccess
### Service sharing mechanism

**Perimeter type applicability**: identity perimeter applied on resource; resource perimeter applied on identity.

**Description**: [AuthorizeEndpointAccess](https://docs.aws.amazon.com/redshift/latest/APIReference/API_AuthorizeEndpointAccess.html) allows you to share a cluster with another account.

**Additional controls**:

If you want to restrict access so that only trusted identities can view information about your resources, consider implementing these additional controls:
* Preventative control example: Consider restricting [AuthorizeEndpointAccess](https://docs.aws.amazon.com/redshift/latest/APIReference/API_AuthorizeEndpointAccess.html) permissions to select principals using an SCP. See [data_perimeter_governance_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/data_perimeter_governance_scp.json) for an example policy and ["Sid":"PreventExternalResourceShare"](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies#sidpreventexternalresourceshare) for a list of resources that can be granted cross-account access.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Account](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-redshift-endpointauthorization.html#cfn-redshift-endpointauthorization-account) property that grants permissions to untrusted identities for the [AWS::Redshift::EndpointAuthorization](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-redshift-endpointauthorization.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [AuthorizeEndpointAccess](https://docs.aws.amazon.com/redshift/latest/APIReference/API_AuthorizeEndpointAccess.html) API calls in your environment (specifically, the [Account](https://docs.aws.amazon.com/redshift/latest/APIReference/API_AuthorizeEndpointAccess.html#redshift-AuthorizeEndpointAccess-request-Account) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access so that your identities cannot view resources that were shared with your accounts by untrusted entities, consider implementing this additional control:
* Detective control example: Consider using [DescribeEndpointAuthorization](https://docs.aws.amazon.com/redshift/latest/APIReference/API_DescribeEndpointAuthorization.html) to monitor the clusters shared with your accounts (specifically, the [EndpointAuthorizationList](https://docs.aws.amazon.com/redshift/latest/APIReference/API_EndpointAuthorization.html) response parameter). If necessary, remediate with the responsive controls of your choice.

## AuthorizeSnapshotAccess
### Service sharing mechanism

**Perimeter type applicability**: identity perimeter applied on resource; resource perimeter applied on identity.

**Description**: [AuthorizeSnapshotAccess](https://docs.aws.amazon.com/redshift/latest/APIReference/API_AuthorizeSnapshotAccess.html) allows you to share a cluster snapshot with another account.

**Additional controls**:

If you want to restrict access so that only trusted identities can view information about your resources, consider implementing these additional controls:
* Preventative control example: Consider restricting [AuthorizeSnapshotAccess](https://docs.aws.amazon.com/redshift/latest/APIReference/API_AuthorizeSnapshotAccess.html) permissions to select principals using an SCP. See [data_perimeter_governance_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/data_perimeter_governance_scp.json) for an example policy and ["Sid":"PreventExternalResourceShare"](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies#sidpreventexternalresourceshare) for a list of resources that can be granted cross-account access.
* Detective control example: Consider using CloudTrail management events to monitor the [AuthorizeSnapshotAccess](https://docs.aws.amazon.com/redshift/latest/APIReference/API_AuthorizeSnapshotAccess.html) API calls in your environment (specifically, the [AccountWithRestoreAccess](https://docs.aws.amazon.com/redshift/latest/APIReference/API_AuthorizeSnapshotAccess.html#API_AuthorizeSnapshotAccess_RequestParameters) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access so that your identities cannot view resources that were shared with your accounts by untrusted entities, consider implementing this additional control:
* Detective control example: Consider using [DescribeClusterSnapshots](https://docs.aws.amazon.com/redshift/latest/APIReference/API_DescribeClusterSnapshots.html) to monitor the cluster snapshots shared with your accounts (specifically, the [Snapshots](https://docs.aws.amazon.com/redshift/latest/APIReference/API_Snapshot.html) response parameter). If necessary, remediate with the responsive controls of your choice.

## EnableLogging
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [EnableLogging](https://docs.aws.amazon.com/redshift/latest/APIReference/API_EnableLogging.html) allows you to specify an Amazon S3 bucket that does not belong to your organization as the value for the `BucketName` request parameter. Because the subsequent call against the Amazon S3 bucket is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Preventative control example: Consider implementing `aws:ResourceOrgID` in an SCP to restrict service API calls so that your identities can only access trusted resources. See [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json) for an example policy. The service uses forward access sessions to validate that the calling principal has permission to access the S3 bucket as part of the API call.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [LoggingProperties.BucketName](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-redshift-cluster-loggingproperties.html#cfn-redshift-cluster-loggingproperties-bucketname) property that does not belong to your organization for the [AWS::Redshift::Cluster](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-redshift-cluster.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [EnableLogging](https://docs.aws.amazon.com/redshift/latest/APIReference/API_EnableLogging.html) API calls in your environment (specifically, the [BucketName](https://docs.aws.amazon.com/redshift/latest/APIReference/API_EnableLogging.html#API_EnableLogging_RequestParameters) request parameter). If necessary, remediate with the responsive controls of your choice.

## PutResourcePolicy
### No RCP support

**Perimeter type applicability**: identity and network perimeter applied on resource.

**Description**: [PutResourcePolicy](https://docs.aws.amazon.com/redshift/latest/APIReference/API_PutResourcePolicy.html) allows you to apply a resource-based policy to grant access to a namespace. The service currently does not support RCPs.

**Additional controls**:

If you want to restrict access to trusted identities, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutResourcePolicy](https://docs.aws.amazon.com/redshift/latest/APIReference/API_PutResourcePolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [NamespaceResourcePolicy](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-redshift-cluster.html#cfn-redshift-cluster-namespaceresourcepolicy) property for the [AWS::Redshift::Cluster](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-redshift-cluster.html) resource that grants permissions to untrusted identities.
* Detective control example: Consider using CloudTrail management events to monitor the [PutResourcePolicy](https://docs.aws.amazon.com/redshift/latest/APIReference/API_PutResourcePolicy.html) API calls in your environment (specifically, the [Policy](https://docs.aws.amazon.com/redshift/latest/APIReference/API_PutResourcePolicy.html#API_PutResourcePolicy_RequestParameters) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutResourcePolicy](https://docs.aws.amazon.com/redshift/latest/APIReference/API_PutResourcePolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [NamespaceResourcePolicy](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-redshift-cluster.html#cfn-redshift-cluster-namespaceresourcepolicy) property for the [AWS::Redshift::Cluster](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-redshift-cluster.html) resource that grants permissions to unexpected networks.
* Detective control example: Consider using CloudTrail management events to monitor the [PutResourcePolicy](https://docs.aws.amazon.com/redshift/latest/APIReference/API_PutResourcePolicy.html) API calls in your environment (specifically, the [Policy](https://docs.aws.amazon.com/redshift/latest/APIReference/API_PutResourcePolicy.html#API_PutResourcePolicy_RequestParameters) request parameter). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

* AddPartner
* AssociateDataShareConsumer
* AuthorizeDataShare
* AuthorizeEndpointAccess
* AuthorizeSnapshotAccess
* BatchDeleteClusterSnapshots
* BatchModifyClusterSnapshots
* CreateAuthenticationProfile
* CreateCluster
* CreateClusterParameterGroup
* CreateClusterSubnetGroup
* CreateCustomDomainAssociation
* CreateEndpointAccess
* CreateEventSubscription
* CreateHsmClientCertificate
* CreateRedshiftIdcApplication
* CreateScheduledAction
* CreateSnapshotCopyGrant
* CreateSnapshotSchedule
* CreateTags
* CreateUsageLimit
* DeauthorizeDataShare
* DeleteAuthenticationProfile
* DeleteCluster
* DeleteClusterParameterGroup
* DeleteClusterSubnetGroup
* DeleteCustomDomainAssociation
* DeleteEndpointAccess
* DeleteEventSubscription
* DeleteHsmClientCertificate
* DeleteRedshiftIdcApplication
* DeleteResourcePolicy
* DeleteScheduledAction
* DeleteSnapshotCopyGrant
* DeleteSnapshotSchedule
* DeleteTags
* DeleteUsageLimit
* DeregisterNamespace
* DescribeAccountAttributes
* DescribeAuthenticationProfiles
* DescribeClusterDbRevisions
* DescribeClusterParameterGroups
* DescribeClusterParameters
* DescribeClusterSnapshots
* DescribeClusterSubnetGroups
* DescribeClusterTracks
* DescribeClusterVersions
* DescribeClusters
* DescribeDataShares
* DescribeDataSharesForConsumer
* DescribeDataSharesForProducer
* DescribeDefaultClusterParameters
* DescribeEndpointAccess
* DescribeEndpointAuthorization
* DescribeEventCategories
* DescribeEventSubscriptions
* DescribeEvents
* DescribeHsmClientCertificates
* DescribeHsmConfigurations
* DescribeInboundIntegrations
* DescribeIntegrations
* DescribeLoggingStatus
* DescribeNodeConfigurationOptions
* DescribeOrderableClusterOptions
* DescribeRedshiftIdcApplications
* DescribeReservedNodeOfferings
* DescribeReservedNodes
* DescribeResize
* DescribeScheduledActions
* DescribeSnapshotCopyGrants
* DescribeSnapshotSchedules
* DescribeStorage
* DescribeTableRestoreStatus
* DescribeTags
* DescribeUsageLimits
* DisableLogging
* DisableSnapshotCopy
* DisassociateDataShareConsumer
* EnableLogging
* EnableSnapshotCopy
* GetClusterCredentials
* GetClusterCredentialsWithIAM
* GetResourcePolicy
* ListRecommendations
* ModifyAuthenticationProfile
* ModifyClusterParameterGroup
* ModifyClusterSnapshot
* ModifyClusterSubnetGroup
* ModifyCustomDomainAssociation
* ModifyEndpointAccess
* ModifyEventSubscription
* ModifyRedshiftIdcApplication
* ModifyScheduledAction
* ModifySnapshotCopyRetentionPeriod
* ModifySnapshotSchedule
* ModifyUsageLimit
* PutResourcePolicy
* RebootCluster
* RegisterNamespace
* ResetClusterParameterGroup
* RestoreFromClusterSnapshot
* RestoreTableFromClusterSnapshot
* RevokeSnapshotAccess
