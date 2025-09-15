# Service-specific guidance: Amazon Managed Streaming for Apache Kafka


This document outlines service-specific guidance for implementing a data perimeter for Amazon Managed Streaming for Apache Kafka. 


Amazon Managed Streaming for Apache Kafka (Amazon MSK) is a fully managed service that makes it easy to build and run applications using Apache Kafka to process streaming data. It provides the control-plane operations, such as creating, updating, and deleting clusters, while automating complex Apache Kafka administrative tasks like broker node replacement and software upgrades. Amazon MSK enables you to quickly set up, scale, and manage Apache Kafka clusters in the cloud without the need for Apache Kafka infrastructure management expertise.


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
* BatchAssociateScramSecret
* BatchDisassociateScramSecret
* CreateCluster
* CreateClusterV2
* CreateConfiguration
* CreateReplicator
* CreateVpcConnection
* DeleteCluster
* DeleteClusterPolicy
* DeleteConfiguration
* DeleteReplicator
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
* UpdateBrokerCount
* UpdateBrokerStorage
* UpdateBrokerType
* UpdateClusterConfiguration
* UpdateClusterKafkaVersion
* UpdateConfiguration
* UpdateMonitoring
* UpdateReplicationInfo
* UpdateSecurity
* UpdateStorage
