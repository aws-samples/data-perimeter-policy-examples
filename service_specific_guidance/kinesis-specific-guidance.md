# Service-specific guidance: Amazon Kinesis Data Streams


This document outlines service-specific guidance for implementing a data perimeter for Amazon Kinesis Data Streams.

Amazon Kinesis is a managed service that enables real-time processing and analysis of streaming data at scale. It allows you to collect, process, and analyze large volumes of data from various sources such as IoT devices, logs, and social media feeds in near real-time. Kinesis provides multiple capabilities including data streams, data firehose, and data analytics, making it easier for developers to build applications that can react to incoming data quickly and efficiently.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | Y |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | N |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | N |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | N |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | Y |

*Y - Additional considerations apply. N - No additional considerations apply.

## PutResourcePolicy
### No RCP support

**Perimeter type applicability**: identity and network perimeter applied on resource.

**Description**: [PutResourcePolicy](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_PutResourcePolicy.html) allows you to apply a resource-based policy to grant access to a data stream or registered consumer. The service currently does not support RCPs.

**Additional controls**:

If you want to restrict access to trusted identities, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutResourcePolicy](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_PutResourcePolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [ResourcePolicy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-kinesis-resourcepolicy.html#cfn-kinesis-resourcepolicy-resourcepolicy) property for the [AWS::Kinesis::ResourcePolicy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-kinesis-resourcepolicy.html) resource that grants permissions to untrusted identities.
* Detective control example: Consider using CloudTrail management events to monitor the [PutResourcePolicy](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_PutResourcePolicy.html) API calls in your environment (specifically, the [Policy](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_PutResourcePolicy.html#Streams-PutResourcePolicy-request-Policy) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutResourcePolicy](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_PutResourcePolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [ResourcePolicy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-kinesis-resourcepolicy.html#cfn-kinesis-resourcepolicy-resourcepolicy) property for the [AWS::Kinesis::ResourcePolicy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-kinesis-resourcepolicy.html) resource that grants permissions to unexpected networks.
* Detective control example: Consider using CloudTrail management events to monitor the [PutResourcePolicy](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_PutResourcePolicy.html) API calls in your environment (specifically, the [Policy](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_PutResourcePolicy.html#Streams-PutResourcePolicy-request-Policy) request parameter). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

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
* ListTagsForResource
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
* TagResource
* UntagResource
* UpdateMaxRecordSize
* UpdateShardCount
* UpdateStreamMode
