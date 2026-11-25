# Service-specific guidance: AWS Resource Groups


This document outlines service-specific guidance for implementing a data perimeter for AWS Resource Groups.

AWS Resource Groups lets you organize AWS resources such as Amazon EC2 instances, Amazon RDS databases, and Amazon S3 buckets into groups, so you can manage and automate tasks on large numbers of resources at one time. A resource group is a collection of AWS resources in the same AWS Region that match the group's query, which can be tag-based or based on an AWS CloudFormation stack.

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

The service APIs covered in this review are based on botocore version [1.43.29](https://github.com/boto/botocore/tree/1.43.29/botocore).

* CancelTagSyncTask
* CreateGroup
* DeleteGroup
* GetAccountSettings
* GetGroup
* GetGroupConfiguration
* GetGroupQuery
* GetTagSyncTask
* GetTags
* GroupResources
* ListGroupResources
* ListGroups
* ListTagSyncTasks
* PutGroupConfiguration
* SearchResources
* StartTagSyncTask
* Tag
* UngroupResources
* Untag
* UpdateAccountSettings
* UpdateGroup
* UpdateGroupQuery
