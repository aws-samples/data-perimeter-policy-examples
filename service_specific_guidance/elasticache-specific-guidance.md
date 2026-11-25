# Service-specific guidance: Amazon ElastiCache


This document outlines service-specific guidance for implementing a data perimeter for Amazon ElastiCache.

Amazon ElastiCache is a web service that makes it easy to set up, manage, and scale a distributed in-memory data store or cache environment in the cloud. It provides a high-performance, scalable, and cost-effective caching solution that works with the Valkey, Memcached, and Redis OSS engines and can be operated as either a serverless cache or a node-based cluster.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | N |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | N |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | N |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | Y |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | Y |

*Y - Additional considerations apply. N - No additional considerations apply.

## Connect
### IAM authentication in VPCs

**Perimeter type applicability**: network perimeter.

**Description**: [Connect](https://docs.aws.amazon.com/service-authorization/latest/reference/list_elasticache.html#list_elasticache-actions-as-permissions) is used to access caches in your VPCs with IAM authentication.

**Additional controls**:

If you want to restrict access to expected networks for your identities:
* Preventative control example: Consider implementing the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json), which lists [Connect](https://docs.aws.amazon.com/service-authorization/latest/reference/list_elasticache.html#list_elasticache-actions-as-permissions) in the `NotAction` element. See [Services and actions that require an exception to the network perimeter](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies#services-and-actions-that-require-an-exception-to-the-network-perimeter) for other IAM actions that should be listed in the `NotAction` when enforcing network perimeter controls across a broad set of services.

If you want to restrict access to your resources to expected networks, consider implementing this additional control:
* Preventative control example: Consider implementing standard infrastructure security controls to restrict which networks and IP addresses can access serverless caches and node-based clusters (replication groups) in your VPCs. These controls include [security groups](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html), [network access control lists](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html), and firewalls such as [AWS Network Firewall](https://aws.amazon.com/network-firewall/).

## List of service APIs reviewed against data perimeter control objectives

The service APIs covered in this review are based on botocore version [1.43.29](https://github.com/boto/botocore/tree/1.43.29/botocore).

* AddTagsToResource
* CopyServerlessCacheSnapshot
* CreateCacheCluster
* CreateCacheParameterGroup
* CreateCacheSubnetGroup
* CreateGlobalReplicationGroup
* CreateReplicationGroup
* CreateServerlessCache
* CreateServerlessCacheSnapshot
* CreateSnapshot
* CreateUser
* CreateUserGroup
* DeleteCacheCluster
* DeleteCacheParameterGroup
* DeleteCacheSubnetGroup
* DeleteGlobalReplicationGroup
* DeleteReplicationGroup
* DeleteServerlessCache
* DeleteServerlessCacheSnapshot
* DeleteSnapshot
* DeleteUser
* DeleteUserGroup
* DescribeCacheClusters
* DescribeCacheEngineVersions
* DescribeCacheParameterGroups
* DescribeCacheParameters
* DescribeCacheSubnetGroups
* DescribeEngineDefaultParameters
* DescribeEvents
* DescribeGlobalReplicationGroups
* DescribeReplicationGroups
* DescribeReservedCacheNodes
* DescribeReservedCacheNodesOfferings
* DescribeServerlessCacheSnapshots
* DescribeServerlessCaches
* DescribeServiceUpdates
* DescribeSnapshots
* DescribeUpdateActions
* DescribeUserGroups
* DescribeUsers
* ExportServerlessCacheSnapshot
* IncreaseReplicaCount
* ListAllowedNodeTypeModifications
* ListTagsForResource
* ModifyCacheCluster
* ModifyCacheSubnetGroup
* ModifyGlobalReplicationGroup
* ModifyReplicationGroup
* ModifyServerlessCache
* ModifyUser
* ModifyUserGroup
* RemoveTagsFromResource
* ResetCacheParameterGroup
