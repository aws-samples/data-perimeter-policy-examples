
# Service-specific guidance: Amazon Managed Service for Prometheus


This document outlines service-specific guidance for implementing a data perimeter for Amazon Managed Service for Prometheus. 

Amazon Managed Service for Prometheus is a fully managed monitoring service that makes it easy to monitor containerized applications and infrastructure at scale. It provides a highly available, secure, and managed environment for Prometheus, an open-source monitoring and alerting tool, allowing users to collect, store, and analyze metrics from their applications and infrastructure without the need to manage the underlying infrastructure.


The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type | Security objective | Applied on | Policy type | Additional considerations |
|----------------|-------------------|------------|-------------|------------------------|
| Identity perimeter | Only trusted identities can access my resources | Resource | RCP | N |
| Identity perimeter | Only trusted identities are allowed from my network | Network | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources | Identity | SCP | N |
| Resource perimeter | Only trusted resources can be accessed from my network | Network | VPC endpoint policy | N |
| Network perimeter | My identities can access resources only from expected networks | Identity | SCP | N |
| Network perimeter | My resources can be accessed only from expected networks | Resource | RCP | N |

*Y – Additional considerations apply. N – No additional considerations apply.

**List of service APIs reviewed against data perimeter control objectives**

* CreateLoggingConfiguration
* CreateRuleGroupsNamespace
* CreateScraper
* CreateWorkspace
* DeleteLoggingConfiguration
* DeleteRuleGroupsNamespace
* DeleteScraper
* DeleteWorkspace
* DescribeLoggingConfiguration
* DescribeRuleGroupsNamespace
* DescribeScraper
* DescribeWorkspace
* GetDefaultScraperConfiguration
* ListRuleGroupsNamespaces
* ListScrapers
* ListTagsForResource
* ListWorkspaces
* PutRuleGroupsNamespace
* TagResource
* UntagResource
* UpdateLoggingConfiguration
* UpdateWorkspaceAlias
