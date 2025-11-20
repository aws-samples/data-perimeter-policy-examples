
# Service-specific guidance: AWS Lambda


This document outlines service-specific guidance for implementing a data perimeter for AWS Lambda. 

AWS Lambda is a serverless compute service that lets you run code without provisioning or managing servers. It automatically scales your applications in response to incoming requests, and you only pay for the compute time you consume. Lambda supports multiple programming languages and integrates seamlessly with other AWS services, making it ideal for building microservices, automating tasks, and creating event-driven applications.


The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type | Security objective | Applied on | Policy type | Additional considerations |
|----------------|-------------------|------------|-------------|------------------------|
| Identity perimeter | Only trusted identities can access my resources | Resource | RCP | Y |
| Identity perimeter | Only trusted identities are allowed from my network | Network | VPC endpoint policy | Y |
| Resource perimeter | My identities can access only trusted resources | Identity | SCP | Y |
| Resource perimeter | Only trusted resources can be accessed from my network | Network | VPC endpoint policy | Y |
| Network perimeter | My identities can access resources only from expected networks | Identity | SCP | Y |
| Network perimeter | My resources can be accessed only from expected networks | Resource | RCP | Y |

*Y – Additional considerations apply. N – No additional considerations apply.
 



**Additional consideration 1**

Perimeter type applicability: resource perimeter.
        
GetLayerVersion might require access to service-owned layers, which are layers that are managed in service accounts.

See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access. 
If you want to restrict access to trusted resources, see [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json) for information about how the default resource perimeter SCP accounts for such an access pattern. Use the service-specific policy,  [lambda_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/lambda_endpoint_policy.json) to enforce the resource perimeter on your network.


**Additional consideration 2**

Perimeter type applicability: network perimeter.
        
GetFunction returns Amazon S3 presigned URL that users can use to download AWS Lambda function deployment packages from service-owned buckets. The presigned URL is created with the AWS Lambda service principal.

If you want to restrict access to expected networks, consider implementing these additional controls:

