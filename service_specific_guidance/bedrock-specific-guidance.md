# Service-specific guidance: Amazon Bedrock


This document outlines service-specific guidance for implementing a data perimeter for Amazon Bedrock.

Amazon Bedrock is a fully managed service that provides secure, enterprise-grade access to high-performing foundation models from leading AI companies, enabling you to build and scale generative AI applications. Using Amazon Bedrock, you can experiment with and evaluate foundation models for your use case, privately customize them with your data using techniques such as fine-tuning and Retrieval Augmented Generation (RAG), and build agents that execute tasks using your enterprise systems and data sources.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | Y |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | Y |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | Y |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | N |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | Y |

*Y - Additional considerations apply. N - No additional considerations apply.

## CreateModelInvocationJob
### Access to service-owned resources

**Perimeter type applicability**: resource perimeter.

**Description**: [CreateModelInvocationJob](https://docs.aws.amazon.com/bedrock/latest/APIReference/API_CreateModelInvocationJob.html) might require access to service-owned foundation models, which are foundation models that are managed in service accounts.

**Additional controls**:

If you want to restrict access to trusted resources for your identities, consider implementing this control:
* Preventative control example: Consider implementing the default resource perimeter SCP, [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json), which lists service-owned foundation models in the `NotResource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access.

If you want to restrict access to your networks to trusted resources, consider implementing this control:
* Preventative control example: Consider implementing the service-specific VPC endpoint policy, [bedrock_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/bedrock_endpoint_policy.json), which lists service-owned foundation models in the `Resource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that might be accessed from your networks.

## PutResourcePolicy
### No RCP support

**Perimeter type applicability**: identity and network perimeter applied on resource.

**Description**: [PutResourcePolicy](https://docs.aws.amazon.com/bedrock/latest/APIReference/API_PutResourcePolicy.html) allows you to apply a resource-based policy to grant access to Bedrock resources. The service currently does not support RCPs.

**Additional controls**:

If you want to restrict access to trusted identities, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutResourcePolicy](https://docs.aws.amazon.com/bedrock/latest/APIReference/API_PutResourcePolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [PolicyDocument](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-bedrock-resourcepolicy.html#cfn-bedrock-resourcepolicy-policydocument) property for the [AWS::Bedrock::ResourcePolicy](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-bedrock-resourcepolicy.html) resource that grants permissions to untrusted identities.
* Detective control example: Consider using CloudTrail management events to monitor the [PutResourcePolicy](https://docs.aws.amazon.com/bedrock/latest/APIReference/API_PutResourcePolicy.html) API calls in your environment (specifically, the [resourcePolicy](https://docs.aws.amazon.com/bedrock/latest/APIReference/API_PutResourcePolicy.html#bedrock-PutResourcePolicy-request-resourcePolicy) request parameter). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

The service APIs covered in this review are based on botocore version [1.43.29](https://github.com/boto/botocore/tree/1.43.29/botocore).

* BatchDeleteAdvancedPromptOptimizationJob
* CancelAutomatedReasoningPolicyBuildWorkflow
* CreateAdvancedPromptOptimizationJob
* CreateAutomatedReasoningPolicy
* CreateAutomatedReasoningPolicyTestCase
* CreateAutomatedReasoningPolicyVersion
* CreateCustomModel
* CreateCustomModelDeployment
* CreateEvaluationJob
* CreateGuardrail
* CreateGuardrailVersion
* CreateInferenceProfile
* CreateMarketplaceModelEndpoint
* CreateModelCopyJob
* CreateModelCustomizationJob
* CreateModelImportJob
* CreateModelInvocationJob
* CreatePromptRouter
* DeleteAutomatedReasoningPolicy
* DeleteAutomatedReasoningPolicyBuildWorkflow
* DeleteAutomatedReasoningPolicyTestCase
* DeleteEnforcedGuardrailConfiguration
* DeleteFoundationModelAgreement
* DeleteGuardrail
* DeleteInferenceProfile
* DeleteModelInvocationLoggingConfiguration
* DeletePromptRouter
* DeleteResourcePolicy
* ExportAutomatedReasoningPolicyVersion
* GetAccountDataRetention
* GetAdvancedPromptOptimizationJob
* GetAutomatedReasoningPolicy
* GetAutomatedReasoningPolicyAnnotations
* GetAutomatedReasoningPolicyBuildWorkflow
* GetAutomatedReasoningPolicyBuildWorkflowResultAssets
* GetAutomatedReasoningPolicyTestCase
* GetAutomatedReasoningPolicyTestResult
* GetEvaluationJob
* GetFoundationModel
* GetFoundationModelAvailability
* GetGuardrail
* GetInferenceProfile
* GetMarketplaceModelEndpoint
* GetModelCopyJob
* GetModelCustomizationJob
* GetModelInvocationJob
* GetModelInvocationLoggingConfiguration
* GetPromptRouter
* GetResourcePolicy
* GetUseCaseForModelAccess
* ListAdvancedPromptOptimizationJobs
* ListAutomatedReasoningPolicies
* ListAutomatedReasoningPolicyBuildWorkflows
* ListAutomatedReasoningPolicyTestCases
* ListAutomatedReasoningPolicyTestResults
* ListCustomModelDeployments
* ListCustomModels
* ListEnforcedGuardrailsConfiguration
* ListEvaluationJobs
* ListFoundationModelAgreementOffers
* ListFoundationModels
* ListGuardrails
* ListImportedModels
* ListInferenceProfiles
* ListMarketplaceModelEndpoints
* ListModelCopyJobs
* ListModelCustomizationJobs
* ListModelImportJobs
* ListModelInvocationJobs
* ListPromptRouters
* ListProvisionedModelThroughputs
* ListTagsForResource
* PutAccountDataRetention
* PutEnforcedGuardrailConfiguration
* PutModelInvocationLoggingConfiguration
* PutResourcePolicy
* RegisterMarketplaceModelEndpoint
* StartAutomatedReasoningPolicyBuildWorkflow
* StartAutomatedReasoningPolicyTestWorkflow
* StopAdvancedPromptOptimizationJob
* StopEvaluationJob
* StopModelCustomizationJob
* TagResource
* UntagResource
* UpdateAutomatedReasoningPolicy
* UpdateAutomatedReasoningPolicyAnnotations
* UpdateAutomatedReasoningPolicyTestCase
* UpdateGuardrail
* UpdateMarketplaceModelEndpoint
