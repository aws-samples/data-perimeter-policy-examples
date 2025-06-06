
# Service-specific guidance: Amazon API Gateway


This document outlines service-specific guidance for implementing a data perimeter for Amazon API Gateway. Amazon API Gateway is a fully managed service that enables developers to create, publish, maintain, monitor, and secure APIs at any scale. It acts as a "front door" for applications to access data, business logic, or functionality from your backend services, such as workloads running on Amazon EC2, code running on AWS Lambda, or any web application. API Gateway handles all the tasks involved in accepting and processing up to hundreds of thousands of concurrent API calls, including traffic management, authorization and access control, monitoring, and API version management.


The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type | Security objective | Applied on | Policy type | Additional considerations |
|----------------|-------------------|------------|-------------|------------------------|
| Identity perimeter | Only trusted identities can access my resources | Resource | RCP | Y |
| Identity perimeter | Only trusted identities are allowed from my network | Network | VPC Endpoint Policy | Y |
| Resource perimeter | My identities can access only trusted resources | Identity | SCP | Y |
| Resource perimeter | Only trusted resources can be accessed from my network | Network | VPC Endpoint Policy | Y |
| Network perimeter | My identities can access resources only from expected networks | Identity | SCP | N |
| Network perimeter | My resources can be accesses only from expected networks | Resource | RCP | Y |

*Y – Additional considerations apply. N – No additional considerations apply.
 



**Additional consideration 1**

Perimeter type applicability: identity and resource perimeter applied on network.
        
The service does not currently support VPC endpoint policies.

If you want to restrict access to your networks to trusted identities and trusted resources, consider implementing these additional controls:

* **Preventative control example**: Consider implementing `aws:ResourceOrgID` in an SCP to restrict service calls so that your identities can only access trusted resources. See [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json) for an example policy.
* **Preventative control example**: Consider using your existing security appliances such as outbound proxies to inspect service calls in your environment for the identities making the calls and resources being accessed, and restrict the calls accordingly. This type of solution might have implications for security, scalability, latency, and reliability that you should evaluate carefully.



**Additional consideration 2**

Perimeter type applicability: identity and network perimeter applied on resource.
        
CreateRestApi allows you to apply a resource-based policy to grant access to a RestApi. The service currently doesn’t support RCPs.


If you want to restrict access to trusted identities and expected networks, consider implementing these additional controls:

* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Policy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-restapi.html#cfn-apigateway-restapi-policy) property for the [AWS::ApiGateway::RestApi](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-restapi.html) resource that grants permissions to untrusted identities or unexpected networks. You can also enforce the standard [identity perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/4ffd54025970993ea1c472e5e99dcae73f79423b/resource_control_policies/resource_based_policies/api_gateway_policy.json#L5) and [network perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/4ffd54025970993ea1c472e5e99dcae73f79423b/resource_control_policies/resource_based_policies/api_gateway_policy.json#L24) in the resource-based policies.
* **Detective control example:** Consider using CloudTrail management events to monitor the [CreateRestApi](https://docs.aws.amazon.com/apigateway/latest/api/API_CreateRestApi.html) API calls in your environment (specifically, the [policy](https://docs.aws.amazon.com/apigateway/latest/api/API_CreateRestApi.html#apigw-CreateRestApi-request-policy) request parameter). If necessary, remediate with the responsive controls of your choice. For example, you can add the standard [identity perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/4ffd54025970993ea1c472e5e99dcae73f79423b/resource_control_policies/resource_based_policies/api_gateway_policy.json#L5) and [network perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/4ffd54025970993ea1c472e5e99dcae73f79423b/resource_control_policies/resource_based_policies/api_gateway_policy.json#L24) to the resource-based policy.



**Additional consideration 3**

Perimeter type applicability: identity and network perimeter applied on resource.
        
CreateDomainName allows you to apply a resource-based policy to grant access to a domain. The service currently doesn’t support RCPs.

If you want to restrict access to trusted identities and expected networks, consider implementing these additional controls:

* **Detective control example:** Consider using CloudTrail management events to monitor the [CreateDomainName](https://docs.aws.amazon.com/apigateway/latest/api/API_CreateDomainName.html) API calls in your environment (specifically, the [policy](https://docs.aws.amazon.com/apigateway/latest/api/API_CreateDomainName.html#apigw-CreateDomainName-request-policy) request parameter). If necessary, remediate with the responsive controls of your choice. For example, you can add the standard [identity perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/4ffd54025970993ea1c472e5e99dcae73f79423b/resource_control_policies/resource_based_policies/api_gateway_policy.json#L5) and [network perimeter statement](https://github.com/aws-samples/data-perimeter-policy-examples/blob/4ffd54025970993ea1c472e5e99dcae73f79423b/resource_control_policies/resource_based_policies/api_gateway_policy.json#L24) to the resource-based policy.


**Additional consideration 4**
Perimeter type applicability: resource perimeter applied on identity.
        
PutMethod allows you to specify a Lambda function that does not belong to your organization as the value for the authorizerId parameter. Because the subsequent call against the function is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls:

* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [AuthorizerId](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-method.html#cfn-apigateway-method-authorizerid) property that does not belong to your organization for the [AWS::ApiGateway::Method](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-apigateway-method.html) resource.
* **Detective control example:** Consider using CloudTrail management events to monitor the [PutMethod](https://docs.aws.amazon.com/apigateway/latest/api/API_PutMethod.html) API calls in your environment (specifically, the [authorizerId](https://docs.aws.amazon.com/apigateway/latest/api/API_PutMethod.html#apigw-PutMethod-request-authorizerId) request parameter). If necessary, remediate with the responsive controls of your  choice.





**List of service APIs reviewed against data perimeter control objectives**


            * CreateDomainName
            
            * CreateRestApi
            
            * GetResources
            
            * CreateResource
            
            * PutMethod
            
            * PutIntegration
            
            * CreateBasePathMapping
            
            * GetClientCertificates
            
            * GetBasePathMappings
            
            * CreateApiKey
            
            * CreateAuthorizer
            
            * CreateDeployment
            
            * CreateDocumentationPart
            
            * CreateDocumentationVersion
            
            * CreateModel
            
            * CreateRequestValidator
            
            * CreateStage
            
            * CreateUsagePlan
            
            * CreateUsagePlanKey
            
            * PutGatewayResponse
            
            * PutIntegrationResponse
            
            * PutMethodResponse
            
            * PutRestApi
            
            * TagResource
            
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
            
            * GetAccount
            
            * GetApiKey
            
            * GetApiKeys
            
            * GetAuthorizer
            
            * GetAuthorizers
            
            * GetBasePathMapping
            
            * GetClientCertificate
            
            * GetDeployment
            
            * GetDeployments
            
            * GetDocumentationPart
            
            * GetDocumentationParts
            
            * GetDocumentationVersion
            
            * GetDocumentationVersions
            
            * GetDomainName
            
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
            
            * UntagResource
            
            * FlushStageCache
            
            * FlushStageAuthorizersCache
            
            * ImportApiKeys
            
            * ImportDocumentationParts
            
            * TestInvokeAuthorizer
            
            * TestInvokeMethod
            
            * GenerateClientCertificate
            
            * ImportRestApi
            
            * DeleteAuthorizer
            
            * DeleteBasePathMapping
            
            * DeleteClientCertificate
            
            * DeleteDocumentationPart
            
            * DeleteDocumentationVersion
            
            * DeleteDomainName
            
            * DeleteGatewayResponse
            
            * DeleteIntegrationResponse
            
            * DeleteMethodResponse
            
            * DeleteModel
            
            * DeleteRequestValidator
            
            * DeleteStage
            
            * DeleteUsagePlanKey
            
            * DeleteDeployment
            
            * DeleteUsagePlan
            
            * DeleteApiKey
            
            * DeleteIntegration
            
            * DeleteMethod
            
            * DeleteResource
            
            * DeleteRestApi
            

