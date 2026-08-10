# Service-specific guidance: Amazon EC2 Image Builder


This document outlines service-specific guidance for implementing a data perimeter for Amazon EC2 Image Builder.

EC2 Image Builder is a fully managed AWS service that helps you to automate the creation, management, and deployment of customized, secure, and up-to-date server images. You can use the AWS Management Console, AWS Command Line Interface, or APIs to create custom images in your AWS account.

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

## CreateImageRecipe
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [CreateImageRecipe](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_CreateImageRecipe.html) allows you to specify an Amazon EC2 image that does not belong to your organization as the value for the `parentImage` request parameter. Because the subsequent call against the Amazon EC2 image is performed by the service-linked role (SLR), it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [ParentImage](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-imagebuilder-imagerecipe.html#cfn-imagebuilder-imagerecipe-parentimage) property that does not belong to your organization for the [AWS::ImageBuilder::ImageRecipe](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-imagebuilder-imagerecipe.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateImageRecipe](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_CreateImageRecipe.html) API calls in your environment (specifically, the [parentImage](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_CreateImageRecipe.html#imagebuilder-CreateImageRecipe-request-parentImage) request parameter). If necessary, remediate with the responsive controls of your choice.

## CreateInfrastructureConfiguration
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [CreateInfrastructureConfiguration](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_CreateInfrastructureConfiguration.html) allows you to specify an Amazon SNS topic that does not belong to your organization as the value for the `snsTopicArn` request parameter. Because the subsequent call against the Amazon SNS topic is performed by the service-linked role (SLR), it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Preventative control example: Consider implementing `imagebuilder:StatusTopicArn` in an SCP to help prevent requests to untrusted resources. See [restrict_untrusted_resources_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_untrusted_resources_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [SnsTopicArn](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-imagebuilder-infrastructureconfiguration.html#cfn-imagebuilder-infrastructureconfiguration-snstopicarn) property that does not belong to your organization for the [AWS::ImageBuilder::InfrastructureConfiguration](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-imagebuilder-infrastructureconfiguration.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateInfrastructureConfiguration](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_CreateInfrastructureConfiguration.html) API calls in your environment (specifically, the [snsTopicArn](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_CreateInfrastructureConfiguration.html#imagebuilder-CreateInfrastructureConfiguration-request-snsTopicArn) request parameter). If necessary, remediate with the responsive controls of your choice.

## GetComponent
### Access to service-owned resources

**Perimeter type applicability**: resource perimeter.

**Description**: [GetComponent](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_GetComponent.html) might require access to service-owned components, which are components that are managed in service accounts.

**Additional controls**:

If you want to restrict access to trusted resources for your identities, consider implementing this control:
* Preventative control example: Consider implementing the default resource perimeter SCP, [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json), which lists service-owned components in the `NotResource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access.

If you want to restrict access to your networks to trusted resources, consider implementing this control:
* Preventative control example: Consider implementing the service-specific VPC endpoint policy, [imagebuilder_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/imagebuilder_endpoint_policy.json), which lists service-owned components in the `Resource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that might be accessed from your networks.

## GetImage
### Access to service-owned resources

**Perimeter type applicability**: resource perimeter.

**Description**: [GetImage](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_GetImage.html) might require access to service-owned images, which are images that are managed in service accounts.

**Additional controls**:

If you want to restrict access to trusted resources for your identities, consider implementing this control:
* Preventative control example: Consider implementing the default resource perimeter SCP, [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json), which lists service-owned images in the `NotResource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access.

If you want to restrict access to your networks to trusted resources, consider implementing this control:
* Preventative control example: Consider implementing the service-specific VPC endpoint policy, [imagebuilder_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/imagebuilder_endpoint_policy.json), which lists service-owned images in the `Resource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that might be accessed from your networks.

## PutComponentPolicy
### No RCP support

**Perimeter type applicability**: identity and network perimeter applied on resource.

**Description**: [PutComponentPolicy](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_PutComponentPolicy.html) allows you to apply a resource-based policy to grant access to a component. The service currently does not support RCPs.

**Additional controls**:

If you want to restrict access to trusted identities, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutComponentPolicy](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_PutComponentPolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [PutComponentPolicy](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_PutComponentPolicy.html) API calls in your environment (specifically, the [policy](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_PutComponentPolicy.html#imagebuilder-PutComponentPolicy-request-policy) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutComponentPolicy](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_PutComponentPolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [PutComponentPolicy](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_PutComponentPolicy.html) API calls in your environment (specifically, the [policy](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_PutComponentPolicy.html#imagebuilder-PutComponentPolicy-request-policy) request parameter). If necessary, remediate with the responsive controls of your choice.

## PutContainerRecipePolicy
### No RCP support

**Perimeter type applicability**: identity and network perimeter applied on resource.

**Description**: [PutContainerRecipePolicy](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_PutContainerRecipePolicy.html) allows you to apply a resource-based policy to grant access to a container recipe. The service currently does not support RCPs.

**Additional controls**:

If you want to restrict access to trusted identities, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutContainerRecipePolicy](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_PutContainerRecipePolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [PutContainerRecipePolicy](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_PutContainerRecipePolicy.html) API calls in your environment (specifically, the [policy](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_PutContainerRecipePolicy.html#imagebuilder-PutContainerRecipePolicy-request-policy) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutContainerRecipePolicy](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_PutContainerRecipePolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [PutContainerRecipePolicy](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_PutContainerRecipePolicy.html) API calls in your environment (specifically, the [policy](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_PutContainerRecipePolicy.html#imagebuilder-PutContainerRecipePolicy-request-policy) request parameter). If necessary, remediate with the responsive controls of your choice.

## PutImagePolicy
### No RCP support

**Perimeter type applicability**: identity and network perimeter applied on resource.

**Description**: [PutImagePolicy](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_PutImagePolicy.html) allows you to apply a resource-based policy to grant access to an image. The service currently does not support RCPs.

**Additional controls**:

If you want to restrict access to trusted identities, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutImagePolicy](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_PutImagePolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [PutImagePolicy](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_PutImagePolicy.html) API calls in your environment (specifically, the [policy](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_PutImagePolicy.html#imagebuilder-PutImagePolicy-request-policy) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutImagePolicy](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_PutImagePolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [PutImagePolicy](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_PutImagePolicy.html) API calls in your environment (specifically, the [policy](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_PutImagePolicy.html#imagebuilder-PutImagePolicy-request-policy) request parameter). If necessary, remediate with the responsive controls of your choice.

## PutImageRecipePolicy
### No RCP support

**Perimeter type applicability**: identity and network perimeter applied on resource.

**Description**: [PutImageRecipePolicy](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_PutImageRecipePolicy.html) allows you to apply a resource-based policy to grant access to an image recipe. The service currently does not support RCPs.

**Additional controls**:

If you want to restrict access to trusted identities, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutImageRecipePolicy](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_PutImageRecipePolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [PutImageRecipePolicy](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_PutImageRecipePolicy.html) API calls in your environment (specifically, the [policy](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_PutImageRecipePolicy.html#imagebuilder-PutImageRecipePolicy-request-policy) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutImageRecipePolicy](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_PutImageRecipePolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [PutImageRecipePolicy](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_PutImageRecipePolicy.html) API calls in your environment (specifically, the [policy](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_PutImageRecipePolicy.html#imagebuilder-PutImageRecipePolicy-request-policy) request parameters). If necessary, remediate with the responsive controls of your choice.

## UpdateInfrastructureConfiguration
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [UpdateInfrastructureConfiguration](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_UpdateInfrastructureConfiguration.html) allows you to specify an Amazon SNS topic that does not belong to your organization as the value for the `snsTopicArn` request parameter. Because the subsequent call against the Amazon SNS topic is performed by the service-linked role (SLR), it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Preventative control example: Consider implementing `imagebuilder:StatusTopicArn` in an SCP to help prevent requests to untrusted resources. See [restrict_untrusted_resources_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_untrusted_resources_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [SnsTopicArn](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-imagebuilder-infrastructureconfiguration.html#cfn-imagebuilder-infrastructureconfiguration-snstopicarn) property that does not belong to your organization for the [AWS::ImageBuilder::InfrastructureConfiguration](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-imagebuilder-infrastructureconfiguration.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateInfrastructureConfiguration](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_UpdateInfrastructureConfiguration.html) API calls in your environment (specifically, the [snsTopicArn](https://docs.aws.amazon.com/imagebuilder/latest/APIReference/API_UpdateInfrastructureConfiguration.html#imagebuilder-UpdateInfrastructureConfiguration-request-snsTopicArn) request parameter). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

* CancelImageCreation
* CancelLifecycleExecution
* CreateComponent
* CreateContainerRecipe
* CreateDistributionConfiguration
* CreateImage
* CreateImagePipeline
* CreateImageRecipe
* CreateInfrastructureConfiguration
* CreateLifecyclePolicy
* CreateWorkflow
* DeleteComponent
* DeleteContainerRecipe
* DeleteDistributionConfiguration
* DeleteImage
* DeleteImagePipeline
* DeleteImageRecipe
* DeleteInfrastructureConfiguration
* DeleteLifecyclePolicy
* DeleteWorkflow
* GetComponent
* GetComponentPolicy
* GetContainerRecipe
* GetContainerRecipePolicy
* GetDistributionConfiguration
* GetImage
* GetImagePipeline
* GetImagePolicy
* GetImageRecipe
* GetImageRecipePolicy
* GetInfrastructureConfiguration
* GetLifecycleExecution
* GetLifecyclePolicy
* GetWorkflow
* ImportComponent
* ImportDiskImage
* ListComponentBuildVersions
* ListComponents
* ListContainerRecipes
* ListDistributionConfigurations
* ListImageBuildVersions
* ListImagePackages
* ListImagePipelineImages
* ListImagePipelines
* ListImageRecipes
* ListImageScanFindingAggregations
* ListImageScanFindings
* ListImages
* ListInfrastructureConfigurations
* ListLifecycleExecutionResources
* ListLifecycleExecutions
* ListLifecyclePolicies
* ListTagsForResource
* ListWaitingWorkflowSteps
* ListWorkflowBuildVersions
* ListWorkflowExecutions
* ListWorkflows
* PutComponentPolicy
* PutContainerRecipePolicy
* PutImagePolicy
* PutImageRecipePolicy
* StartImagePipelineExecution
* StartResourceStateUpdate
* TagResource
* UntagResource
* UpdateDistributionConfiguration
* UpdateImagePipeline
* UpdateInfrastructureConfiguration
* UpdateLifecyclePolicy
