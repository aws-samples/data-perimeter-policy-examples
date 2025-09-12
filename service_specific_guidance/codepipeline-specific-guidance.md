
# Service-specific guidance: AWS CodePipeline


This document outlines service-specific guidance for implementing a data perimeter for AWS CodePipeline. 

AWS CodePipeline is a fully managed continuous delivery service that helps you automate your software release processes. It enables you to model, visualize, and automate the steps required to release your software, allowing you to rapidly and reliably deliver features and updates. CodePipeline integrates with various AWS services and third-party tools, making it easy to build, test, and deploy your code every time there's a change.


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

Perimeter type applicability: all.

UpdatePipeline allows you to associate a Commands action with an Amazon VPC to run your code.

If you want to achieve data perimeter control objectives, consider implementing these additional controls:

* **Proactive control example:** Consider implementing CloudFormation Hooks to help ensure that developers specify VpcId in the [Configuration](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-codepipeline-pipeline-actiondeclaration.html#cfn-codepipeline-pipeline-actiondeclaration-configuration) property of the [AWS::CodePipeline::Pipeline](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-codepipeline-pipeline.html) resource.

* **Detective control example:** Consider implementing a custom AWS Config rule to help detect misconfigurations or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.

* **Detective control example:** Consider using CloudTrail management events to monitor the [UpdatePipeline](https://docs.aws.amazon.com/codepipeline/latest/APIReference/API_UpdatePipeline.html) API call in your environment (specifically, the [actionTypeId](https://docs.aws.amazon.com/codepipeline/latest/APIReference/API_ActionDeclaration.html#CodePipeline-Type-ActionDeclaration-actionTypeId) and [configuration](https://docs.aws.amazon.com/codepipeline/latest/APIReference/API_ActionDeclaration.html#CodePipeline-Type-ActionDeclaration-configuration) request parameters). If necessary, remediate with the responsive controls of your choice.


**Additional consideration 2**

Perimeter type applicability: all.

CreatePipeline allows you to associate a Commands action with an Amazon VPC to run your code.

If you want to achieve data perimeter control objectives, consider implementing these additional controls:

* **Proactive control example:** Consider implementing CloudFormation Hooks to help ensure that developers specify VpcId in the [Configuration](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-codepipeline-pipeline-actiondeclaration.html#cfn-codepipeline-pipeline-actiondeclaration-configuration) property of the [AWS::CodePipeline::Pipeline](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-codepipeline-pipeline.html) resource.

* **Detective control example:** Consider implementing a custom AWS Config rule to help detect misconfigurations or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.

* **Detective control example:** Consider using CloudTrail management events to monitor the [CreatePipeline](https://docs.aws.amazon.com/codepipeline/latest/APIReference/API_CreatePipeline.html) API call in your environment (specifically, the [actionTypeId](https://docs.aws.amazon.com/codepipeline/latest/APIReference/API_ActionDeclaration.html#CodePipeline-Type-ActionDeclaration-actionTypeId) and [configuration](https://docs.aws.amazon.com/codepipeline/latest/APIReference/API_ActionDeclaration.html#CodePipeline-Type-ActionDeclaration-configuration) request parameters). If necessary, remediate with the responsive controls of your choice.


**Additional consideration 3**

Perimeter type applicability: resource perimeter applied on identity.

UpdatePipeline allows you to specify action providers for which the Owner is not AWS as the value for the provider request parameter. Because the subsequent requests against the endpoints are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls:

* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Owner](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-codepipeline-pipeline-actiontypeid.html#cfn-codepipeline-pipeline-actiontypeid-owner) and [Provider](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-codepipeline-pipeline-actiontypeid.html#cfn-codepipeline-pipeline-actiontypeid-provider) properties that do not belong to your organization for the [AWS::CodePipeline::Pipeline](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-codepipeline-pipeline.html) resource.

* **Detective control example:** Consider using CloudTrail management events to monitor the [UpdatePipeline](https://docs.aws.amazon.com/codepipeline/latest/APIReference/API_UpdatePipeline.html) API calls in your environment (specifically, the [owner](https://docs.aws.amazon.com/codepipeline/latest/APIReference/API_ActionTypeId.html#CodePipeline-Type-ActionTypeId-owner) and [provider](https://docs.aws.amazon.com/codepipeline/latest/APIReference/API_ActionTypeId.html#CodePipeline-Type-ActionTypeId-provider) request parameters). If necessary, remediate with the responsive controls of your choice.


**Additional consideration 4**

Perimeter type applicability: resource perimeter applied on identity.

CreatePipeline allows you to specify action providers for which the Owner is not AWS as the value for the provider request parameter. Because the subsequent requests against the endpoints are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls:

* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Owner](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-codepipeline-pipeline-actiontypeid.html#cfn-codepipeline-pipeline-actiontypeid-owner) and [Provider](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-codepipeline-pipeline-actiontypeid.html#cfn-codepipeline-pipeline-actiontypeid-provider) properties that do not belong to your organization for the [AWS::CodePipeline::Pipeline](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-codepipeline-pipeline.html) resource.

* **Detective control example:**
Consider using CloudTrail management events to monitor the [CreatePipeline](https://docs.aws.amazon.com/codepipeline/latest/APIReference/API_CreatePipeline.html) API calls in your environment (specifically, the [owner](https://docs.aws.amazon.com/codepipeline/latest/APIReference/API_ActionTypeId.html#CodePipeline-Type-ActionTypeId-owner) and [provider](https://docs.aws.amazon.com/codepipeline/latest/APIReference/API_ActionTypeId.html#CodePipeline-Type-ActionTypeId-provider) request parameters). If necessary, remediate with the responsive controls of your choice.

**List of service APIs reviewed against data perimeter control objectives**

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
