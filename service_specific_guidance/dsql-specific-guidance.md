# Service-specific guidance: Amazon Aurora DSQL


This document outlines service-specific guidance for implementing a data perimeter for Amazon Aurora DSQL.

Amazon Aurora DSQL is a serverless, distributed relational database service optimized for transactional workloads, offering virtually unlimited scale and PostgreSQL compatibility without requiring infrastructure management. Its active-active highly available architecture provides 99.99% single-Region and 99.999% multi-Region availability.

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

* CreateCluster
* DeleteCluster
* DeleteClusterPolicy
* GetCluster
* GetClusterPolicy
* GetVpcEndpointServiceName
* ListClusters
* ListTagsForResource
* PutClusterPolicy
* TagResource
* UntagResource
* UpdateCluster
