# Service-specific guidance: Amazon Managed Streaming for Apache Kafka


This document outlines service-specific guidance for implementing a data perimeter for Amazon Managed Streaming for Apache Kafka.

Amazon Managed Streaming for Apache Kafka (Amazon MSK) is a fully managed service that makes it easy to build and run applications using Apache Kafka to process streaming data. It provides the control-plane operations, such as creating, updating, and deleting clusters, while automating complex Apache Kafka administrative tasks like broker node replacement and software upgrades. Amazon MSK enables you to quickly set up, scale, and manage Apache Kafka clusters in the cloud without the need for Apache Kafka infrastructure management expertise.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | Y |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | N |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | N |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | Y |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | Y |

*Y - Additional considerations apply. N - No additional considerations apply.

## PutClusterPolicy
### No RCP support

**Perimeter type applicability**: identity and network perimeter applied on resource.

**Description**: [PutClusterPolicy](https://docs.aws.amazon.com/msk/1.0/apireference/clusters-clusterarn-policy.html#PutClusterPolicy) allows you to apply a resource-based policy to grant access to a cluster. The service currently does not support RCPs.

**Additional controls**:

If you want to restrict access to trusted identities, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutClusterPolicy](https://docs.aws.amazon.com/msk/1.0/apireference/clusters-clusterarn-policy.html#PutClusterPolicy) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [`Policy`](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-msk-clusterpolicy.html#cfn-msk-clusterpolicy-policy) property for the [`AWS::MSK::ClusterPolicy`](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-msk-clusterpolicy.html) resource that grants permissions to untrusted identities.
* Detective control example: Consider using CloudTrail management events to monitor the [PutClusterPolicy](https://docs.aws.amazon.com/msk/1.0/apireference/clusters-clusterarn-policy.html#PutClusterPolicy) API calls in your environment (specifically, the [Policy](https://docs.aws.amazon.com/msk/1.0/apireference/clusters-clusterarn-policy.html#clusters-clusterarn-policy-schemas) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutClusterPolicy](https://docs.aws.amazon.com/msk/1.0/apireference/clusters-clusterarn-policy.html#PutClusterPolicy) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [`Policy`](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-msk-clusterpolicy.html#cfn-msk-clusterpolicy-policy) property for the [`AWS::MSK::ClusterPolicy`](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-msk-clusterpolicy.html) resource that grants permissions to unexpected networks.
* Detective control example: Consider using CloudTrail management events to monitor the [PutClusterPolicy](https://docs.aws.amazon.com/msk/1.0/apireference/clusters-clusterarn-policy.html#PutClusterPolicy) API calls in your environment (specifically, the [policy](https://docs.aws.amazon.com/msk/1.0/apireference/clusters-clusterarn-policy.html#clusters-clusterarn-policy-schemas) request parameter). If necessary, remediate with the responsive controls of your choice.

## kafka-cluster:*
### IAM authentication in VPCs

**Perimeter type applicability**: network perimeter.

**Description**: [kafka-cluster IAM actions](https://docs.aws.amazon.com/service-authorization/latest/reference/list_kafka-cluster.html) are used to access clusters in your VPCs with IAM authentication.

**Additional controls**:

If you want to restrict access to expected networks for your identities:
* Preventative control example: Consider implementing the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json), which lists [kafka-cluster:*](https://docs.aws.amazon.com/service-authorization/latest/reference/list_kafka-cluster.html) in the `NotAction` element. See [Services and actions that require an exception to the network perimeter](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies#services-and-actions-that-require-an-exception-to-the-network-perimeter) for other IAM actions that should be listed in the `NotAction` when enforcing network perimeter controls across a broad set of services.

If you want to restrict access to your resources to expected networks, consider implementing this additional control:
* Preventative control example: Consider implementing standard infrastructure security controls to restrict which networks and IP addresses can access MSK clusters. These controls include [security groups](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html), [network access control lists](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html), and firewalls such as [AWS Network Firewall](https://aws.amazon.com/network-firewall/).

## List of service APIs reviewed against data perimeter control objectives

* BatchAssociateScramSecret
* BatchDisassociateScramSecret
* CreateCluster
* CreateClusterV2
* CreateConfiguration
* CreateVpcConnection
* DeleteCluster
* DeleteClusterPolicy
* DeleteConfiguration
* DeleteVpcConnection
* DescribeCluster
* DescribeClusterOperation
* DescribeClusterOperationV2
* DescribeClusterV2
* DescribeConfiguration
* DescribeConfigurationRevision
* DescribeReplicator
* DescribeVpcConnection
* GetBootstrapBrokers
* GetClusterPolicy
* GetCompatibleKafkaVersions
* ListClientVpcConnections
* ListClusterOperations
* ListClusterOperationsV2
* ListClusters
* ListClustersV2
* ListConfigurationRevisions
* ListConfigurations
* ListKafkaVersions
* ListNodes
* ListReplicators
* ListScramSecrets
* ListTagsForResource
* ListVpcConnections
* PutClusterPolicy
* RebootBroker
* TagResource
* UntagResource
* UpdateClusterConfiguration
* UpdateConfiguration
* UpdateMonitoring
* UpdateSecurity
