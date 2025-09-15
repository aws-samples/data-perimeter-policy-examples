# Service-specific guidance: Amazon Kinesis


This document outlines service-specific guidance for implementing a data perimeter for Amazon Kinesis. 


Amazon Kinesis is a managed service that enables real-time processing and analysis of streaming data at scale. It allows you to collect, process, and analyze large volumes of data from various sources such as IoT devices, logs, and social media feeds in near real-time. Kinesis provides multiple capabilities including data streams, data firehose, and data analytics, making it easier for developers to build applications that can react to incoming data quickly and efficiently.


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
* AddTagsToStream
* CreateStream
* DecreaseStreamRetentionPeriod
* DeleteResourcePolicy
* DeleteStream
* DeregisterStreamConsumer
* DescribeLimits
* DescribeStream
* DescribeStreamConsumer
* DescribeStreamSummary
* DisableEnhancedMonitoring
* EnableEnhancedMonitoring
* GetRecords
* GetResourcePolicy
* GetShardIterator
* IncreaseStreamRetentionPeriod
* ListShards
* ListStreamConsumers
* ListStreams
* ListTagsForStream
* PutRecord
* PutRecords
* PutResourcePolicy
* RegisterStreamConsumer
* RemoveTagsFromStream
* SplitShard
* StartStreamEncryption
* StopStreamEncryption
* SubscribeToShard
* UpdateShardCount
* UpdateStreamMode
