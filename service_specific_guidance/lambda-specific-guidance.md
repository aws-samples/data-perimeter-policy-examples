# Service-specific guidance: AWS Lambda


This document outlines service-specific guidance for implementing a data perimeter for AWS Lambda.

AWS Lambda is a serverless compute service that lets you run code without provisioning or managing servers. Lambda automatically manages the underlying infrastructure — including server maintenance, capacity provisioning, scaling, and patching — so you can focus on your application logic.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | Y |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | Y |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | Y |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | Y |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | Y |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | Y |

*Y - Additional considerations apply. N - No additional considerations apply.

## AddLayerVersionPermission
### No RCP support

**Perimeter type applicability**: identity and network perimeter applied on resource.

**Description**: [AddLayerVersionPermission](https://docs.aws.amazon.com/lambda/latest/api/API_AddLayerVersionPermission.html) allows you to apply a resource-based policy to grant access to a layer version. The service currently does not support RCPs.

**Additional controls**:

If you want to restrict access to trusted identities, consider implementing these additional controls:
* Preventative control example: Consider restricting [AddLayerVersionPermission](https://docs.aws.amazon.com/lambda/latest/api/API_AddLayerVersionPermission.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Principal](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-lambda-layerversionpermission.html#cfn-lambda-layerversionpermission-principal) and [OrganizationId](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-lambda-layerversionpermission.html#cfn-lambda-layerversionpermission-organizationid) properties for the [AWS::Lambda::LayerVersionPermission](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-lambda-layerversionpermission.html) resource that grant permissions to untrusted identities.
* Detective control example: Consider using [AWS Identity and Access Management Access Analyzer](https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html) external access analyzers to help identify layer versions in your accounts that are shared with untrusted identities. If necessary, remediate with the responsive controls of your choice.
* Detective control example: Consider using CloudTrail management events to monitor the [AddLayerVersionPermission](https://docs.aws.amazon.com/lambda/latest/api/API_AddLayerVersionPermission.html) API calls in your environment (specifically, the [Principal](https://docs.aws.amazon.com/lambda/latest/api/API_AddLayerVersionPermission.html#lambda-AddLayerVersionPermission-request-Principal) and [OrganizationId](https://docs.aws.amazon.com/lambda/latest/api/API_AddLayerVersionPermission.html#lambda-AddLayerVersionPermission-request-OrganizationId) request parameters). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access to expected networks, consider implementing this additional control:
* Preventative control example: Consider restricting [AddLayerVersionPermission](https://docs.aws.amazon.com/lambda/latest/api/API_AddLayerVersionPermission.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.

## AddPermission
### No RCP support

**Perimeter type applicability**: identity and network perimeter applied on resource.

**Description**: [AddPermission](https://docs.aws.amazon.com/lambda/latest/api/API_AddPermission.html) allows you to apply a resource-based policy to grant access to a function. The service currently does not support RCPs.

**Additional controls**:

If you want to restrict access to trusted identities, consider implementing these additional controls:
* Preventative control example: Consider restricting [AddPermission](https://docs.aws.amazon.com/lambda/latest/api/API_AddPermission.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Principal](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-lambda-permission.html#cfn-lambda-permission-principal) or [PrincipalOrgID](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-lambda-permission.html#cfn-lambda-permission-principalorgid) property for the [AWS::Lambda::Permission](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-lambda-permission.html) resource that grants permissions to untrusted identities.
* Detective control example: Consider using [AWS Identity and Access Management Access Analyzer](https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html) external access analyzers to help identify functions in your accounts that are shared with untrusted identities. If necessary, remediate with the responsive controls of your choice.
* Detective control example: Consider using CloudTrail management events to monitor the [AddPermission](https://docs.aws.amazon.com/lambda/latest/api/API_AddPermission.html) API calls in your environment (specifically, the [Principal](https://docs.aws.amazon.com/lambda/latest/api/API_AddPermission.html#lambda-AddPermission-request-Principal), [PrincipalOrgID](https://docs.aws.amazon.com/lambda/latest/api/API_AddPermission.html#lambda-AddPermission-request-PrincipalOrgID), [SourceArn](https://docs.aws.amazon.com/lambda/latest/api/API_AddPermission.html#lambda-AddPermission-request-SourceArn), and [SourceAccount](https://docs.aws.amazon.com/lambda/latest/api/API_AddPermission.html#lambda-AddPermission-request-SourceAccount) request parameters). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access to expected networks, consider implementing this additional control:
* Preventative control example: Consider restricting [AddPermission](https://docs.aws.amazon.com/lambda/latest/api/API_AddPermission.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.

## CreateCodeSigningConfig
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [CreateCodeSigningConfig](https://docs.aws.amazon.com/lambda/latest/api/API_CreateCodeSigningConfig.html) allows you to specify a signing profile that does not belong to your organization as the value for the `AllowedPublishers.SigningProfileVersionArns` request parameter. Because the subsequent call against the signing profile is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [AllowedPublishers.SigningProfileVersionArns](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-lambda-codesigningconfig-allowedpublishers.html) property that does not belong to your organization for the [AWS::Lambda::CodeSigningConfig](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-lambda-codesigningconfig.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateCodeSigningConfig](https://docs.aws.amazon.com/lambda/latest/api/API_CreateCodeSigningConfig.html) API calls in your environment (specifically, the [AllowedPublishers.SigningProfileVersionArns](https://docs.aws.amazon.com/lambda/latest/api/API_AllowedPublishers.html#lambda-Type-AllowedPublishers-SigningProfileVersionArns) request parameter). If necessary, remediate with the responsive controls of your choice.

## CreateFunction
### Code execution within a VPC

**Perimeter type applicability**: identity and resource perimeter on network, network perimeter.

**Description**: [CreateFunction](https://docs.aws.amazon.com/lambda/latest/api/API_CreateFunction.html) allows you to associate functions with an Amazon VPC to run your code.

**Additional controls**:

If you want to achieve data perimeter control objectives, consider implementing these additional controls:
* Preventative control example: Consider implementing `lambda:VpcIds` in an SCP to help restrict creation of resources to a customer managed VPC only. See [restrict_nonvpc_deployment_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help ensure that developers specify the [VpcConfig](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-lambda-function.html#cfn-lambda-function-vpcconfig) property of the [AWS::Lambda::Function](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-lambda-function.html) resource.
* Detective control example: Consider implementing the AWS Config rule, [lambda-inside-vpc](https://docs.aws.amazon.com/config/latest/developerguide/lambda-inside-vpc.html), to help detect functions not associated with a VPC or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateFunction](https://docs.aws.amazon.com/lambda/latest/api/API_CreateFunction.html) API calls in your environment (specifically, the [VpcConfig](https://docs.aws.amazon.com/lambda/latest/api/API_VpcConfig.html) request parameter). If necessary, remediate with the responsive controls of your choice.

### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [CreateFunction](https://docs.aws.amazon.com/lambda/latest/api/API_CreateFunction.html) allows you to specify a service role, referred to as the function's execution role, for your Lambda function operations. The service uses the role to make AWS API calls from your code.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the service-specific SCP, [network_perimeter_lambda_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/network_perimeter_lambda_scp.json). This policy lists `logs:CreateLogGroup`, `logs:CreateLogStream`, `logs:PutLogEvents`, `elasticfilesystem:client*`, and `xray:PutTraceSegments` in the `NotAction` element because the service makes direct calls from its network to Amazon CloudWatch Logs, Amazon EFS, and AWS X-Ray on your behalf. If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

## GetFunction
### S3 presigned URLs created by service principals

**Perimeter type applicability**: network perimeter on resource.

**Description**: [GetFunction](https://docs.aws.amazon.com/lambda/latest/api/API_GetFunction.html) returns an Amazon S3 presigned URL that users can use to download AWS Lambda function deployment packages from service-owned buckets. The presigned URL is created with the AWS Lambda service principal.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [GetFunction](https://docs.aws.amazon.com/lambda/latest/api/API_GetFunction.html) permissions to select principals using an SCP. See [restrict_presignedURL_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_presignedURL_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [GetFunction](https://docs.aws.amazon.com/lambda/latest/api/API_GetFunction.html) API calls in your environment. If necessary, remediate with the responsive controls of your choice.

## GetLayerVersion
### Access to service-owned resources

**Perimeter type applicability**: resource perimeter.

**Description**: [GetLayerVersion](https://docs.aws.amazon.com/lambda/latest/api/API_GetLayerVersion.html) might require access to service-owned layers, which are layers that are managed in service accounts.

**Additional controls**:

If you want to restrict access to trusted resources for your identities, consider implementing this control:
* Preventative control example: Consider implementing the default resource perimeter SCP, [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json), which lists service-owned layers in the `NotResource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access.

If you want to restrict access to your networks to trusted resources, consider implementing this control:
* Preventative control example: Consider implementing the service-specific VPC endpoint policy, [lambda_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/lambda_endpoint_policy.json), which lists service-owned layers in the `Resource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that might be accessed from your networks.

## UpdateCodeSigningConfig
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [UpdateCodeSigningConfig](https://docs.aws.amazon.com/lambda/latest/api/API_UpdateCodeSigningConfig.html) allows you to specify an AWS Signer signing profile that does not belong to your organization as the value for the `AllowedPublishers.SigningProfileVersionArns` request parameter. Because the subsequent call against the AWS Signer signing profile is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [AllowedPublishers.SigningProfileVersionArns](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-lambda-codesigningconfig-allowedpublishers.html#cfn-lambda-codesigningconfig-allowedpublishers-signingprofileversionarns) property that does not belong to your organization for the [AWS::Lambda::CodeSigningConfig](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-lambda-codesigningconfig.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateCodeSigningConfig](https://docs.aws.amazon.com/lambda/latest/api/API_UpdateCodeSigningConfig.html) API calls in your environment (specifically, the [AllowedPublishers.SigningProfileVersionArns](https://docs.aws.amazon.com/lambda/latest/api/API_AllowedPublishers.html#lambda-Type-AllowedPublishers-SigningProfileVersionArns) request parameter). If necessary, remediate with the responsive controls of your choice.

## UpdateFunctionConfiguration
### Code execution within a VPC

**Perimeter type applicability**: identity and resource perimeter on network, network perimeter.

**Description**: [UpdateFunctionConfiguration](https://docs.aws.amazon.com/lambda/latest/api/API_UpdateFunctionConfiguration.html) allows you to associate functions with an Amazon VPC to run your code.

**Additional controls**:

If you want to achieve data perimeter control objectives, consider implementing these additional controls:
* Preventative control example: Consider implementing `lambda:VpcIds` in an SCP to help restrict creation of resources to a customer managed VPC only. See [restrict_nonvpc_deployment_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help ensure that developers specify the [VpcConfig](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-lambda-function.html#cfn-lambda-function-vpcconfig) property of the [AWS::Lambda::Function](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-lambda-function.html) resource.
* Detective control example: Consider implementing the AWS Config rule, [lambda-inside-vpc](https://docs.aws.amazon.com/config/latest/developerguide/lambda-inside-vpc.html), to help detect functions not associated with a VPC or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateFunctionConfiguration](https://docs.aws.amazon.com/lambda/latest/api/API_UpdateFunctionConfiguration.html) API calls in your environment (specifically, the [VpcConfig](https://docs.aws.amazon.com/lambda/latest/api/API_UpdateFunctionConfiguration.html#lambda-UpdateFunctionConfiguration-request-VpcConfig) request parameter). If necessary, remediate with the responsive controls of your choice.

### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [UpdateFunctionConfiguration](https://docs.aws.amazon.com/lambda/latest/api/API_UpdateFunctionConfiguration.html) allows you to specify a service role, referred to as the function's execution role, for your Lambda function operations. The service uses the role to make AWS API calls from your code.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the service-specific SCP, [network_perimeter_lambda_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/network_perimeter_lambda_scp.json). This policy lists `logs:CreateLogGroup`, `logs:CreateLogStream`, `logs:PutLogEvents`, `elasticfilesystem:client*`, and `xray:PutTraceSegments` in the `NotAction` element because the service makes direct calls from its network to Amazon CloudWatch Logs, Amazon EFS, and AWS X-Ray on your behalf. If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

## List of service APIs reviewed against data perimeter control objectives

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
* UpdateCodeSigningConfig
* UpdateFunctionCode
* UpdateFunctionConfiguration
* UpdateFunctionUrlConfig
