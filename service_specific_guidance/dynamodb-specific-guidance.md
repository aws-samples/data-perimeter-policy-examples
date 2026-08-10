# Service-specific guidance: Amazon DynamoDB


This document outlines service-specific guidance for implementing a data perimeter for Amazon DynamoDB.

Amazon DynamoDB is a fully managed NoSQL database service that provides fast and predictable performance with seamless scalability. It allows you to store and retrieve any amount of data, and serve any level of request traffic. DynamoDB offers built-in security, backup and restore, and in-memory caching for internet-scale applications.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | N |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | N |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | N |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | N |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | N |

*Y - Additional considerations apply. N - No additional considerations apply.

## List of service APIs reviewed against data perimeter control objectives

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
