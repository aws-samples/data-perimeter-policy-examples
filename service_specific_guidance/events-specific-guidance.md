# Service-specific guidance: Amazon EventBridge


This document outlines service-specific guidance for implementing a data perimeter for Amazon EventBridge.

Amazon EventBridge is a serverless service that uses events to connect application components together, making it easier for you to build scalable event-driven applications. It provides simple and consistent ways to ingest, filter, transform, and deliver events via event buses, pipes, and the EventBridge Scheduler.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | Y |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | Y |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | N |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | N |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | Y |

*Y - Additional considerations apply. N - No additional considerations apply.

## CreateApiDestination
### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [CreateApiDestination](https://docs.aws.amazon.com/eventbridge/latest/APIReference/API_CreateApiDestination.html) allows you to specify an HTTP invocation endpoint as the value for the `InvocationEndpoint` request parameter. Because the subsequent requests against the HTTP invocation endpoint are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [InvocationEndpoint](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-events-apidestination.html#cfn-events-apidestination-invocationendpoint) property that does not belong to your organization for the [AWS::Events::ApiDestination](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-events-apidestination.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateApiDestination](https://docs.aws.amazon.com/eventbridge/latest/APIReference/API_CreateApiDestination.html) API calls in your environment (specifically, the [InvocationEndpoint](https://docs.aws.amazon.com/eventbridge/latest/APIReference/API_CreateApiDestination.html#eventbridge-CreateApiDestination-request-InvocationEndpoint) request parameter). If necessary, remediate with the responsive controls of your choice.

## PutPermission
### No RCP support

**Perimeter type applicability**: identity and network perimeter applied on resource.

**Description**: [PutPermission](https://docs.aws.amazon.com/eventbridge/latest/APIReference/API_PutPermission.html) allows you to apply a resource-based policy to grant access to an event bus. The service currently does not support RCPs.

**Additional controls**:

If you want to restrict access to trusted identities, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutPermission](https://docs.aws.amazon.com/eventbridge/latest/APIReference/API_PutPermission.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Policy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-events-eventbus.html#cfn-events-eventbus-policy) property for the [AWS::Events::EventBus](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-events-eventbus.html) resource that grants permissions to untrusted identities.
* Detective control example: Consider using CloudTrail management events to monitor the [PutPermission](https://docs.aws.amazon.com/eventbridge/latest/APIReference/API_PutPermission.html) API calls in your environment (specifically, the [Policy](https://docs.aws.amazon.com/eventbridge/latest/APIReference/API_PutPermission.html#eventbridge-PutPermission-request-Policy) and [Principal](https://docs.aws.amazon.com/eventbridge/latest/APIReference/API_PutPermission.html#eventbridge-PutPermission-request-Principal) request parameters). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutPermission](https://docs.aws.amazon.com/eventbridge/latest/APIReference/API_PutPermission.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Policy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-events-eventbus.html#cfn-events-eventbus-policy) property for the [AWS::Events::EventBus](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-events-eventbus.html) resource that grants permissions to unexpected networks.
* Detective control example: Consider using CloudTrail management events to monitor the [PutPermission](https://docs.aws.amazon.com/eventbridge/latest/APIReference/API_PutPermission.html) API calls in your environment (specifically, the [Policy](https://docs.aws.amazon.com/eventbridge/latest/APIReference/API_PutPermission.html#eventbridge-PutPermission-request-Policy) and [Principal](https://docs.aws.amazon.com/eventbridge/latest/APIReference/API_PutPermission.html#eventbridge-PutPermission-request-Principal) request parameters). If necessary, remediate with the responsive controls of your choice.

## PutTargets
### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [PutTargets](https://docs.aws.amazon.com/eventbridge/latest/APIReference/API_PutTargets.html) allows you to specify API destinations as the value for the `Targets` request parameter. Because the subsequent requests against the API destinations are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Preventative control example: Consider implementing `events:TargetArn` in an SCP to help prevent requests to untrusted resources. See [restrict_untrusted_endpoints_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_untrusted_endpoints_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Targets](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-events-rule.html#cfn-events-rule-targets) property that does not belong to your organization for the [AWS::Events::Rule](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-events-rule.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [PutTargets](https://docs.aws.amazon.com/eventbridge/latest/APIReference/API_PutTargets.html) API calls in your environment (specifically, the [Targets](https://docs.aws.amazon.com/eventbridge/latest/APIReference/API_PutTargets.html#eventbridge-PutTargets-request-Targets) request parameter). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

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
