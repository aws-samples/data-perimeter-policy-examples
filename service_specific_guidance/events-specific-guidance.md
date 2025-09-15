# Service-specific guidance: Amazon EventBridge


This document outlines service-specific guidance for implementing a data perimeter for Amazon EventBridge. 


Amazon EventBridge is a serverless event bus service that makes it easy to connect applications together using data from your own applications, integrated Software-as-a-Service (SaaS) applications, and AWS services. EventBridge delivers a stream of real-time data from event sources and routes that data to targets like AWS Lambda. It simplifies the process of building event-driven architectures and allows you to create scalable, loosely coupled applications.


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
        
PutPermission allows you to apply a resource-based policy to grant access to an event bus. The service currently doesn’t support RCPs.

If you want to restrict access to trusted identities and expected networks, consider implementing these additional controls:

* **Preventative control example:** Consider restricting PutPermission permissions to administrators only using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Policy](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-events-eventbus.html#cfn-events-eventbus-policy) property for the [AWS::Events::EventBus](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-events-eventbus.html) resource that grants permissions to untrusted identities or unexpected networks. 
* **Detective control example:** Consider using CloudTrail management events to monitor the [PutPermission](https://docs.aws.amazon.com/eventbridge/latest/APIReference/API_PutPermission.html) API calls in your environment (specifically, the [Policy](https://docs.aws.amazon.com/eventbridge/latest/APIReference/API_PutPermission.html#eventbridge-PutPermission-request-Policy) request parameter). If necessary, remediate with the responsive controls of your choice. 


**Additional consideration 2**

Perimeter type applicability: resource perimeter applied on identity.
        
PutTargets allows you to specify an API endpoint as the value for the Target request parameter. Because the subsequent requests against the endpoints are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls:

* **Preventative control example:** Consider implementing [events:TargetArn](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazoneventbridge.html#amazoneventbridge-policy-keys) in an SCP to help prevent requests to untrusted resources. See [restrict_untrusted_endpoints_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies/service_specific_controls/restrict_untrusted_endpoints_scp.json) for an example policy.
* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Target](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-events-rule.html#cfn-events-rule-targets) property that does not belong to your organization for the [AWS::Events::Rule](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-events-rule.html) resource
* **Detective control example:** Consider using CloudTrail management events to monitor the [PutTargets](https://docs.aws.amazon.com/eventbridge/latest/APIReference/API_PutTargets.html) API calls in your environment (specifically, the [Arn](https://docs.aws.amazon.com/eventbridge/latest/APIReference/API_Target.html#eventbridge-Type-Target-Arn) request parameter). If necessary, remediate with the responsive controls of your choice.


**Additional consideration 3**

Perimeter type applicability: resource perimeter applied on identity.

CreateApiDestination allows you to specify an HTTP invocation endpoint as the value for the InvocationEndpoint request parameter. Because the subsequent requests against the endpoint are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls:

* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [InvocationEndpoint](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-events-apidestination.html#cfn-events-apidestination-invocationendpoint) property that does not belong to your organization for the [AWS::Events::ApiDestination](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-events-apidestination.html) resource
* **Detective control example:** Consider using CloudTrail management events to monitor the [CreateApiDestination](https://docs.aws.amazon.com/eventbridge/latest/APIReference/API_CreateApiDestination.html) API calls in your environment (specifically, the [InvocationEndpoint](https://docs.aws.amazon.com/eventbridge/latest/APIReference/API_CreateApiDestination.html#eventbridge-CreateApiDestination-request-InvocationEndpoint) request parameter). If necessary, remediate with the responsive controls of your choice.

**List of service APIs reviewed against data perimeter control objectives**
* ActivateEventSource
* CancelReplay
* CreateApiDestination
* CreateArchive
* CreateConnection
* CreateEndpoint
* CreateEventBus
* DeactivateEventSource
* DeauthorizeConnection
* DeleteApiDestination
* DeleteArchive
* DeleteConnection
* DeleteEndpoint
* DeleteEventBus
* DeleteRule
* DescribeApiDestination
* DescribeArchive
* DescribeConnection
* DescribeEndpoint
* DescribeEventBus
* DescribeEventSource
* DescribeReplay
* DescribeRule
* DisableRule
* EnableRule
* ListApiDestinations
* ListArchives
* ListConnections
* ListEndpoints
* ListEventBuses
* ListEventSources
* ListReplays
* ListRuleNamesByTarget
* ListRules
* ListTagsForResource
* ListTargetsByRule
* PutEvents
* PutPartnerEvents
* PutPermission
* PutRule
* PutTargets
* RemovePermission
* RemoveTargets
* StartReplay
* TagResource
* TestEventPattern
* UntagResource
* UpdateApiDestination
* UpdateArchive
* UpdateConnection
* UpdateEndpoint
* UpdateEventBus
