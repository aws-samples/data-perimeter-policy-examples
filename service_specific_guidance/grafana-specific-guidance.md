
# Service-specific guidance: Amazon Managed Grafana


This document outlines service-specific guidance for implementing a data perimeter for Amazon Managed Grafana. 

Amazon Managed Grafana is a fully managed service that makes it easy to deploy, operate, and scale Grafana, an open-source analytics and monitoring platform. It allows users to create, explore, and share observability dashboards to visualize and analyze metrics, logs, and traces from various data sources across their applications and infrastructure.


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

* CreateWorkspace
* CreateWorkspaceApiKey
* CreateWorkspaceServiceAccount
* CreateWorkspaceServiceAccountToken
* DeleteWorkspace
* DeleteWorkspaceApiKey
* DeleteWorkspaceServiceAccount
* DeleteWorkspaceServiceAccountToken
* DescribeWorkspace
* DescribeWorkspaceAuthentication
* DescribeWorkspaceConfiguration
* DisassociateLicense
* ListPermissions
* ListTagsForResource
* ListVersions
* ListWorkspaceServiceAccountTokens
* ListWorkspaceServiceAccounts
* ListWorkspaces
* TagResource
* UntagResource
* UpdatePermissions
* UpdateWorkspace
* UpdateWorkspaceAuthentication
* UpdateWorkspaceConfiguration
