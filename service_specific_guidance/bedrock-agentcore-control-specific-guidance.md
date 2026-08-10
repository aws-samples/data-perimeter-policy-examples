# Service-specific guidance: Amazon Bedrock AgentCore


This document outlines service-specific guidance for implementing a data perimeter for Amazon Bedrock AgentCore.

Amazon Bedrock AgentCore is an agentic platform for building, deploying, and operating highly effective agents securely at scale using any framework and foundation model. AgentCore services work together or independently with any open-source framework and any foundation model, so customers do not have to choose between open-source flexibility and enterprise-grade security and reliability.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | N |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | Y |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | Y |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | Y |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | Y |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | Y |

*Y - Additional considerations apply. N - No additional considerations apply.

## CreateAgentRuntime
### Code execution within a VPC

**Perimeter type applicability**: identity and resource perimeter on network, network perimeter.

**Description**: [CreateAgentRuntime](https://docs.aws.amazon.com/bedrock-agentcore-control/latest/APIReference/API_CreateAgentRuntime.html) allows you to associate an agent runtime with an Amazon VPC to run your code.

**Additional controls**:

If you want to achieve data perimeter control objectives, consider implementing these additional controls:
* Preventative control example: Consider implementing `bedrock-agentcore:subnets` in an SCP to help restrict creation of resources to a customer managed VPC only. See [restrict_nonvpc_deployment_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help ensure that developers specify the `VPC` value for the [NetworkConfiguration.NetworkMode](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-bedrockagentcore-runtime-networkconfiguration.html#cfn-bedrockagentcore-runtime-networkconfiguration-networkmode) property of the [AWS::BedrockAgentCore::Runtime](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-bedrockagentcore-runtime.html) resource.
* Detective control example: Consider implementing the AWS Config rule, [bedrockagentcore-runtime-private-network-required](https://docs.aws.amazon.com/config/latest/developerguide/bedrockagentcore-runtime-private-network-required.html), to help detect misconfigurations or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateAgentRuntime](https://docs.aws.amazon.com/bedrock-agentcore-control/latest/APIReference/API_CreateAgentRuntime.html) API calls in your environment (specifically, the [networkConfiguration](https://docs.aws.amazon.com/bedrock-agentcore-control/latest/APIReference/API_CreateAgentRuntime.html#bedrockagentcorecontrol-CreateAgentRuntime-request-networkConfiguration) request parameter). If necessary, remediate with the responsive controls of your choice.

### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [CreateAgentRuntime](https://docs.aws.amazon.com/bedrock-agentcore-control/latest/APIReference/API_CreateAgentRuntime.html) allows you to specify a service role, referred to as the AgentCore Runtime execution role, for your AgentCore Runtime operations. The service uses the role to make AWS API calls from your code.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json) or [network_perimeter_vpceorgid_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_vpceorgid_scp.json). These policies demonstrate how to use the `dp:include:network` tag set to `true` to restrict calls made by the AgentCore Runtime execution role. If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [CreateAgentRuntime](https://docs.aws.amazon.com/bedrock-agentcore-control/latest/APIReference/API_CreateAgentRuntime.html) allows you to specify an external JWT authorizer as the value for the `authorizerConfiguration` request parameter. Because the subsequent requests against the external JWT authorizer are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [AuthorizerConfiguration](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-bedrockagentcore-runtime-authorizerconfiguration.html) property that does not belong to your organization for the [AWS::BedrockAgentCore::Runtime](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-bedrockagentcore-runtime.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateAgentRuntime](https://docs.aws.amazon.com/bedrock-agentcore-control/latest/APIReference/API_CreateAgentRuntime.html) API calls in your environment (specifically, the [authorizerConfiguration](https://docs.aws.amazon.com/bedrock-agentcore-control/latest/APIReference/API_CreateAgentRuntime.html#bedrockagentcorecontrol-CreateAgentRuntime-request-authorizerConfiguration) request parameter). If necessary, remediate with the responsive controls of your choice.

## CreateBrowser
### Code execution within a VPC

**Perimeter type applicability**: identity and resource perimeter on network, network perimeter.

**Description**: [CreateBrowser](https://docs.aws.amazon.com/bedrock-agentcore-control/latest/APIReference/API_CreateBrowser.html) allows you to associate a custom browser with an Amazon VPC to run your code.

**Additional controls**:

If you want to achieve data perimeter control objectives, consider implementing these additional controls:
* Preventative control example: Consider implementing `bedrock-agentcore:subnets` in an SCP to help restrict creation of resources to a customer managed VPC only. See [restrict_nonvpc_deployment_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help ensure that developers specify the value `VPC` for the [NetworkConfiguration.NetworkMode](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-bedrockagentcore-browsercustom-browsernetworkconfiguration.html#cfn-bedrockagentcore-browsercustom-browsernetworkconfiguration-networkmode) property of the [AWS::BedrockAgentCore::BrowserCustom](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-bedrockagentcore-browsercustom.html) resource.
* Detective control example: Consider implementing the AWS Config rule, [bedrockagentcore-browsercustom-network-mode-not-public](https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html), to help detect misconfigurations or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateBrowser](https://docs.aws.amazon.com/bedrock-agentcore-control/latest/APIReference/API_CreateBrowser.html) API calls in your environment (specifically, the [networkConfiguration](https://docs.aws.amazon.com/bedrock-agentcore-control/latest/APIReference/API_BrowserNetworkConfiguration.html) request parameter). If necessary, remediate with the responsive controls of your choice.

### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [CreateBrowser](https://docs.aws.amazon.com/bedrock-agentcore-control/latest/APIReference/API_CreateBrowser.html) allows you to specify a service role for your AgentCore Browser operations. The service uses the role to make AWS API calls from your code.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json) or [network_perimeter_vpceorgid_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_vpceorgid_scp.json). These policies demonstrate how to use the `dp:include:network` tag set to `true` to restrict calls made by the browser's execution role. If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

## CreateCodeInterpreter
### Code execution within a VPC

**Perimeter type applicability**: identity and resource perimeter on network, network perimeter.

**Description**: [CreateCodeInterpreter](https://docs.aws.amazon.com/bedrock-agentcore-control/latest/APIReference/API_CreateCodeInterpreter.html) allows you to associate a custom code interpreter with an Amazon VPC to run your code.

**Additional controls**:

If you want to achieve data perimeter control objectives, consider implementing these additional controls:
* Preventative control example: Consider implementing `bedrock-agentcore:subnets` in an SCP to help restrict creation of resources to a customer managed VPC only. See [restrict_nonvpc_deployment_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help ensure that developers specify the [NetworkConfiguration.NetworkMode](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-bedrockagentcore-codeinterpretercustom-codeinterpreternetworkconfiguration.html#cfn-bedrockagentcore-codeinterpretercustom-codeinterpreternetworkconfiguration-networkmode) property of the [AWS::BedrockAgentCore::CodeInterpreterCustom](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-bedrockagentcore-codeinterpretercustom.html) resource with a value of `VPC`.
* Detective control example: Consider implementing the AWS Config rule, [bedrockagentcore-codeinterpreter-networkmode-check](https://docs.aws.amazon.com/config/latest/developerguide/bedrockagentcore-codeinterpreter-networkmode-check.html), to help detect misconfigurations or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateCodeInterpreter](https://docs.aws.amazon.com/bedrock-agentcore-control/latest/APIReference/API_CreateCodeInterpreter.html) API calls in your environment (specifically, the [networkConfiguration](https://docs.aws.amazon.com/bedrock-agentcore-control/latest/APIReference/API_CodeInterpreterNetworkConfiguration.html) request parameter). If necessary, remediate with the responsive controls of your choice.

### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [CreateCodeInterpreter](https://docs.aws.amazon.com/bedrock-agentcore-control/latest/APIReference/API_CreateCodeInterpreter.html) allows you to specify a service role, referred to as the code interpreter execution role, for your code interpreter operations. The service uses the role to make AWS API calls from your code.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json) or [network_perimeter_vpceorgid_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_vpceorgid_scp.json). These policies demonstrate how to use the `dp:include:network` tag set to `true` to restrict calls made by the code interpreter execution role. If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

## CreateGateway
### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [CreateGateway](https://docs.aws.amazon.com/bedrock-agentcore-control/latest/APIReference/API_CreateGateway.html) allows you to specify an external JWT authorizer as the value for the `authorizerConfiguration` request parameter. Because the subsequent requests against the external JWT authorizer are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Preventative control example: Consider implementing the `bedrock-agentcore:DiscoveryUrl` condition key in an SCP to help prevent requests to untrusted resources. See [restrict_untrusted_endpoints_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_untrusted_endpoints_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [AuthorizerConfiguration](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-bedrockagentcore-gateway-authorizerconfiguration.html) property that does not belong to your organization for the [AWS::BedrockAgentCore::Gateway](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-bedrockagentcore-gateway.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateGateway](https://docs.aws.amazon.com/bedrock-agentcore-control/latest/APIReference/API_CreateGateway.html) API calls in your environment (specifically, the [authorizerConfiguration](https://docs.aws.amazon.com/bedrock-agentcore-control/latest/APIReference/API_CreateGateway.html#bedrockagentcorecontrol-CreateGateway-request-authorizerConfiguration) request parameter). If necessary, remediate with the responsive controls of your choice.

## CreateGatewayTarget
### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [CreateGatewayTarget](https://docs.aws.amazon.com/bedrock-agentcore-control/latest/APIReference/API_CreateGatewayTarget.html) allows you to specify an HTTP endpoint as the value for the `targetConfiguration` request parameter. Because the subsequent requests against the HTTP endpoint are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [TargetConfiguration](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-bedrockagentcore-gatewaytarget-targetconfiguration.html) property that does not belong to your organization for the [AWS::BedrockAgentCore::GatewayTarget](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-bedrockagentcore-gatewaytarget.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateGatewayTarget](https://docs.aws.amazon.com/bedrock-agentcore-control/latest/APIReference/API_CreateGatewayTarget.html) API calls in your environment (specifically, the [targetConfiguration](https://docs.aws.amazon.com/bedrock-agentcore-control/latest/APIReference/API_CreateGatewayTarget.html#bedrockagentcorecontrol-CreateGatewayTarget-request-targetConfiguration) request parameter). If necessary, remediate with the responsive controls of your choice.

## UpdateAgentRuntime
### Code execution within a VPC

**Perimeter type applicability**: identity and resource perimeter on network, network perimeter.

**Description**: [UpdateAgentRuntime](https://docs.aws.amazon.com/bedrock-agentcore-control/latest/APIReference/API_UpdateAgentRuntime.html) allows you to associate a runtime with an Amazon VPC to run your code.

**Additional controls**:

If you want to achieve data perimeter control objectives, consider implementing these additional controls:
* Preventative control example: Consider implementing `bedrock-agentcore:subnets` in an SCP to help restrict creation of resources to a customer managed VPC only. See [restrict_nonvpc_deployment_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help ensure that developers specify the [NetworkConfiguration.NetworkMode](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-bedrockagentcore-runtime-networkconfiguration.html#cfn-bedrockagentcore-runtime-networkconfiguration-networkmode) (set to `VPC`) property of the [AWS::BedrockAgentCore::Runtime](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-bedrockagentcore-runtime.html) resource.
* Detective control example: Consider implementing the AWS Config rule, [bedrockagentcore-runtime-private-network-required](https://docs.aws.amazon.com/config/latest/developerguide/bedrockagentcore-runtime-private-network-required.html), to help detect misconfigurations or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateAgentRuntime](https://docs.aws.amazon.com/bedrock-agentcore-control/latest/APIReference/API_UpdateAgentRuntime.html) API calls in your environment (specifically, the [networkConfiguration](https://docs.aws.amazon.com/bedrock-agentcore-control/latest/APIReference/API_UpdateAgentRuntime.html#bedrockagentcorecontrol-UpdateAgentRuntime-request-networkConfiguration) request parameter). If necessary, remediate with the responsive controls of your choice.

### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [UpdateAgentRuntime](https://docs.aws.amazon.com/bedrock-agentcore-control/latest/APIReference/API_UpdateAgentRuntime.html) allows you to specify a service role for your AgentCore Runtime operations. The service uses the role to make AWS API calls from your code.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json) or [network_perimeter_vpceorgid_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_vpceorgid_scp.json). These policies demonstrate how to use the `dp:include:network` tag set to `true` to restrict calls made by the AgentCore Runtime execution role. If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [UpdateAgentRuntime](https://docs.aws.amazon.com/bedrock-agentcore-control/latest/APIReference/API_UpdateAgentRuntime.html) allows you to specify an external JWT authorizer as the value for the `authorizerConfiguration` request parameter. Because the subsequent requests against the external JWT authorizer are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [AuthorizerConfiguration](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-bedrockagentcore-runtime-authorizerconfiguration.html) property that does not belong to your organization for the [AWS::BedrockAgentCore::Runtime](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-bedrockagentcore-runtime.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateAgentRuntime](https://docs.aws.amazon.com/bedrock-agentcore-control/latest/APIReference/API_UpdateAgentRuntime.html) API calls in your environment (specifically, the [authorizerConfiguration](https://docs.aws.amazon.com/bedrock-agentcore-control/latest/APIReference/API_UpdateAgentRuntime.html#bedrockagentcorecontrol-UpdateAgentRuntime-request-authorizerConfiguration) request parameter). If necessary, remediate with the responsive controls of your choice.

## UpdateGateway
### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [UpdateGateway](https://docs.aws.amazon.com/bedrock-agentcore-control/latest/APIReference/API_UpdateGateway.html) allows you to specify an external JWT authorizer as the value for the `authorizerConfiguration` request parameter. Because the subsequent requests against the external JWT authorizer are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Preventative control example: Consider implementing `bedrock-agentcore:DiscoveryUrl` in an SCP to help prevent requests to untrusted resources. See [restrict_untrusted_endpoints_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_untrusted_endpoints_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [AuthorizerConfiguration](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-bedrockagentcore-gateway-authorizerconfiguration.html) property that does not belong to your organization for the [AWS::BedrockAgentCore::Gateway](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-bedrockagentcore-gateway.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateGateway](https://docs.aws.amazon.com/bedrock-agentcore-control/latest/APIReference/API_UpdateGateway.html) API calls in your environment (specifically, the [authorizerConfiguration](https://docs.aws.amazon.com/bedrock-agentcore-control/latest/APIReference/API_UpdateGateway.html#bedrockagentcorecontrol-UpdateGateway-request-authorizerConfiguration) request parameter). If necessary, remediate with the responsive controls of your choice.

## UpdateGatewayTarget
### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [UpdateGatewayTarget](https://docs.aws.amazon.com/bedrock-agentcore-control/latest/APIReference/API_UpdateGatewayTarget.html) allows you to specify an HTTP endpoint as the value for the `targetConfiguration` request parameter. Because the subsequent requests against the HTTP endpoint are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [TargetConfiguration](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-bedrockagentcore-gatewaytarget-targetconfiguration.html) property that does not belong to your organization for the [AWS::BedrockAgentCore::GatewayTarget](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-bedrockagentcore-gatewaytarget.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateGatewayTarget](https://docs.aws.amazon.com/bedrock-agentcore-control/latest/APIReference/API_UpdateGatewayTarget.html) API calls in your environment (specifically, the [targetConfiguration](https://docs.aws.amazon.com/bedrock-agentcore-control/latest/APIReference/API_TargetConfiguration.html) request parameter). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

* CreateAgentRuntime
* CreateAgentRuntimeEndpoint
* CreateApiKeyCredentialProvider
* CreateBrowser
* CreateCodeInterpreter
* CreateGateway
* CreateGatewayTarget
* CreateMemory
* CreateOauth2CredentialProvider
* CreateWorkloadIdentity
* DeleteAgentRuntime
* DeleteAgentRuntimeEndpoint
* DeleteApiKeyCredentialProvider
* DeleteBrowser
* DeleteCodeInterpreter
* DeleteGateway
* DeleteGatewayTarget
* DeleteMemory
* DeleteOauth2CredentialProvider
* DeleteWorkloadIdentity
* GetAgentRuntime
* GetAgentRuntimeEndpoint
* GetApiKeyCredentialProvider
* GetBrowser
* GetCodeInterpreter
* GetGateway
* GetGatewayTarget
* GetMemory
* GetOauth2CredentialProvider
* GetTokenVault
* GetWorkloadIdentity
* ListAgentRuntimeEndpoints
* ListAgentRuntimeVersions
* ListAgentRuntimes
* ListApiKeyCredentialProviders
* ListBrowsers
* ListCodeInterpreters
* ListGatewayTargets
* ListGateways
* ListMemories
* ListOauth2CredentialProviders
* ListTagsForResource
* ListWorkloadIdentities
* SetTokenVaultCMK
* TagResource
* UntagResource
* UpdateAgentRuntime
* UpdateAgentRuntimeEndpoint
* UpdateApiKeyCredentialProvider
* UpdateGateway
* UpdateGatewayTarget
* UpdateMemory
* UpdateOauth2CredentialProvider
* UpdateWorkloadIdentity
