
# Service-specific guidance: Amazon DynamoDB


This document outlines service-specific guidance for implementing a data perimeter for Amazon DynamoDB. 

Amazon DynamoDB is a fully managed NoSQL database service that provides fast and predictable performance with seamless scalability. It allows you to store and retrieve any amount of data, and serve any level of request traffic. DynamoDB offers built-in security, backup and restore, and in-memory caching for internet-scale applications.


The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type | Security objective | Applied on | Policy type | Additional considerations |
|----------------|-------------------|------------|-------------|------------------------|
| Identity perimeter | Only trusted identities can access my resources | Resource | RCP | Y |
| Identity perimeter | Only trusted identities are allowed from my network | Network | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources | Identity | SCP | N |
| Resource perimeter | Only trusted resources can be accessed from my network | Network | VPC endpoint policy | N |
| Network perimeter | My identities can access resources only from expected networks | Identity | SCP | N |
| Network perimeter | My resources can be accessed only from expected networks | Resource | RCP | Y |

*Y – Additional considerations apply. N – No additional considerations apply.
 



**Additional consideration 1**

Perimeter type applicability: identity and network perimeter applied on resource.
        
PutResourcePolicy allows you to apply a resource-based policy to grant access to a table or stream. The service currently doesn’t support RCPs.

If you want to restrict access to trusted identities and expected networks, consider implementing these additional controls:

* **Preventative control example**: Consider restricting [PutResourcePolicy](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutResourcePolicy.html) permissions to administrators only using an SCP. See [restrict_resource_policy_configurations_scp.json](../service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* **Proactive control example 1:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [ResourcePolicy](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-dynamodb-table.html#cfn-dynamodb-table-resourcepolicy) property for the [AWS::DynamoDB::Table](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-dynamodb-table.html) resource that grants permissions to untrusted identities or unexpected networks. 
* **Proactive control example 2:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [ResourcePolicy](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-dynamodb-globaltable-replicaspecification.html#cfn-dynamodb-globaltable-replicaspecification-resourcepolicy) property for the [AWS::DynamoDB::GlobalTable](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-dynamodb-globaltable.html) resource that grants permissions to untrusted identities or unexpected networks. 
* **Detective control example 1:** Consider using [AWS Identity and Access Management Access Analyzer](https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html) external access analyzers to help identify resource types that support resource-based policies in your accounts that are shared with untrusted identities. If necessary, remediate with the responsive controls of your choice. 
* **Detective control example 2:** Consider using CloudTrail management events to monitor the [PutResourcePolicy](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutResourcePolicy.html) API calls in your environment (specifically, the [Policy](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_PutResourcePolicy.html#DDB-PutResourcePolicy-request-Policy) request parameter). If necessary, remediate with the responsive controls of your choice. 


**Additional consideration 2**

Perimeter type applicability: identity and network perimeter applied on resource.
        
CreateTable allows you to apply a resource-based policy to grant access to a table. The service currently doesn’t support RCPs.

If you want to restrict access to trusted identities and expected networks, consider implementing these additional controls:

* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [ResourcePolicy](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-dynamodb-table.html#cfn-dynamodb-table-resourcepolicy) property for the [AWS::DynamoDB::Table](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-dynamodb-table.html) resource that grants permissions to untrusted identities or unexpected networks. 
* **Detective control example 1:** Consider using [AWS Identity and Access Management Access Analyzer](https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html) external access analyzers to help identify resource types that support resource-based policies in your accounts that are shared with untrusted identities. If necessary, remediate with the responsive controls of your choice. 
* **Detective control example 2:** Consider using CloudTrail management events to monitor the [CreateTable](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_CreateTable.html) API calls in your environment (specifically, the [ResourcePolicy](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_CreateTable.html#DDB-CreateTable-request-ResourcePolicy) request parameter). 


**List of service APIs reviewed against data perimeter control objectives**

* BatchExecuteStatement
* BatchGetItem
* BatchWriteItem
* CreateBackup
* CreateGlobalTable
* CreateTable
* DeleteBackup
* DeleteItem
* DeleteResourcePolicy
* DeleteTable
* DescribeBackup
* DescribeContinuousBackups
* DescribeContributorInsights
* DescribeEndpoints
* DescribeExport
* DescribeGlobalTable
* DescribeGlobalTableSettings
* DescribeImport
* DescribeKinesisStreamingDestination
* DescribeLimits
* DescribeTable
* DescribeTableReplicaAutoScaling
* DescribeTimeToLive
* DisableKinesisStreamingDestination
* EnableKinesisStreamingDestination
* ExecuteStatement
* ExecuteTransaction
* ExportTableToPointInTime
* GetItem
* GetResourcePolicy
* ImportTable
* ListBackups
* ListContributorInsights
* ListExports
* ListGlobalTables
* ListImports
* ListTables
* ListTagsOfResource
* PutItem
* PutResourcePolicy
* Query
* RestoreTableFromBackup
* RestoreTableToPointInTime
* Scan
* TagResource
* TransactGetItems
* TransactWriteItems
* UntagResource
* UpdateContinuousBackups
* UpdateContributorInsights
* UpdateGlobalTable
* UpdateGlobalTableSettings
* UpdateItem
* UpdateTable
* UpdateTableReplicaAutoScaling
* UpdateTimeToLive


