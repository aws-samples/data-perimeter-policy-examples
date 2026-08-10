# Service-specific guidance: Amazon CloudWatch Logs


This document outlines service-specific guidance for implementing a data perimeter for Amazon CloudWatch Logs.

Amazon CloudWatch Logs lets you monitor, store, and access log files from Amazon EC2 instances, AWS CloudTrail, Route 53, and other sources, centralizing logs from all of your systems, applications, and AWS services in a single, highly scalable service. You can view, search, filter, and archive logs and query them with a powerful query language, and generate metrics from log data.

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

## CreateExportTask
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [CreateExportTask](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_CreateExportTask.html) allows you to specify an S3 bucket that does not belong to your organization as the value for the `destination` request parameter. Because the subsequent call against the S3 bucket is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Preventative control example: Consider implementing `aws:ResourceOrgID` in an SCP to restrict service API calls so that your identities can only access trusted resources. See [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json) for an example policy. The service uses forward access sessions to validate that the calling principal has permission to access the S3 bucket as part of the API call.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateExportTask](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_CreateExportTask.html) API calls in your environment (specifically, the [destination](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_CreateExportTask.html#CWL-CreateExportTask-request-destination) request parameter). If necessary, remediate with the responsive controls of your choice.

## PutSubscriptionFilter
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [PutSubscriptionFilter](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_PutSubscriptionFilter.html) allows you to specify a destination that does not belong to your organization as the value for the `destinationArn` request parameter. Because the subsequent call against the destination is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [DestinationArn](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-logs-subscriptionfilter.html#cfn-logs-subscriptionfilter-destinationarn) property that does not belong to your organization for the [AWS::Logs::SubscriptionFilter](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-logs-subscriptionfilter.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [PutSubscriptionFilter](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_PutSubscriptionFilter.html) API calls in your environment (specifically, the [destinationArn](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_PutSubscriptionFilter.html#CWL-PutSubscriptionFilter-request-destinationArn) request parameter). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

* AssociateKmsKey
* CancelExportTask
* CreateDelivery
* CreateExportTask
* CreateLogAnomalyDetector
* CreateLogGroup
* CreateLogStream
* DeleteAccountPolicy
* DeleteDataProtectionPolicy
* DeleteDelivery
* DeleteDeliveryDestination
* DeleteDeliveryDestinationPolicy
* DeleteDeliverySource
* DeleteDestination
* DeleteIndexPolicy
* DeleteIntegration
* DeleteLogAnomalyDetector
* DeleteLogGroup
* DeleteLogStream
* DeleteMetricFilter
* DeleteQueryDefinition
* DeleteResourcePolicy
* DeleteRetentionPolicy
* DeleteSubscriptionFilter
* DeleteTransformer
* DescribeConfigurationTemplates
* DescribeDeliveries
* DescribeDeliveryDestinations
* DescribeDeliverySources
* DescribeDestinations
* DescribeExportTasks
* DescribeFieldIndexes
* DescribeIndexPolicies
* DescribeLogGroups
* DescribeLogStreams
* DescribeMetricFilters
* DescribeQueries
* DescribeQueryDefinitions
* DescribeResourcePolicies
* DescribeSubscriptionFilters
* DisassociateKmsKey
* FilterLogEvents
* GetDataProtectionPolicy
* GetDelivery
* GetDeliveryDestination
* GetDeliveryDestinationPolicy
* GetDeliverySource
* GetIntegration
* GetLogAnomalyDetector
* GetLogEvents
* GetLogGroupFields
* GetQueryResults
* GetTransformer
* ListAnomalies
* ListIntegrations
* ListLogAnomalyDetectors
* ListLogGroups
* ListLogGroupsForQuery
* ListTagsForResource
* ListTagsLogGroup
* PutAccountPolicy
* PutDataProtectionPolicy
* PutDeliveryDestination
* PutDeliveryDestinationPolicy
* PutDeliverySource
* PutDestination
* PutDestinationPolicy
* PutIndexPolicy
* PutIntegration
* PutLogEvents
* PutMetricFilter
* PutQueryDefinition
* PutResourcePolicy
* PutRetentionPolicy
* PutSubscriptionFilter
* PutTransformer
* StartLiveTail
* StartQuery
* TagLogGroup
* TagResource
* TestMetricFilter
* TestTransformer
* UntagLogGroup
* UntagResource
* UpdateDeliveryConfiguration
* UpdateLogAnomalyDetector
