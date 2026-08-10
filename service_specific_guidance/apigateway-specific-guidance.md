# Service-specific guidance: Amazon API Gateway Management


This document outlines service-specific guidance for implementing a data perimeter for Amazon API Gateway Management.

Amazon API Gateway is an AWS service for creating, publishing, maintaining, monitoring, and securing REST, HTTP, and WebSocket APIs at any scale. It acts as a "front door" for applications to access data, business logic, or functionality from backend services running on Lambda, EC2, or any web service.

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

## CreateAuthorizer
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [CreateAuthorizer](https://docs.aws.amazon.com/apigateway/latest/api/API_CreateAuthorizer.html) allows you to specify a Cognito user pool as the value for the `providerARNs` request parameter. Because the subsequent requests against the Cognito user pool are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Preventative control example: Consider implementing `apigateway:Request/AuthorizerType` in an SCP to help prevent requests to untrusted resources. See [restrict_untrusted_resources_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_untrusted_resources_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [ProviderARNs](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-authorizer.html#cfn-apigateway-authorizer-providerarns) property that does not belong to your organization for the [AWS::ApiGateway::Authorizer](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-authorizer.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateAuthorizer](https://docs.aws.amazon.com/apigateway/latest/api/API_CreateAuthorizer.html) API calls in your environment (specifically, the [providerARNs](https://docs.aws.amazon.com/apigateway/latest/api/API_CreateAuthorizer.html#apigw-CreateAuthorizer-request-providerARNs) request parameter). If necessary, remediate with the responsive controls of your choice.

### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [CreateAuthorizer](https://docs.aws.amazon.com/apigateway/latest/api/API_CreateAuthorizer.html) allows you to specify a Lambda function that does not belong to your organization as the value for the `authorizerUri` request parameter. Because the subsequent call against the Lambda function is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Preventative control example: Consider implementing `apigateway:Request/AuthorizerUri` in an SCP to help prevent requests to untrusted resources. See [restrict_untrusted_resources_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_untrusted_resources_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [AuthorizerUri](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-authorizer.html#cfn-apigateway-authorizer-authorizeruri) property that does not belong to your organization for the [AWS::ApiGateway::Authorizer](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-authorizer.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateAuthorizer](https://docs.aws.amazon.com/apigateway/latest/api/API_CreateAuthorizer.html) API calls in your environment (specifically, the [authorizerUri](https://docs.aws.amazon.com/apigateway/latest/api/API_CreateAuthorizer.html#apigw-CreateAuthorizer-request-authorizerUri) request parameter). If necessary, remediate with the responsive controls of your choice.

## CreateDomainName
### No RCP support

**Perimeter type applicability**: identity and network perimeter applied on resource.

**Description**: [CreateDomainName](https://docs.aws.amazon.com/apigateway/latest/api/API_CreateDomainName.html) allows you to apply a resource-based policy to grant access to a domain. The service currently does not support RCPs.

**Additional controls**:

If you want to restrict access to trusted identities, consider implementing these additional controls:
* Preventative control example: Consider restricting [UpdateDomainNamePolicy](https://docs.aws.amazon.com/service-authorization/latest/reference/list_apigateway.html#list_apigateway-action-UpdateDomainNamePolicy) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Policy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-domainnamev2.html#cfn-apigateway-domainnamev2-policy) property for the [AWS::ApiGateway::DomainNameV2](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-domainnamev2.html) resource that grants permissions to untrusted identities. You can also enforce the standard [identity perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/service_specific_controls/api_gateway_policy.json#L5) in the resource-based policies.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateDomainName](https://docs.aws.amazon.com/apigateway/latest/api/API_CreateDomainName.html) API calls in your environment (specifically, the [policy](https://docs.aws.amazon.com/apigateway/latest/api/API_CreateDomainName.html#apigw-CreateDomainName-request-policy) request parameter). If necessary, remediate with the responsive controls of your choice. For example, you can add the standard [identity perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/service_specific_controls/api_gateway_policy.json#L5) to the resource-based policy.

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [UpdateDomainNamePolicy](https://docs.aws.amazon.com/service-authorization/latest/reference/list_apigateway.html#list_apigateway-action-UpdateDomainNamePolicy) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Policy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-domainnamev2.html#cfn-apigateway-domainnamev2-policy) property for the [AWS::ApiGateway::DomainNameV2](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-domainnamev2.html) resource that grants permissions to unexpected networks. You can also enforce the standard [network perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/service_specific_controls/api_gateway_policy.json#L46) in the resource-based policies.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateDomainName](https://docs.aws.amazon.com/apigateway/latest/api/API_CreateDomainName.html) API calls in your environment (specifically, the [policy](https://docs.aws.amazon.com/apigateway/latest/api/API_CreateDomainName.html#apigw-CreateDomainName-request-policy) request parameter). If necessary, remediate with the responsive controls of your choice. For example, you can add the standard [network perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/service_specific_controls/api_gateway_policy.json#L46) to the resource-based policy.

## CreateRestApi
### No RCP support

**Perimeter type applicability**: identity and network perimeter applied on resource.

**Description**: [CreateRestApi](https://docs.aws.amazon.com/apigateway/latest/api/API_CreateRestApi.html) allows you to apply a resource-based policy to grant access to a REST API. The service currently does not support RCPs.

**Additional controls**:

If you want to restrict access to trusted identities, consider implementing these additional controls:
* Preventative control example: Consider restricting [UpdateRestApiPolicy](https://docs.aws.amazon.com/service-authorization/latest/reference/list_apigateway.html#list_apigateway-action-UpdateRestApiPolicy) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Policy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-restapi.html#cfn-apigateway-restapi-policy) property for the [AWS::ApiGateway::RestApi](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-restapi.html) resource that grants permissions to untrusted identities. You can also enforce the standard [identity perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/service_specific_controls/api_gateway_policy.json#L5) in the resource-based policies.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateRestApi](https://docs.aws.amazon.com/apigateway/latest/api/API_CreateRestApi.html) API calls in your environment (specifically, the [policy](https://docs.aws.amazon.com/apigateway/latest/api/API_CreateRestApi.html#apigw-CreateRestApi-request-policy) request parameter). If necessary, remediate with the responsive controls of your choice. For example, you can add the standard [identity perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/service_specific_controls/api_gateway_policy.json#L5) to the resource-based policy.

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [UpdateRestApiPolicy](https://docs.aws.amazon.com/service-authorization/latest/reference/list_apigateway.html#list_apigateway-action-UpdateRestApiPolicy) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Policy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-restapi.html#cfn-apigateway-restapi-policy) property for the [AWS::ApiGateway::RestApi](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-restapi.html) resource that grants permissions to unexpected networks. You can also enforce the standard [network perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/service_specific_controls/api_gateway_policy.json#L46) in the resource-based policies.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateRestApi](https://docs.aws.amazon.com/apigateway/latest/api/API_CreateRestApi.html) API calls in your environment (specifically, the [policy](https://docs.aws.amazon.com/apigateway/latest/api/API_CreateRestApi.html#apigw-CreateRestApi-request-policy) request parameter). If necessary, remediate with the responsive controls of your choice. For example, you can add the standard [network perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/service_specific_controls/api_gateway_policy.json#L46) to the resource-based policy.

## ImportRestApi
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [ImportRestApi](https://docs.aws.amazon.com/apigateway/latest/api/API_ImportRestApi.html) allows you to specify a Cognito user pool and HTTP endpoint as the value for the `body` request parameter properties. Because the subsequent requests against the user pool and HTTP endpoint are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Preventative control example: Consider implementing `apigateway:Request/AuthorizerType` in an SCP to help prevent requests to untrusted resources. See [restrict_untrusted_resources_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_untrusted_resources_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Body](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-restapi.html#cfn-apigateway-restapi-body) and [BodyS3Location](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-restapi.html#cfn-apigateway-restapi-bodys3location) properties that do not belong to your organization for the [AWS::ApiGateway::RestApi](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-restapi.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [ImportRestApi](https://docs.aws.amazon.com/apigateway/latest/api/API_ImportRestApi.html) API calls in your environment (specifically, the [body](https://docs.aws.amazon.com/apigateway/latest/api/API_ImportRestApi.html#API_ImportRestApi_RequestBody) request parameter). If necessary, remediate with the responsive controls of your choice.

### No RCP support

**Perimeter type applicability**: identity and network perimeter applied on resource.

**Description**: [ImportRestApi](https://docs.aws.amazon.com/apigateway/latest/api/API_ImportRestApi.html) allows you to apply a resource-based policy to grant access to a REST API. The service currently does not support RCPs.

**Additional controls**:

If you want to restrict access to trusted identities, consider implementing these additional controls:
* Preventative control example: Consider restricting [UpdateRestApiPolicy](https://docs.aws.amazon.com/service-authorization/latest/reference/list_apigateway.html#list_apigateway-action-UpdateRestApiPolicy) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Body](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-restapi.html#cfn-apigateway-restapi-body) and [BodyS3Location](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-restapi.html#cfn-apigateway-restapi-bodys3location) properties for the [AWS::ApiGateway::RestApi](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-restapi.html) resource that grant permissions to untrusted identities. You can also enforce the standard [identity perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/service_specific_controls/api_gateway_policy.json#L5) in the resource-based policies.
* Detective control example: Consider using CloudTrail management events to monitor the [ImportRestApi](https://docs.aws.amazon.com/apigateway/latest/api/API_ImportRestApi.html) API calls in your environment (specifically, the [body](https://docs.aws.amazon.com/apigateway/latest/api/API_ImportRestApi.html#apigw-ImportRestApi-request-body) request parameter). If necessary, remediate with the responsive controls of your choice. For example, you can add the standard [identity perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/service_specific_controls/api_gateway_policy.json#L5) to the resource-based policy.

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [UpdateRestApiPolicy](https://docs.aws.amazon.com/service-authorization/latest/reference/list_apigateway.html#list_apigateway-action-UpdateRestApiPolicy) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Body](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-restapi.html#cfn-apigateway-restapi-body) or [BodyS3Location](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-restapi.html#cfn-apigateway-restapi-bodys3location) property for the [AWS::ApiGateway::RestApi](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-restapi.html) resource that grants permissions to unexpected networks. You can also enforce the standard [network perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/service_specific_controls/api_gateway_policy.json#L46) in the resource-based policies.
* Detective control example: Consider using CloudTrail management events to monitor the [ImportRestApi](https://docs.aws.amazon.com/apigateway/latest/api/API_ImportRestApi.html) API calls in your environment (specifically, the [body](https://docs.aws.amazon.com/apigateway/latest/api/API_ImportRestApi.html#apigw-ImportRestApi-request-body) request parameter). If necessary, remediate with the responsive controls of your choice. For example, you can add the standard [network perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/service_specific_controls/api_gateway_policy.json#L46) to the resource-based policy.

### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [ImportRestApi](https://docs.aws.amazon.com/apigateway/latest/api/API_ImportRestApi.html) allows you to specify a Lambda function that does not belong to your organization as the value for the `body` request parameter property. Because the subsequent call against the Lambda function is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Preventative control example: Consider implementing `apigateway:Request/AuthorizerUri` in an SCP to help prevent requests to untrusted resources. See [restrict_untrusted_resources_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_untrusted_resources_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Body](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-restapi.html#cfn-apigateway-restapi-body) or [BodyS3Location](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-restapi.html#cfn-apigateway-restapi-bodys3location) property that does not belong to your organization for the [AWS::ApiGateway::RestApi](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-restapi.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [ImportRestApi](https://docs.aws.amazon.com/apigateway/latest/api/API_ImportRestApi.html) API calls in your environment (specifically, the [body](https://docs.aws.amazon.com/apigateway/latest/api/API_ImportRestApi.html#API_ImportRestApi_RequestBody) request parameter). If necessary, remediate with the responsive controls of your choice.

## PutIntegration
### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [PutIntegration](https://docs.aws.amazon.com/apigateway/latest/api/API_PutIntegration.html) allows you to specify HTTP endpoints as the value for the `type` and `uri` request parameters. Because the subsequent requests against the HTTP endpoints are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Integration.Type](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-apigateway-method-integration.html#cfn-apigateway-method-integration-type), [Integration.Uri](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-apigateway-method-integration.html#cfn-apigateway-method-integration-uri) properties that do not belong to your organization for the [AWS::ApiGateway::Method](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-method.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [PutIntegration](https://docs.aws.amazon.com/apigateway/latest/api/API_PutIntegration.html) API calls in your environment (specifically, the [type](https://docs.aws.amazon.com/apigateway/latest/api/API_PutIntegration.html#apigw-PutIntegration-request-type) and [uri](https://docs.aws.amazon.com/apigateway/latest/api/API_PutIntegration.html#apigw-PutIntegration-request-uri) request parameters). If necessary, remediate with the responsive controls of your choice.

### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [PutIntegration](https://docs.aws.amazon.com/apigateway/latest/api/API_PutIntegration.html) allows you to specify a Lambda function that does not belong to your organization as the value for the `uri` and `type` request parameters. Because the subsequent call against the Lambda function is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Integration.Type](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-apigateway-method-integration.html#cfn-apigateway-method-integration-type)/[Integration.Uri](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-apigateway-method-integration.html#cfn-apigateway-method-integration-uri) property that does not belong to your organization for the [AWS::ApiGateway::Method](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-method.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [PutIntegration](https://docs.aws.amazon.com/apigateway/latest/api/API_PutIntegration.html) API calls in your environment (specifically, the [uri](https://docs.aws.amazon.com/apigateway/latest/api/API_PutIntegration.html#apigw-PutIntegration-request-uri) and [type](https://docs.aws.amazon.com/apigateway/latest/api/API_PutIntegration.html#apigw-PutIntegration-request-type) request parameters). If necessary, remediate with the responsive controls of your choice.

## UpdateAuthorizer
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [UpdateAuthorizer](https://docs.aws.amazon.com/apigateway/latest/api/API_UpdateAuthorizer.html) allows you to specify a Cognito user pool as the value for the `patchOperations` request parameter. Because the subsequent requests against the user pool are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Preventative control example: Consider implementing `apigateway:Request/AuthorizerType` in an SCP to help prevent requests to untrusted resources. See [restrict_untrusted_resources_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_untrusted_resources_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [ProviderARNs](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-authorizer.html#cfn-apigateway-authorizer-providerarns) property that does not belong to your organization for the [AWS::ApiGateway::Authorizer](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-authorizer.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateAuthorizer](https://docs.aws.amazon.com/apigateway/latest/api/API_UpdateAuthorizer.html) API calls in your environment (specifically, the [patchOperations](https://docs.aws.amazon.com/apigateway/latest/api/API_UpdateAuthorizer.html#apigw-UpdateAuthorizer-request-patchOperations) request parameter). If necessary, remediate with the responsive controls of your choice.

### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [UpdateAuthorizer](https://docs.aws.amazon.com/apigateway/latest/api/API_UpdateAuthorizer.html) allows you to specify a Lambda function that does not belong to your organization as the value for the `patchOperations` request parameter. Because the subsequent call against the Lambda function is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Preventative control example: Consider implementing `apigateway:Request/AuthorizerUri` in an SCP to help prevent requests to untrusted resources. See [restrict_untrusted_resources_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_untrusted_resources_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [AuthorizerUri](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-authorizer.html#cfn-apigateway-authorizer-authorizeruri) property that does not belong to your organization for the [AWS::ApiGateway::Authorizer](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-authorizer.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateAuthorizer](https://docs.aws.amazon.com/apigateway/latest/api/API_UpdateAuthorizer.html) API calls in your environment (specifically, the [patchOperations](https://docs.aws.amazon.com/apigateway/latest/api/API_UpdateAuthorizer.html#apigw-UpdateAuthorizer-request-patchOperations) request parameter). If necessary, remediate with the responsive controls of your choice.

## UpdateDomainName
### No RCP support

**Perimeter type applicability**: identity and network perimeter applied on resource.

**Description**: [UpdateDomainName](https://docs.aws.amazon.com/apigateway/latest/api/API_UpdateDomainName.html) allows you to apply a resource-based policy to grant access to a domain. The service currently does not support RCPs.

**Additional controls**:

If you want to restrict access to trusted identities, consider implementing these additional controls:
* Preventative control example: Consider restricting [UpdateDomainNamePolicy](https://docs.aws.amazon.com/service-authorization/latest/reference/list_apigateway.html#list_apigateway-action-UpdateDomainNamePolicy) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Policy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-domainnamev2.html#cfn-apigateway-domainnamev2-policy) property for the [AWS::ApiGateway::DomainNameV2](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-domainnamev2.html) resource that grants permissions to untrusted identities. You can also enforce the standard [identity perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/service_specific_controls/api_gateway_policy.json#L5) in the resource-based policies.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateDomainName](https://docs.aws.amazon.com/apigateway/latest/api/API_UpdateDomainName.html) API calls in your environment (specifically, the [patchOperations](https://docs.aws.amazon.com/apigateway/latest/api/API_UpdateDomainName.html#apigw-UpdateDomainName-request-patchOperations) request parameter). If necessary, remediate with the responsive controls of your choice. For example, you can add the standard [identity perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/service_specific_controls/api_gateway_policy.json#L5) to the resource-based policy.

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [UpdateDomainNamePolicy](https://docs.aws.amazon.com/service-authorization/latest/reference/list_apigateway.html#list_apigateway-action-UpdateDomainNamePolicy) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Policy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-domainnamev2.html#cfn-apigateway-domainnamev2-policy) property for the [AWS::ApiGateway::DomainNameV2](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-domainnamev2.html) resource that grants permissions to unexpected networks. You can also enforce the standard [network perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/service_specific_controls/api_gateway_policy.json#L46) in the resource-based policies.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateDomainName](https://docs.aws.amazon.com/apigateway/latest/api/API_UpdateDomainName.html) API calls in your environment (specifically, the [patchOperations](https://docs.aws.amazon.com/apigateway/latest/api/API_UpdateDomainName.html#apigw-UpdateDomainName-request-patchOperations) request parameter). If necessary, remediate with the responsive controls of your choice. For example, you can add the standard [network perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/service_specific_controls/api_gateway_policy.json#L46) to the resource-based policy.

## UpdateIntegration
### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [UpdateIntegration](https://docs.aws.amazon.com/apigateway/latest/api/API_UpdateIntegration.html) allows you to specify HTTP endpoints as the value for the `patchOperations` request parameter. Because the subsequent requests against the HTTP endpoints are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Integration.Uri](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-apigateway-method-integration.html#cfn-apigateway-method-integration-uri) property that does not belong to your organization for the [AWS::ApiGateway::Method](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-apigateway-method.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateIntegration](https://docs.aws.amazon.com/apigateway/latest/api/API_UpdateIntegration.html) API calls in your environment (specifically, the [patchOperations](https://docs.aws.amazon.com/apigateway/latest/api/API_UpdateIntegration.html#apigw-UpdateIntegration-request-patchOperations) request parameter). If necessary, remediate with the responsive controls of your choice.

### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [UpdateIntegration](https://docs.aws.amazon.com/apigateway/latest/api/API_UpdateIntegration.html) allows you to specify a Lambda function that does not belong to your organization as the value for the `patchOperations` request parameter. Because the subsequent call against the Lambda function is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Integration.Uri](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-apigateway-method-integration.html#cfn-apigateway-method-integration-uri) property that does not belong to your organization for the [AWS::ApiGateway::Method](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-method.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateIntegration](https://docs.aws.amazon.com/apigateway/latest/api/API_UpdateIntegration.html) API calls in your environment (specifically, the [patchOperations](https://docs.aws.amazon.com/apigateway/latest/api/API_UpdateIntegration.html#apigw-UpdateIntegration-request-patchOperations) request parameter). If necessary, remediate with the responsive controls of your choice.

## UpdateRestApi
### No RCP support

**Perimeter type applicability**: identity and network perimeter applied on resource.

**Description**: [UpdateRestApi](https://docs.aws.amazon.com/apigateway/latest/api/API_UpdateRestApi.html) allows you to apply a resource-based policy to grant access to a RestApi. The service currently does not support RCPs.

**Additional controls**:

If you want to restrict access to trusted identities, consider implementing these additional controls:
* Preventative control example: Consider restricting [UpdateRestApiPolicy](https://docs.aws.amazon.com/service-authorization/latest/reference/list_apigateway.html#list_apigateway-action-UpdateRestApiPolicy) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Policy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-restapi.html#cfn-apigateway-restapi-policy) property for the [AWS::ApiGateway::RestApi](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-restapi.html) resource that grants permissions to untrusted identities. You can also enforce the standard [identity perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/service_specific_controls/api_gateway_policy.json#L5) in the resource-based policies.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateRestApi](https://docs.aws.amazon.com/apigateway/latest/api/API_UpdateRestApi.html) API calls in your environment (specifically, the [patchOperations](https://docs.aws.amazon.com/apigateway/latest/api/API_UpdateRestApi.html#apigw-UpdateRestApi-request-patchOperations) request parameter). If necessary, remediate with the responsive controls of your choice. For example, you can add the standard [identity perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/service_specific_controls/api_gateway_policy.json#L5) to the resource-based policy.

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [UpdateRestApiPolicy](https://docs.aws.amazon.com/service-authorization/latest/reference/list_apigateway.html#list_apigateway-action-UpdateRestApiPolicy) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Policy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-restapi.html#cfn-apigateway-restapi-policy) property for the [AWS::ApiGateway::RestApi](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-restapi.html) resource that grants permissions to unexpected networks. You can also enforce the standard [network perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/service_specific_controls/api_gateway_policy.json#L46) in the resource-based policies.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateRestApi](https://docs.aws.amazon.com/apigateway/latest/api/API_UpdateRestApi.html) API calls in your environment (specifically, the [patchOperations](https://docs.aws.amazon.com/apigateway/latest/api/API_UpdateRestApi.html#apigw-UpdateRestApi-request-patchOperations) request parameter). If necessary, remediate with the responsive controls of your choice. For example, you can add the standard [network perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/service_specific_controls/api_gateway_policy.json#L46) to the resource-based policy.

## List of service APIs reviewed against data perimeter control objectives

* CreateApiKey
* CreateAuthorizer
* CreateBasePathMapping
* CreateDeployment
* CreateDocumentationPart
* CreateDocumentationVersion
* CreateDomainName
* CreateModel
* CreateRequestValidator
* CreateResource
* CreateRestApi
* CreateStage
* CreateUsagePlan
* CreateUsagePlanKey
* DeleteApiKey
* DeleteAuthorizer
* DeleteBasePathMapping
* DeleteClientCertificate
* DeleteDeployment
* DeleteDocumentationPart
* DeleteDocumentationVersion
* DeleteDomainName
* DeleteGatewayResponse
* DeleteIntegration
* DeleteIntegrationResponse
* DeleteMethod
* DeleteMethodResponse
* DeleteModel
* DeleteRequestValidator
* DeleteResource
* DeleteRestApi
* DeleteStage
* DeleteUsagePlan
* DeleteUsagePlanKey
* FlushStageAuthorizersCache
* FlushStageCache
* GenerateClientCertificate
* GetAccount
* GetApiKey
* GetApiKeys
* GetAuthorizer
* GetAuthorizers
* GetBasePathMapping
* GetBasePathMappings
* GetClientCertificate
* GetClientCertificates
* GetDeployment
* GetDeployments
* GetDocumentationPart
* GetDocumentationParts
* GetDocumentationVersion
* GetDocumentationVersions
* GetDomainName
* GetDomainNameAccessAssociations
* GetDomainNames
* GetExport
* GetGatewayResponse
* GetGatewayResponses
* GetIntegration
* GetIntegrationResponse
* GetMethod
* GetMethodResponse
* GetModel
* GetModelTemplate
* GetModels
* GetRequestValidator
* GetRequestValidators
* GetResource
* GetResources
* GetRestApi
* GetRestApis
* GetSdk
* GetSdkType
* GetSdkTypes
* GetStage
* GetStages
* GetTags
* GetUsage
* GetUsagePlan
* GetUsagePlanKey
* GetUsagePlanKeys
* GetUsagePlans
* GetVpcLinks
* ImportApiKeys
* ImportDocumentationParts
* ImportRestApi
* PutGatewayResponse
* PutIntegration
* PutIntegrationResponse
* PutMethod
* PutMethodResponse
* PutRestApi
* TagResource
* TestInvokeAuthorizer
* TestInvokeMethod
* UntagResource
* UpdateAccount
* UpdateApiKey
* UpdateAuthorizer
* UpdateBasePathMapping
* UpdateClientCertificate
* UpdateDeployment
* UpdateDocumentationPart
* UpdateDocumentationVersion
* UpdateDomainName
* UpdateGatewayResponse
* UpdateIntegration
* UpdateIntegrationResponse
* UpdateMethod
* UpdateMethodResponse
* UpdateModel
* UpdateRequestValidator
* UpdateResource
* UpdateRestApi
* UpdateStage
* UpdateUsage
* UpdateUsagePlan
