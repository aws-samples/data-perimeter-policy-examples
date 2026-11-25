# Service-specific guidance: Amazon Kinesis Video Streams


This document outlines service-specific guidance for implementing a data perimeter for Amazon Kinesis Video Streams.

Amazon Kinesis Video Streams is a fully managed AWS service that you can use to stream live video and other time-serialized data (such as audio, thermal imagery, depth, and RADAR data) from devices to the AWS Cloud. It can store this media for a retention period you choose, encrypting it at rest automatically, for real-time or batch video processing and analytics. It also offers a fully managed WebRTC capability for live peer-to-peer streaming and two-way audio or video between camera IoT devices and web or mobile players.

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

* CreateSignalingChannel
* CreateStream
* DeleteEdgeConfiguration
* DeleteSignalingChannel
* DeleteStream
* DescribeEdgeConfiguration
* DescribeImageGenerationConfiguration
* DescribeMappedResourceConfiguration
* DescribeMediaStorageConfiguration
* DescribeNotificationConfiguration
* DescribeSignalingChannel
* DescribeStream
* DescribeStreamStorageConfiguration
* GetDataEndpoint
* GetSignalingChannelEndpoint
* ListEdgeAgentConfigurations
* ListSignalingChannels
* ListStreams
* ListTagsForResource
* ListTagsForStream
* StartEdgeConfigurationUpdate
* TagResource
* TagStream
* UntagResource
* UntagStream
* UpdateDataRetention
* UpdateImageGenerationConfiguration
* UpdateMediaStorageConfiguration
* UpdateNotificationConfiguration
* UpdateSignalingChannel
* UpdateStream
* UpdateStreamStorageConfiguration
