# Service-specific guidance: Amazon Kinesis Firehose


This document outlines service-specific guidance for implementing a data perimeter for Amazon Kinesis Firehose.

Amazon Data Firehose (formerly Amazon Kinesis Data Firehose) is a fully managed service for delivering real-time streaming data to destinations such as Amazon S3, Amazon Redshift, Amazon OpenSearch Service, Amazon OpenSearch Serverless, Splunk, Apache Iceberg Tables, and custom HTTP endpoints or HTTP endpoints owned by supported third-party service providers. You configure your data producers to send data to a Firehose stream, and the service automatically delivers the data to the destination you specify, optionally transforming it before delivery.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | N |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | Y |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | N |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | N |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | N |

*Y - Additional considerations apply. N - No additional considerations apply.

## CreateDeliveryStream
### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [CreateDeliveryStream](https://docs.aws.amazon.com/firehose/latest/APIReference/API_CreateDeliveryStream.html) allows you to specify HTTP endpoints, Splunk HTTP Event Collector (HEC) endpoints, and Snowflake account URLs as the value for the `HttpEndpointDestinationConfiguration.EndpointConfiguration.Url`, `SplunkDestinationConfiguration.HECEndpoint`, and `SnowflakeDestinationConfiguration.AccountUrl` request parameters. Because the subsequent requests against the HTTP endpoints, Splunk HTTP Event Collector (HEC) endpoints, and Snowflake account URLs are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [HttpEndpointDestinationConfiguration.EndpointConfiguration.Url](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-kinesisfirehose-deliverystream-httpendpointconfiguration.html#cfn-kinesisfirehose-deliverystream-httpendpointconfiguration-url), [SplunkDestinationConfiguration.HECEndpoint](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-kinesisfirehose-deliverystream-splunkdestinationconfiguration.html#cfn-kinesisfirehose-deliverystream-splunkdestinationconfiguration-hecendpoint), and [SnowflakeDestinationConfiguration.AccountUrl](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-kinesisfirehose-deliverystream-snowflakedestinationconfiguration.html#cfn-kinesisfirehose-deliverystream-snowflakedestinationconfiguration-accounturl) properties that do not belong to your organization for the [AWS::KinesisFirehose::DeliveryStream](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-kinesisfirehose-deliverystream.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateDeliveryStream](https://docs.aws.amazon.com/firehose/latest/APIReference/API_CreateDeliveryStream.html) API calls in your environment. If necessary, remediate with the responsive controls of your choice.

## UpdateDestination
### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [UpdateDestination](https://docs.aws.amazon.com/firehose/latest/APIReference/API_UpdateDestination.html) allows you to specify HTTPS endpoints, Splunk HTTP Event Collector (HEC) endpoints, and Snowflake account URLs as the value for the `HttpEndpointDestinationUpdate.EndpointConfiguration.Url`, `SplunkDestinationUpdate.HECEndpoint`, and `SnowflakeDestinationUpdate.AccountUrl` request parameters. Because the subsequent requests against the HTTPS endpoints, Splunk HEC endpoints, and Snowflake account URLs are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [HttpEndpointDestinationConfiguration.EndpointConfiguration.Url](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-kinesisfirehose-deliverystream-httpendpointconfiguration.html#cfn-kinesisfirehose-deliverystream-httpendpointconfiguration-url), [SplunkDestinationConfiguration.HECEndpoint](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-kinesisfirehose-deliverystream-splunkdestinationconfiguration.html#cfn-kinesisfirehose-deliverystream-splunkdestinationconfiguration-hecendpoint), and [SnowflakeDestinationConfiguration.AccountUrl](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-kinesisfirehose-deliverystream-snowflakedestinationconfiguration.html#cfn-kinesisfirehose-deliverystream-snowflakedestinationconfiguration-accounturl) properties that do not belong to your organization for the [AWS::KinesisFirehose::DeliveryStream](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-kinesisfirehose-deliverystream.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateDestination](https://docs.aws.amazon.com/firehose/latest/APIReference/API_UpdateDestination.html) API calls in your environment. If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

The service APIs covered in this review are based on botocore version [1.43.29](https://github.com/boto/botocore/tree/1.43.29/botocore).

* CreateDeliveryStream
* DeleteDeliveryStream
* DescribeDeliveryStream
* ListDeliveryStreams
* ListTagsForDeliveryStream
* PutRecord
* PutRecordBatch
* StartDeliveryStreamEncryption
* StopDeliveryStreamEncryption
* TagDeliveryStream
* UntagDeliveryStream
* UpdateDestination
