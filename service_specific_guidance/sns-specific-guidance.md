
# Service-specific guidance: Amazon Simple Notification Service


This document outlines service-specific guidance for implementing a data perimeter for Amazon Simple Notification Service (SNS). 

Amazon SNS is a fully managed messaging service that enables you to send messages, notifications, and alerts to a wide range of endpoints. It provides a flexible, scalable, and cost-effective way to distribute messages to multiple subscribers through a publish/subscribe model. SNS supports various communication protocols, including email, SMS, mobile push notifications, and HTTP/HTTPS endpoints, making it easy to integrate with diverse applications and services.


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

Perimeter type applicability: resource perimeter applied on identity.
        
Subscribe allows you to specify an HTTP/HTTPS endpoint, email addresses, and SMS phone numbers as the value for the Endpoint and Protocol request parameters. Because the subsequent requests against the endpoints are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls:

* **Preventative control example:** Consider implementing [sns:Protocol](https://docs.aws.amazon.com/sns/latest/dg/UsingIAMwithSNS.html#w2ab1c11c23c19) and [sns:Endpoint](https://docs.aws.amazon.com/sns/latest/dg/UsingIAMwithSNS.html#w2ab1c11c23c19) in an SCP to help prevent requests to untrusted resources. See [restrict_untrusted_endpoints_scp.json](../service_control_policies/service_specific_controls/restrict_untrusted_endpoints_scp.json) for an example policy.
* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Endpoint](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-sns-topic-subscription.html#cfn-sns-topic-subscription-endpoint) and [Protocol](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-sns-topic-subscription.html#cfn-sns-topic-subscription-protocol) properties that do not belong to your organization for the [AWS::SNS::Subscription](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-sns-subscription.html) resource.
* **Detective control example:** Consider using CloudTrail management events to monitor the [Subscribe](https://docs.aws.amazon.com/sns/latest/api/API_Subscribe.html) API calls in your environment (specifically, the `Endpoint` and `Protocol` request parameters). If necessary, remediate with the responsive controls of your choice.


**Additional consideration 2**

Perimeter type applicability: identity and network perimeter applied on resource.
        
SetTopicAttributes allows you to apply a resource-based policy to grant access to a topic. The service currently does not support RCPs.


If you want to restrict access to trusted identities and expected networks, consider implementing these additional controls:

* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [PolicyDocument](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-sns-topicpolicy.html#cfn-sns-topicpolicy-policydocument) property for the [AWS::SNS::TopicPolicy](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-sns-topicpolicy.html) and [AWS::SNS::TopicInlinePolicy](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-sns-topicinlinepolicy.html) resources that grants permissions to untrusted identities or unexpected networks. You can also enforce the standard [identity perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/resource_based_policies/sns_topic_policy.json#L5) and [network perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/resource_based_policies/sns_topic_policy.json#L33) in the resource-based policies.
* **Detective control example 1:** Consider using [AWS Identity and Access Management Access Analyzer](https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html) external access analyzers to help identify resource types that support resource-based policies in your accounts that are shared with untrusted identities. If necessary, remediate with the responsive controls of your choice. For example, you can add the standard [identity perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/resource_based_policies/sns_topic_policy.json#L5) and [network perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/resource_based_policies/sns_topic_policy.json#L33) to the resource-based policy.
* **Detective control example 2:** Consider using CloudTrail management events to monitor the [SetTopicAttributes](https://docs.aws.amazon.com/sns/latest/api/API_SetTopicAttributes.html) API calls in your environment (specifically, the [AttributeName](https://docs.aws.amazon.com/sns/latest/api/API_SetTopicAttributes.html#API_SetTopicAttributes_RequestParameters) request parameter when set to "Policy"). If necessary, remediate with the responsive controls of your choice. For example, you can add the standard [identity perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/resource_based_policies/sns_topic_policy.json#L5) and [network perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/resource_based_policies/sns_topic_policy.json#L33) to the resource-based policy.



**Additional consideration 3**

Perimeter type applicability: identity and network perimeter applied on resource.
        
AddPermission allows you to apply a resource-based policy to grant access to a topic. The service currently does not support RCPs.


If you want to restrict access to trusted identities and expected networks, consider implementing these additional controls:

* **Preventative control example**: Consider restricting [AddPermission](https://docs.aws.amazon.com/sns/latest/api/API_AddPermission.html) permissions to administrators only using an SCP. See [restrict_resource_policy_configurations_scp.json](../service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [PolicyDocument](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-sns-topicpolicy.html#cfn-sns-topicpolicy-policydocument) property for the [AWS::SNS::TopicPolicy](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-sns-topicpolicy.html) and [AWS::SNS::TopicInlinePolicy](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-sns-topicinlinepolicy.html) resources that grants permissions to untrusted identities or unexpected networks. You can also enforce the standard [identity perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/resource_based_policies/sns_topic_policy.json#L5) and [network perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/resource_based_policies/sns_topic_policy.json#L33) in the resource-based policies.
* **Detective control example:** Consider using [AWS Identity and Access Management Access Analyzer](https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html) external access analyzers to help identify resource types that support resource-based policies in your accounts that are shared with untrusted identities. If necessary, remediate with the responsive controls of your choice. For example, you can add the standard [identity perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/resource_based_policies/sns_topic_policy.json#L5) and [network perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/resource_based_policies/sns_topic_policy.json#L33) to the resource-based policy.
* **Detective control example:** Consider using CloudTrail management events to monitor the [AddPermission](https://docs.aws.amazon.com/sns/latest/api/API_AddPermission.html) API calls in your environment (specifically, the [AWSAccountId.member.N](https://docs.aws.amazon.com/sns/latest/api/API_AddPermission.html#SNS-AddPermission-request-AWSAccountId) request parameter). If necessary, remediate with the responsive controls of your choice. For example, you can add the standard [identity perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/resource_based_policies/sns_topic_policy.json#L5) and [network perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/resource_based_policies/sns_topic_policy.json#L33) to the resource-based policy.


**Additional consideration 4**

Perimeter type applicability: identity and network perimeter applied on resource.
        
CreateTopic allows you to apply a resource-based policy to grant access to a topic. The service currently does not support RCPs.


If you want to restrict access to trusted identities and expected networks, consider implementing these additional controls:

* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [PolicyDocument](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-sns-topicpolicy.html#cfn-sns-topicpolicy-policydocument) property for the [AWS::SNS::TopicPolicy](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-sns-topicpolicy.html) and [AWS::SNS::TopicInlinePolicy](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-sns-topicinlinepolicy.html) resources that grants permissions to untrusted identities or unexpected networks. You can also enforce the standard [identity perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/resource_based_policies/sns_topic_policy.json#L5) and [network perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/resource_based_policies/sns_topic_policy.json#L33) in the resource-based policies.
* **Detective control example 1:** Consider using [AWS Identity and Access Management Access Analyzer](https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html) external access analyzers to help identify resource types that support resource-based policies in your accounts that are shared with untrusted identities. If necessary, remediate with the responsive controls of your choice. For example, you can add the standard [identity perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/resource_based_policies/sns_topic_policy.json#L5) and [network perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/resource_based_policies/sns_topic_policy.json#L33) to the resource-based policy.
* **Detective control example 2:** Consider using CloudTrail management events to monitor the [CreateTopic](https://docs.aws.amazon.com/sns/latest/api/API_CreateTopic.html) API calls in your environment (specifically, the [Attributes.entry.N.key](https://docs.aws.amazon.com/sns/latest/api/API_CreateTopic.html#API_CreateTopic_RequestParameters) request parameter when set to "Policy"). If necessary, remediate with the responsive controls of your choice. For example, you can add the standard [identity perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/resource_based_policies/sns_topic_policy.json#L5) and [network perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/resource_based_policies/sns_topic_policy.json#L33) to the resource-based policy.


**Additional consideration 5**

Perimeter type applicability: resource perimeter applied on identity.

Subscribe allows you to specify a resource, such as SQS queue and Lambda function, that does not belong to your organization as the value for the Endpoint and Protocol request parameters. Because the subsequent call against the resources is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls:

* **Preventative control example:** Consider implementing [sns:Protocol](https://docs.aws.amazon.com/sns/latest/dg/UsingIAMwithSNS.html#w2ab1c11c23c19) and [sns:Endpoint](https://docs.aws.amazon.com/sns/latest/dg/UsingIAMwithSNS.html#w2ab1c11c23c19) in an SCP to help prevent requests to untrusted resources. See [restrict_untrusted_endpoints_scp.json](../service_control_policies/service_specific_controls/restrict_untrusted_endpoints_scp.json) for an example policy.
* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Endpoint](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-sns-topic-subscription.html#cfn-sns-topic-subscription-endpoint) and [Protocol](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-sns-topic-subscription.html#cfn-sns-topic-subscription-protocol) properties that do not belong to your organization for the [AWS::SNS::Subscription](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-sns-subscription.html) resource.
* **Detective control example:** Consider using CloudTrail management events to monitor the [Subscribe](https://docs.aws.amazon.com/sns/latest/api/API_Subscribe.html) API calls in your environment (specifically, the `Endpoint` and `Protocol` request parameters). If necessary, remediate with the responsive controls of your choice.

**List of service APIs reviewed against data perimeter control objectives**

* AddPermission
* CheckIfPhoneNumberIsOptedOut
* CreatePlatformApplication
* CreatePlatformEndpoint
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