* **Preventative control example:** Consider restricting [GetFunction](https://docs.aws.amazon.com/lambda/latest/dg/API_GetFunction.html) permissions to administrators or specific applications only using an SCP. See [restrict_presignedURL_scp.json](../service_control_policies/service_specific_controls/restrict_presignedURL_scp.json) for an example policy.
* **Detective control example:** Consider using CloudTrail management events to monitor the [GetFunction](https://docs.aws.amazon.com/lambda/latest/dg/API_GetFunction.html) API calls in your environment. If necessary, remediate with the responsive controls of your choice.


**Additional consideration 3**

Perimeter type applicability: all.
        
UpdateFunctionConfiguration allows you to associate functions with an Amazon VPC to run your code.

If you want to achieve data perimeter control objectives, consider implementing these additional controls:

* **Preventative control example:** Consider implementing `lambda:VpcIds` in an SCP to help restrict creation of resources to a customer managed VPC only. See [restrict_nonvpc_deployment_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.
* **Proactive control example:** Consider implementing CloudFormation Hooks to help ensure that developers specify the [VpcConfig](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-lambda-function.html#cfn-lambda-function-vpcconfig) property of the [AWS::Lambda::Function](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-lambda-function.html) resource.
* **Detective control example 1:** Consider implementing the AWS Config rule, [lambda-inside-vpc](https://docs.aws.amazon.com/config/latest/developerguide/lambda-inside-vpc.html), to help detect misconfigurations or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* **Detective control example 2:** Consider using CloudTrail management events to monitor the [UpdateFunctionConfiguration](https://docs.aws.amazon.com/lambda/latest/api/API_UpdateFunctionConfiguration.html) API calls in your environment (specifically, the [VpcConfig](https://docs.aws.amazon.com/lambda/latest/api/API_UpdateFunctionConfiguration.html#lambda-UpdateFunctionConfiguration-request-VpcConfig) request parameter). If necessary, remediate with the responsive controls of your choice.


**Additional consideration 4**

Perimeter type applicability: all.
        
CreateFunction allows you to associate functions with an Amazon VPC to run your code.


If you want to achieve data perimeter control objectives, consider implementing these additional controls:

* **Preventative control example:** Consider implementing `lambda:VpcIds` in an SCP to help restrict creation of resources to a customer managed VPC only. See [restrict_nonvpc_deployment_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.
* **Proactive control example:** Consider implementing CloudFormation Hooks to help ensure that developers specify the [VpcConfig](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-lambda-function.html#cfn-lambda-function-vpcconfig) property of the [AWS::Lambda::Function](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-lambda-function.html) resource.
* **Detective control example 1:** Consider implementing the AWS Config rule, [lambda-inside-vpc](https://docs.aws.amazon.com/config/latest/developerguide/lambda-inside-vpc.html), to help detect misconfigurations or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* **Detective control example 2:** Consider using CloudTrail management events to monitor the [CreateFunction](https://docs.aws.amazon.com/lambda/latest/api/API_CreateFunction.html) API calls in your environment (specifically, the [VpcConfig](https://docs.aws.amazon.com/lambda/latest/api/API_CreateFunction.html#lambda-CreateFunction-request-VpcConfig) request parameter). If necessary, remediate with the responsive controls of your choice.



**Additional consideration 5**

Perimeter type applicability: resource perimeter applied on identity.
        
UpdateCodeSigningConfig allows you to specify a signing profile that does not belong to your organization as the value for the SigningProfileVersionArns parameter. Because the subsequent call is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls:

* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [SigningProfileVersionArns](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-lambda-codesigningconfig-allowedpublishers.html#cfn-lambda-codesigningconfig-allowedpublishers-signingprofileversionarns) property that does not belong to your organization for the [AWS::Lambda::CodeSigningConfig](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-lambda-codesigningconfig.html) resource.
* **Detective control example:** Consider using CloudTrail management events to monitor the [UpdateCodeSigningConfig](https://docs.aws.amazon.com/lambda/latest/api/API_UpdateCodeSigningConfig.html) API calls in your environment (specifically, the [SigningProfileVersionArns](https://docs.aws.amazon.com/lambda/latest/api/API_AllowedPublishers.html#lambda-Type-AllowedPublishers-SigningProfileVersionArns) request parameter). If necessary, remediate with the responsive controls of your  choice.


**Additional consideration 6**

Perimeter type applicability: identity and network perimeter applied on resource.
        
AddLayerVersionPermission allows you to apply a resource-based policy to grant access to a layer. The service currently does not support RCPs.

If you want to restrict access to trusted identities and expected networks, consider implementing these additional controls:

* **Preventative control example**: Consider restricting [AddLayerVersionPermission](https://docs.aws.amazon.com/lambda/latest/dg/API_AddLayerVersionPermission.html) permissions to administrators only using an SCP. See [restrict_resource_policy_configurations_scp.json](../service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Principal](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-lambda-layerversionpermission.html#cfn-lambda-layerversionpermission-principal) and [OrganizationId](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-lambda-layerversionpermission.html#cfn-lambda-layerversionpermission-organizationid) properties for the [AWS::Lambda::LayerVersionPermission](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-lambda-layerversionpermission.html) resource that grants permissions to untrusted identities. 
* **Detective control example 1:** Consider using [AWS Identity and Access Management Access Analyzer](https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html) external access analyzers to help identify resource types that support resource-based policies in your accounts that are shared with untrusted identities. If necessary, remediate with the responsive controls of your choice. 
* **Detective control example 2:** Consider using CloudTrail management events to monitor the [AddLayerVersionPermission](https://docs.aws.amazon.com/lambda/latest/dg/API_AddLayerVersionPermission.html) API calls in your environment (specifically, the [Principal](https://docs.aws.amazon.com/lambda/latest/api/API_AddLayerVersionPermission.html#lambda-AddLayerVersionPermission-request-Principal) and [OrganizationId](https://docs.aws.amazon.com/lambda/latest/api/API_AddLayerVersionPermission.html#lambda-AddLayerVersionPermission-request-OrganizationId) request parameters). 


**Additional consideration 7**

Perimeter type applicability: identity and network perimeter applied on resource.
        
AddPermission allows you to apply a resource-based policy to grant access to a function. The service currently does not support RCPs.

If you want to restrict access to trusted identities and expected networks, consider implementing these additional controls:

* **Preventative control example**: Consider restricting [AddPermission](https://docs.aws.amazon.com/lambda/latest/dg/API_AddPermission.html) permissions to administrators only using an SCP. See [restrict_resource_policy_configurations_scp.json](../service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Principal](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-lambda-permission.html#cfn-lambda-permission-principal) and [PrincipalOrgID](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-lambda-permission.html#cfn-lambda-permission-principalorgid) properties for the [AWS::Lambda::Permission](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-lambda-permission.html) resource that grants permissions to untrusted identities or unexpected networks. 
* **Detective control example:** Consider using [AWS Identity and Access Management Access Analyzer](https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html) external access analyzers to help identify resource types that support resource-based policies in your accounts that are shared with untrusted identities. If necessary, remediate with the responsive controls of your choice. 
* **Detective control example:** Consider using CloudTrail management events to monitor the [AddPermission](https://docs.aws.amazon.com/lambda/latest/dg/API_AddPermission.html) API calls in your environment (specifically, the [Principal](https://docs.aws.amazon.com/lambda/latest/api/API_AddPermission.html#lambda-AddPermission-request-Principal) and [PrincipalOrgID](https://docs.aws.amazon.com/lambda/latest/api/API_AddPermission.html#lambda-AddPermission-request-PrincipalOrgID) request parameter). If necessary, remediate with the responsive controls of your choice. 


**Additional consideration 8**

Perimeter type applicability: network perimeter.
        
CreateFunction allows you to specify a service role, referred to as a function’s execution role, for your function’s operations. The service uses the role to make AWS API calls from your code and to directly call other services on your behalf.


If you want to restrict access to expected networks, consider using the service-specific policy, [network_perimeter_lambda_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/network_perimeter_lambda_scp.json). This policy lists `logs:CreateLogGroup`, `logs:CreateLogStream`, `logs:PutLogEvents`, `elasticfilesystem:ClientMount`, `xray:PutTraceSegments` in the `NotAction` element because the service makes direct calls from its network to CloudWatch Logs, AWS X-Ray, and Amazon Elastic File System (Amazon EFS) on your behalf. If the service role assumes another role as part of function operations, consider restricting access to expected networks for the assumed role by implementing [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json).



**Additional consideration 9**

Perimeter type applicability: network perimeter.
        
UpdateFunctionConfiguration allows you to specify a service role, referred to as a function’s execution role, for your function’s operations. The service uses the role to make AWS API calls from your code and to directly call other services on your behalf.


If you want to restrict access to expected networks, consider using the service-specific policy, [network_perimeter_lambda_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/network_perimeter_lambda_scp.json). This policy lists `logs:CreateLogGroup`, `logs:CreateLogStream`, `logs:PutLogEvents`, `elasticfilesystem:ClientMount`, `xray:PutTraceSegments` in the `NotAction` element because the service makes direct calls from its network to CloudWatch Logs, AWS X-Ray, and Amazon Elastic File System (Amazon EFS) on your behalf. If the service role assumes another role as part of function operations, consider restricting access to expected networks for the assumed role, see [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).



**Additional consideration 10**

Perimeter type applicability: resource perimeter applied on identity.
        
CreateCodeSigningConfig allows you to specify a signing profile that does not belong to your organization as the value for the SigningProfileVersionArns parameter. Because the subsequent call is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls:

* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [SigningProfileVersionArns](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-lambda-codesigningconfig-allowedpublishers.html#cfn-lambda-codesigningconfig-allowedpublishers-signingprofileversionarns) property that does not belong to your organization for the [AWS::Lambda::CodeSigningConfig](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-lambda-codesigningconfig.html) resource.
* **Detective control example:** Consider using CloudTrail management events to monitor the [CreateCodeSigningConfig](https://docs.aws.amazon.com/lambda/latest/dg/API_CreateCodeSigningConfig.html) API calls in your environment (specifically, the [SigningProfileVersionArns](https://docs.aws.amazon.com/lambda/latest/api/API_AllowedPublishers.html#lambda-Type-AllowedPublishers-SigningProfileVersionArns) request parameter). If necessary, remediate with the responsive controls of your  choice.


**List of service APIs reviewed against data perimeter control objectives**

* AddLayerVersionPermission
* AddPermission
* CreateAlias
* CreateCodeSigningConfig
* CreateEventSourceMapping
* CreateFunction
* CreateFunctionUrlConfig
* DeleteAlias
* DeleteCodeSigningConfig
* DeleteEventSourceMapping
* DeleteFunction
* DeleteFunctionCodeSigningConfig
* DeleteFunctionConcurrency
* DeleteFunctionUrlConfig
* DeleteLayerVersion
* GetAccountSettings
* GetAlias
* GetCodeSigningConfig
* GetFunction
* GetFunctionCodeSigningConfig
* GetFunctionConcurrency
* GetFunctionConfiguration
* GetFunctionEventInvokeConfig
* GetFunctionRecursionConfig
* GetFunctionUrlConfig
* GetLayerVersion
* GetLayerVersionByArn
* GetLayerVersionPolicy
* GetPolicy
* GetRuntimeManagementConfig
* Invoke
* ListAliases
* ListCodeSigningConfigs
* ListEventSourceMappings
* ListFunctionEventInvokeConfigs
* ListFunctionUrlConfigs
* ListFunctions
* ListFunctionsByCodeSigningConfig
* ListLayerVersions
* ListLayers
* ListProvisionedConcurrencyConfigs
* ListTags
* ListVersionsByFunction
* PublishLayerVersion
* PublishVersion
* PutFunctionCodeSigningConfig
* PutFunctionConcurrency
* PutFunctionEventInvokeConfig
* PutFunctionRecursionConfig
* RemoveLayerVersionPermission
* RemovePermission
* TagResource
* UntagResource
* UpdateAlias
* UpdateFunctionCode
* UpdateFunctionConfiguration
* UpdateFunctionUrlConfig