# Service-specific guidance: Amazon Simple Notification Service


This document outlines service-specific guidance for implementing a data perimeter for Amazon Simple Notification Service.

Amazon SNS is a fully managed messaging service that enables you to send messages, notifications, and alerts to a wide range of endpoints. It provides a flexible, scalable, and cost-effective way to distribute messages to multiple subscribers through a publish/subscribe model. SNS supports various communication protocols, including email, SMS, mobile push notifications, and HTTP/HTTPS endpoints, making it easy to integrate with diverse applications and services.

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

## CreateSMSSandboxPhoneNumber
### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [CreateSMSSandboxPhoneNumber](https://docs.aws.amazon.com/sns/latest/api/API_CreateSMSSandboxPhoneNumber.html) allows you to specify an SMS phone number as the value for the `PhoneNumber` request parameter. Because the subsequent requests against the SMS phone number are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing this additional control:
* Detective control example: Consider using CloudTrail management events to monitor the [CreateSMSSandboxPhoneNumber](https://docs.aws.amazon.com/sns/latest/api/API_CreateSMSSandboxPhoneNumber.html) API calls in your environment (specifically, the [PhoneNumber](https://docs.aws.amazon.com/sns/latest/api/API_CreateSMSSandboxPhoneNumber.html#API_CreateSMSSandboxPhoneNumber_RequestParameters) request parameter). If necessary, remediate with the responsive controls of your choice.

## Publish
### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [Publish](https://docs.aws.amazon.com/sns/latest/api/API_Publish.html) allows you to specify an SMS phone number as the value for the `PhoneNumber` request parameter. Because the subsequent requests against the SMS phone number are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing this additional control:

* Preventative control example: Consider restricting [Publish](https://docs.aws.amazon.com/sns/latest/api/API_Publish.html) permissions to select principals using an SCP. See [restrict_untrusted_endpoints_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_untrusted_endpoints_scp.json) for an example policy.

## Subscribe
### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [Subscribe](https://docs.aws.amazon.com/sns/latest/api/API_Subscribe.html) allows you to specify an HTTP/HTTPS endpoint, an email address, or an SMS phone number as the value for the `Endpoint` and `Protocol` request parameters. Because the subsequent requests against the endpoints are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Preventative control example: Consider implementing `sns:Endpoint` and `sns:Protocol` in an SCP to help prevent requests to untrusted resources. See [restrict_untrusted_endpoints_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_untrusted_endpoints_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Endpoint](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-sns-subscription.html#cfn-sns-subscription-endpoint) and [Protocol](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-sns-subscription.html#cfn-sns-subscription-protocol) properties that do not belong to your organization for the [AWS::SNS::Subscription](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-sns-subscription.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [Subscribe](https://docs.aws.amazon.com/sns/latest/api/API_Subscribe.html) API calls in your environment (specifically, the [Endpoint](https://docs.aws.amazon.com/sns/latest/api/API_Subscribe.html#API_Subscribe_RequestParameters) and [Protocol](https://docs.aws.amazon.com/sns/latest/api/API_Subscribe.html#API_Subscribe_RequestParameters) request parameters). If necessary, remediate with the responsive controls of your choice.

### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [Subscribe](https://docs.aws.amazon.com/sns/latest/api/API_Subscribe.html) allows you to specify a resource, such as an SQS queue and a Lambda function, that does not belong to your organization as the value for the `Endpoint` and `Protocol` request parameters. Because the subsequent calls against the resources are performed by the service principal, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Preventative control example: Consider implementing `sns:Protocol` and `sns:Endpoint` in an SCP to help prevent requests to untrusted resources. See [restrict_untrusted_endpoints_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_untrusted_endpoints_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Endpoint](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-sns-subscription.html#cfn-sns-subscription-endpoint) and [Protocol](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-sns-subscription.html#cfn-sns-subscription-protocol) properties that do not belong to your organization for the [AWS::SNS::Subscription](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-sns-subscription.html) resource or the [Subscription](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-sns-topic.html#cfn-sns-topic-subscription) property of the [AWS::SNS::Topic](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-sns-topic.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [Subscribe](https://docs.aws.amazon.com/sns/latest/api/API_Subscribe.html) API calls in your environment (specifically, the [Endpoint](https://docs.aws.amazon.com/sns/latest/api/API_Subscribe.html#API_Subscribe_RequestParameters) and [Protocol](https://docs.aws.amazon.com/sns/latest/api/API_Subscribe.html#API_Subscribe_RequestParameters) request parameters). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

The service APIs covered in this review are based on botocore version [1.43.29](https://github.com/boto/botocore/tree/1.43.29/botocore).

* AddPermission
* CheckIfPhoneNumberIsOptedOut
* CreatePlatformApplication
* CreatePlatformEndpoint
* CreateSMSSandboxPhoneNumber
* CreateTopic
* DeleteEndpoint
* DeletePlatformApplication
* DeleteTopic
* GetDataProtectionPolicy
* GetEndpointAttributes
* GetPlatformApplicationAttributes
* GetSMSAttributes
* GetSMSSandboxAccountStatus
* GetSubscriptionAttributes
* GetTopicAttributes
* ListEndpointsByPlatformApplication
* ListOriginationNumbers
* ListPhoneNumbersOptedOut
* ListPlatformApplications
* ListSMSSandboxPhoneNumbers
* ListSubscriptions
* ListSubscriptionsByTopic
* ListTagsForResource
* ListTopics
* OptInPhoneNumber
* Publish
* PublishBatch
* PutDataProtectionPolicy
* RemovePermission
* SetEndpointAttributes
* SetPlatformApplicationAttributes
* SetSMSAttributes
* SetSubscriptionAttributes
* SetTopicAttributes
* Subscribe
* TagResource
* UntagResource
