
# Service-specific guidance: Amazon CloudWatch Logs


This document outlines service-specific guidance for implementing a data perimeter for Amazon CloudWatch Logs. 

Amazon CloudWatch Logs is a fully managed service that enables you to centralize, monitor, and store log files from various AWS resources and applications. It allows you to easily search, analyze, and set alarms on your log data, helping you troubleshoot issues, identify patterns, and gain insights into your system's performance and behavior.


The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type | Security objective | Applied on | Policy type | Additional considerations |
|----------------|-------------------|------------|-------------|------------------------|
| Identity perimeter | Only trusted identities can access my resources | Resource | RCP | Y |
| Identity perimeter | Only trusted identities are allowed from my network | Network | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources | Identity | SCP | Y |
| Resource perimeter | Only trusted resources can be accessed from my network | Network | VPC endpoint policy | N |
| Network perimeter | My identities can access resources only from expected networks | Identity | SCP | N |
| Network perimeter | My resources can be accessed only from expected networks | Resource | RCP | Y |

*Y – Additional considerations apply. N – No additional considerations apply.
 



**Additional consideration 1**

Perimeter type applicability: identity and network perimeter applied on resource.
        
PutResourcePolicy allows you to apply a resource-based policy to grant access to a log group. The service currently does not support RCPs.

If you want to restrict access to trusted identities and expected networks, consider implementing these additional controls:

* **Preventative control example**: Consider restricting [PutResourcePolicy](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_PutResourcePolicy.html) permissions to administrators only using an SCP. See [restrict_resource_policy_configurations_scp.json](../service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [PolicyDocument](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-logs-resourcepolicy.html#cfn-logs-resourcepolicy-policydocument) property for the [AWS::Logs::ResourcePolicy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-logs-resourcepolicy.html) resource that grants permissions to untrusted identities or unexpected networks. 
* **Detective control example:** Consider using CloudTrail management events to monitor the [PutResourcePolicy](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_PutResourcePolicy.html) API calls in your environment (specifically, the [policyDocument](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_PutResourcePolicy.html#API_PutResourcePolicy_RequestSyntax) request parameter). If necessary, remediate with the responsive controls of your choice. 


**Additional consideration 2**

Perimeter type applicability: identity and network perimeter applied on resource.
        
PutDestinationPolicy allows you to apply a resource-based policy to grant access to a destination. The service currently does not support RCPs.


If you want to restrict access to trusted identities and expected networks, consider implementing these additional controls:

* **Preventative control example**: Consider restricting [PutDestinationPolicy](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_PutDestinationPolicy.html) permissions to administrators only using an SCP. See [restrict_resource_policy_configurations_scp.json](../service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [DestinationPolicy](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-logs-destination.html#cfn-logs-destination-destinationpolicy) property for the [AWS::Logs::Destination](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-logs-destination.html) resource that grants permissions to untrusted identities or unexpected networks. 
* **Detective control example:** Consider using CloudTrail management events to monitor the [PutDestinationPolicy](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_PutDestinationPolicy.html) API calls in your environment (specifically, the [accessPolicy](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_PutDestinationPolicy.html#CWL-PutDestinationPolicy-request-accessPolicy) request parameter). If necessary, remediate with the responsive controls of your choice. 


**Additional consideration 3**

Perimeter type applicability: resource perimeter applied on identity.
        
PutSubscriptionFilter allows you to specify a delivery destination, such as Lambda function, that does not belong to your organization as the value for the destinationArn parameter. Because the subsequent call against the destination is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls

* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [DestinationArn](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-logs-subscriptionfilter.html#cfn-logs-subscriptionfilter-destinationarn) property that does not belong to your organization for the [AWS::Logs::SubscriptionFilter](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-logs-subscriptionfilter.html) resource.
* **Detective control example:** Consider using CloudTrail management events to monitor the [PutSubscriptionFilter](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_PutSubscriptionFilter.html) API calls in your environment (specifically, the [destinationArn](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_PutSubscriptionFilter.html#CWL-PutSubscriptionFilter-request-destinationArn) request parameter). If necessary, remediate with the responsive controls of your  choice.


**Additional consideration 4**

Perimeter type applicability: resource perimeter applied on identity.
        
CreateExportTask allows you to specify an S3 bucket that does not belong to your organization as the value for the destination parameter. Because the subsequent PubObject call is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP

If you want to restrict access to trusted resources, consider implementing these additional controls

* **Detective control example:** Consider using CloudTrail management events to monitor the [CreateExportTask](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_CreateExportTask.html) API calls in your environment (specifically, the [destination](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_CreateExportTask.html#CWL-CreateExportTask-request-destination) request parameter). If necessary, remediate with the responsive controls of your  choice.





**List of service APIs reviewed against data perimeter control objectives**

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
* DeleteLogAnomalyDetector
* DeleteLogGroup
* DeleteLogStream
* DeleteMetricFilter
* DeleteQueryDefinition
* DeleteResourcePolicy
* DeleteRetentionPolicy
* DeleteSubscriptionFilter
* DescribeDeliveries
* DescribeDeliveryDestinations
* DescribeDeliverySources
* DescribeDestinations
* DescribeExportTasks
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
* GetLogAnomalyDetector
* GetLogEvents
* GetLogGroupFields
* GetQueryResults
* ListAnomalies
* ListLogAnomalyDetectors
* ListTagsForResource
* ListTagsLogGroup
* PutAccountPolicy
* PutDataProtectionPolicy
* PutDeliveryDestination
* PutDeliveryDestinationPolicy
* PutDeliverySource
* PutDestination
* PutDestinationPolicy
* PutLogEvents
* PutMetricFilter
* PutQueryDefinition
* PutResourcePolicy
* PutRetentionPolicy
* PutSubscriptionFilter
* StartLiveTail
* StartQuery
* TagLogGroup
* TagResource
* TestMetricFilter
* UntagLogGroup
* UntagResource
* UpdateLogAnomalyDetector