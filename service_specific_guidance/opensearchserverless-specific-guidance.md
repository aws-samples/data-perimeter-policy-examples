# Service-specific guidance: Amazon OpenSearch Serverless


This document outlines service-specific guidance for implementing a data perimeter for Amazon OpenSearch Serverless.

Amazon OpenSearch Serverless is an on-demand, serverless option for Amazon OpenSearch Service that eliminates the operational complexity of provisioning, configuring, and tuning OpenSearch clusters. With OpenSearch Serverless, you can search and analyze large volumes of data without managing the underlying infrastructure.

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

* BatchGetCollection
* BatchGetCollectionGroup
* BatchGetEffectiveLifecyclePolicy
* BatchGetLifecyclePolicy
* BatchGetVpcEndpoint
* CreateAccessPolicy
* CreateCollection
* CreateCollectionGroup
* CreateIndex
* CreateLifecyclePolicy
* CreateSecurityConfig
* CreateSecurityPolicy
* DeleteAccessPolicy
* DeleteCollection
* DeleteCollectionGroup
* DeleteIndex
* DeleteLifecyclePolicy
* DeleteSecurityConfig
* DeleteSecurityPolicy
* GetAccessPolicy
* GetAccountSettings
* GetIndex
* GetPoliciesStats
* GetSecurityConfig
* GetSecurityPolicy
* ListAccessPolicies
* ListCollectionGroups
* ListCollections
* ListLifecyclePolicies
* ListSecurityConfigs
* ListSecurityPolicies
* ListTagsForResource
* ListVpcEndpoints
* TagResource
* UntagResource
* UpdateAccessPolicy
* UpdateAccountSettings
* UpdateCollection
* UpdateCollectionGroup
* UpdateIndex
* UpdateLifecyclePolicy
* UpdateSecurityConfig
* UpdateSecurityPolicy
