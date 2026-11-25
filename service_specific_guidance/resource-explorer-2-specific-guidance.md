# Service-specific guidance: AWS Resource Explorer


This document outlines service-specific guidance for implementing a data perimeter for AWS Resource Explorer.

AWS Resource Explorer is a resource search and discovery service that lets you explore your AWS resources, such as Amazon EC2 instances, Amazon Kinesis streams, or Amazon DynamoDB tables, using an internet search engine-like experience. You can search for resources using metadata like names, tags, and IDs, and Resource Explorer works across AWS Regions in your account to simplify cross-Region workloads.

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

* AssociateDefaultView
* BatchGetView
* CreateResourceExplorerSetup
* CreateView
* DeleteResourceExplorerSetup
* DeleteView
* DisassociateDefaultView
* GetDefaultView
* GetIndex
* GetResourceExplorerSetup
* GetServiceIndex
* GetServiceView
* GetView
* ListIndexes
* ListIndexesForMembers
* ListManagedViews
* ListResources
* ListServiceIndexes
* ListServiceViews
* ListStreamingAccessForServices
* ListSupportedResourceTypes
* ListTagsForResource
* ListViews
* Search
* TagResource
* UntagResource
* UpdateView
