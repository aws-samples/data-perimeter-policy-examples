# Service-specific guidance: Amazon Connect Cases


This document outlines service-specific guidance for implementing a data perimeter for Amazon Connect Cases.

Amazon Connect Cases (called Connect Customer Cases in newer AWS documentation) lets contact center agents track and manage customer issues that require multiple interactions, follow-up tasks, and teams. A case represents a customer issue and records the issue, the steps and interactions taken to resolve it, and the outcome.

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

* BatchGetCaseRule
* BatchGetField
* BatchPutFieldOptions
* CreateCase
* CreateCaseRule
* CreateDomain
* CreateField
* CreateLayout
* CreateRelatedItem
* CreateTemplate
* DeleteCase
* DeleteCaseRule
* DeleteDomain
* DeleteField
* DeleteLayout
* DeleteRelatedItem
* DeleteTemplate
* GetCase
* GetCaseAuditEvents
* GetCaseEventConfiguration
* GetDomain
* GetLayout
* GetTemplate
* ListCaseRules
* ListCasesForContact
* ListDomains
* ListFieldOptions
* ListFields
* ListLayouts
* ListTagsForResource
* ListTemplates
* PutCaseEventConfiguration
* SearchAllRelatedItems
* SearchCases
* SearchRelatedItems
* TagResource
* UntagResource
* UpdateCase
* UpdateCaseRule
* UpdateField
* UpdateLayout
* UpdateRelatedItem
* UpdateTemplate
