# Service-specific guidance: AWS CodePipeline


This document outlines service-specific guidance for implementing a data perimeter for AWS CodePipeline.

AWS CodePipeline is a continuous delivery service you can use to model, visualize, and automate the steps required to release your software. CodePipeline automates the steps required to release your software changes continuously.

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

## CreatePipeline
### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [CreatePipeline](https://docs.aws.amazon.com/codepipeline/latest/APIReference/API_CreatePipeline.html) allows you to specify non-AWS action providers as the value for the `provider` request parameter. Because the subsequent requests against the endpoints are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Owner](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-codepipeline-pipeline-actiontypeid.html#cfn-codepipeline-pipeline-actiontypeid-owner) and [Provider](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-codepipeline-pipeline-actiontypeid.html#cfn-codepipeline-pipeline-actiontypeid-provider) properties that do not belong to your organization for the [AWS::CodePipeline::Pipeline](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-codepipeline-pipeline.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [CreatePipeline](https://docs.aws.amazon.com/codepipeline/latest/APIReference/API_CreatePipeline.html) API calls in your environment (specifically, the [owner](https://docs.aws.amazon.com/codepipeline/latest/APIReference/API_ActionTypeId.html#CodePipeline-Type-ActionTypeId-owner) and [provider](https://docs.aws.amazon.com/codepipeline/latest/APIReference/API_ActionTypeId.html#CodePipeline-Type-ActionTypeId-provider) request parameters). If necessary, remediate with the responsive controls of your choice.

### Code execution within a VPC

**Perimeter type applicability**: identity and resource perimeter on network, network perimeter.

**Description**: [CreatePipeline](https://docs.aws.amazon.com/codepipeline/latest/APIReference/API_CreatePipeline.html) allows you to associate a Commands action with an Amazon VPC to run your code.

**Additional controls**:

If you want to achieve data perimeter control objectives, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help ensure that developers specify `VpcId` in the [Configuration](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-codepipeline-pipeline-actiondeclaration.html#cfn-codepipeline-pipeline-actiondeclaration-configuration) property of the [AWS::CodePipeline::Pipeline](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-codepipeline-pipeline.html) resource.
* Detective control example: Consider implementing a custom AWS Config rule to help detect misconfigurations or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* Detective control example: Consider using CloudTrail management events to monitor the [CreatePipeline](https://docs.aws.amazon.com/codepipeline/latest/APIReference/API_CreatePipeline.html) API calls in your environment (specifically, the [actionTypeId](https://docs.aws.amazon.com/codepipeline/latest/APIReference/API_ActionDeclaration.html#CodePipeline-Type-ActionDeclaration-actionTypeId) and [configuration](https://docs.aws.amazon.com/codepipeline/latest/APIReference/API_ActionDeclaration.html#CodePipeline-Type-ActionDeclaration-configuration) request parameters). If necessary, remediate with the responsive controls of your choice.

### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [CreatePipeline](https://docs.aws.amazon.com/codepipeline/latest/APIReference/API_CreatePipeline.html) allows you to specify a service role, referred to as the CodePipeline service role, for your Commands action operations. The service uses the role to make AWS API calls from your code.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json) or [network_perimeter_vpceorgid_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_vpceorgid_scp.json). These policies demonstrate how to use the `dp:include:network` tag set to `true` to restrict calls made by the CodePipeline service role. If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

## UpdatePipeline
### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [UpdatePipeline](https://docs.aws.amazon.com/codepipeline/latest/APIReference/API_UpdatePipeline.html) allows you to specify non-AWS action providers as the value for the `provider` request parameter. Because the subsequent requests against the endpoints are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Owner](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-codepipeline-pipeline-actiontypeid.html#cfn-codepipeline-pipeline-actiontypeid-owner) and [Provider](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-codepipeline-pipeline-actiontypeid.html#cfn-codepipeline-pipeline-actiontypeid-provider) properties that do not belong to your organization for the [AWS::CodePipeline::Pipeline](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-codepipeline-pipeline.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdatePipeline](https://docs.aws.amazon.com/codepipeline/latest/APIReference/API_UpdatePipeline.html) API calls in your environment (specifically, the [owner](https://docs.aws.amazon.com/codepipeline/latest/APIReference/API_ActionTypeId.html#CodePipeline-Type-ActionTypeId-owner) and [provider](https://docs.aws.amazon.com/codepipeline/latest/APIReference/API_ActionTypeId.html#CodePipeline-Type-ActionTypeId-provider) request parameters). If necessary, remediate with the responsive controls of your choice.

### Code execution within a VPC

**Perimeter type applicability**: identity and resource perimeter on network, network perimeter.

**Description**: [UpdatePipeline](https://docs.aws.amazon.com/codepipeline/latest/APIReference/API_UpdatePipeline.html) allows you to associate a Commands action with an Amazon VPC to run your code.

**Additional controls**:

If you want to achieve data perimeter control objectives, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help ensure that developers specify `VpcId` in the [Configuration](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-codepipeline-pipeline-actiondeclaration.html#cfn-codepipeline-pipeline-actiondeclaration-configuration) property of the [AWS::CodePipeline::Pipeline](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-codepipeline-pipeline.html) resource.
* Detective control example: Consider implementing a custom AWS Config rule to help detect misconfigurations or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdatePipeline](https://docs.aws.amazon.com/codepipeline/latest/APIReference/API_UpdatePipeline.html) API calls in your environment (specifically, the [actionTypeId](https://docs.aws.amazon.com/codepipeline/latest/APIReference/API_ActionDeclaration.html#CodePipeline-Type-ActionDeclaration-actionTypeId) and [configuration](https://docs.aws.amazon.com/codepipeline/latest/APIReference/API_ActionDeclaration.html#CodePipeline-Type-ActionDeclaration-configuration) request parameters). If necessary, remediate with the responsive controls of your choice.

### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [UpdatePipeline](https://docs.aws.amazon.com/codepipeline/latest/APIReference/API_UpdatePipeline.html) allows you to specify a service role, referred to as the CodePipeline service role, for your Commands action operations. The service uses the role to make AWS API calls from your code.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json) or [network_perimeter_vpceorgid_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_vpceorgid_scp.json). These policies demonstrate how to use the `dp:include:network` tag set to `true` to restrict calls made by the CodePipeline service role. If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

## List of service APIs reviewed against data perimeter control objectives

* CreatePipeline
* DeleteCustomActionType
* DeletePipeline
* DisableStageTransition
* EnableStageTransition
* GetPipeline
* GetPipelineExecution
* GetPipelineState
* ListActionExecutions
* ListActionTypes
* ListDeployActionExecutionTargets
* ListPipelineExecutions
* ListPipelines
* ListRuleExecutions
* ListRuleTypes
* ListTagsForResource
* ListWebhooks
* PutActionRevision
* RetryStageExecution
* StartPipelineExecution
* StopPipelineExecution
* TagResource
* UntagResource
* UpdatePipeline
