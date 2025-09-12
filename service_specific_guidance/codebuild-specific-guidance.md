
# Service-specific guidance: AWS CodeBuild


This document outlines service-specific guidance for implementing a data perimeter for AWS CodeBuild. 

AWS CodeBuild is a fully managed continuous integration service that compiles source code, runs tests, and produces software packages that are ready for deployment. It eliminates the need to provision, manage, and scale your own build servers, allowing developers to focus on writing code rather than managing infrastructure. CodeBuild supports various programming languages and build tools, integrates seamlessly with other AWS services, and can be used as part of a continuous delivery pipeline.


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
        
UpdateProject allows you to associate projects with an Amazon VPC to run your code.

If you want to achieve data perimeter control objectives, consider implementing these additional controls:

* **Preventative control example:** Consider implementing `codebuild:vpcConfig.vpcId` in an SCP to help restrict creation of resources to a customer managed VPC only. See [restrict_nonvpc_deployment_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.
* **Proactive control example:** Consider implementing CloudFormation Hooks to help ensure that developers specify the [VpcConfig](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-codebuild-project.html#cfn-codebuild-project-vpcconfig) property of the [AWS::CodeBuild::Project](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-codebuild-project.html) resource.
* **Detective control example 1:** Consider implementing a custom AWS Config rule to check if CodeBuild projects are associated with a VPC, or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* **Detective control example 2:** Consider using CloudTrail management events to monitor the [UpdateProject](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_UpdateProject.html) API calls in your environment (specifically, the [vpcConfig](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_UpdateProject.html#CodeBuild-UpdateProject-request-vpcConfig) request parameter). If necessary, remediate with the responsive controls of your choice.


**Additional consideration 2**

Perimeter type applicability: all.
        
UpdateFleet allows you to associate compute fleets with an Amazon VPC to run your code.

If you want to achieve data perimeter control objectives, consider implementing these additional controls:

* **Preventative control example:** Consider implementing `codebuild:vpcConfig.vpcId` in an SCP to help restrict creation of resources to a customer managed VPC only. See [restrict_nonvpc_deployment_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.
* **Proactive control example:** Consider implementing CloudFormation Hooks to help ensure that developers specify the [FleetVpcConfig](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-codebuild-fleet.html#cfn-codebuild-fleet-fleetvpcconfig) property of the [AWS::CodeBuild::Fleet](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-codebuild-fleet.html) resource.
* **Detective control example 1:** Consider implementing a custom AWS Config rule to check if CodeBuild fleets are associated with a VPC, or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* **Detective control example 2:** Consider using CloudTrail management events to monitor the [UpdateFleet](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_UpdateFleet.html) API calls in your environment (specifically, the [vpcConfig](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_UpdateFleet.html#CodeBuild-UpdateFleet-request-vpcConfig) request parameter). If necessary, remediate with the responsive controls of your choice.


**Additional consideration 3**

Perimeter type applicability: all.
        
CreateProject allows you to associate projects with an Amazon VPC to run your code.


If you want to achieve data perimeter control objectives, consider implementing these additional controls:

* **Preventative control example:** Consider implementing `codebuild:vpcConfig.vpcId` in an SCP to help restrict creation of resources to a customer managed VPC only. See [restrict_nonvpc_deployment_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.
* **Proactive control example:** Consider implementing CloudFormation Hooks to help ensure that developers specify the [VpcConfig](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-codebuild-project.html#cfn-codebuild-project-vpcconfig) property of the [AWS::CodeBuild::Project](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-codebuild-project.html) resource.
* **Detective control example 1:** Consider implementing a custom AWS Config rule to check if CodeBuild projects are associated with a VPC, or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* **Detective control example 2:** Consider using CloudTrail management events to monitor the [CreateProject](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_CreateProject.html) API calls in your environment (specifically, the [vpcConfig](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_CreateProject.html#CodeBuild-CreateProject-request-vpcConfig) request parameter). If necessary, remediate with the responsive controls of your choice.



**Additional consideration 4**

Perimeter type applicability: all.
        
CreateFleet allows you to associate compute fleets with an Amazon VPC to run your code.

If you want to achieve data perimeter control objectives, consider implementing these additional controls:

* **Preventative control example:** Consider implementing `codebuild:vpcConfig.vpcId` in an SCP to help restrict creation of resources to a customer managed VPC only. See [restrict_nonvpc_deployment_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.
* **Proactive control example:** Consider implementing CloudFormation Hooks to help ensure that developers specify the [FleetVpcConfig](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-codebuild-fleet.html#cfn-codebuild-fleet-fleetvpcconfig) property of the [AWS::CodeBuild::Fleet](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-codebuild-fleet.html) resource.
* **Detective control example 1:** Consider implementing a custom AWS Config rule to check if CodeBuild fleets are associated with a VPC, or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* **Detective control example 2:** Consider using CloudTrail management events to monitor the [CreateFleet](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_CreateFleet.html) API calls in your environment (specifically, the [vpcConfig](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_CreateFleet.html#CodeBuild-CreateFleet-request-vpcConfig) request parameter). If necessary, remediate with the responsive controls of your choice.


**Additional consideration 5**

Perimeter type applicability: identity and network perimeter applied on resource.
        
PutResourcePolicy allows you to apply a resource-based policy to grant access to a project or report group. The service currently doesn’t support RCPs.


If you want to restrict access to trusted identities and expected networks, consider implementing these additional controls:

* **Preventative control example**: Consider restricting [PutResourcePolicy](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_PutResourcePolicy.html) permissions to administrators only using an SCP. See [restrict_resource_policy_configurations_scp.json](../service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* **Detective control example:** Consider using CloudTrail management events to monitor the [PutResourcePolicy](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_PutResourcePolicy.html) API calls in your environment (specifically, the [policy](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_PutResourcePolicy.html#CodeBuild-PutResourcePolicy-request-policy) request parameter).


**Additional consideration 6**

Perimeter type applicability: all.

UpdateProject allows you to specify Lambda as the compute type, but it does not currently support Lambda functions creation within your Amazon VPC.

If you want to achieve data perimeter control objectives, consider implementing these additional controls:

* **Preventative control example:** Consider implementing `lambda:VpcIds` in an SCP to help restrict creation of resources to a customer managed VPC only. See [restrict_nonvpc_deployment_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.
* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying Lambda as the build environment type in the [Environment](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-codebuild-project.html#cfn-codebuild-project-environment) property of the [AWS::CodeBuild::Project](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-codebuild-project.html) resource.
* **Detective control example 1:** Consider implementing a custom AWS Config rule to check if CodeBuild projects use Lambda as the build environment type, or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* **Detective control example 2:** Consider using CloudTrail management events to monitor the [UpdateProject](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_UpdateProject.html) API calls in your environment (specifically, the [environment](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_UpdateProject.html#CodeBuild-UpdateProject-request-environment) request parameter). If necessary, remediate with the responsive controls of your choice.


**Additional consideration 7**

Perimeter type applicability: all.

UpdateFleet allows you to specify Lambda as the compute type, but it does not currently support Lambda functions creation within your Amazon VPC.

If you want to achieve data perimeter control objectives, consider implementing these additional controls:

* **Preventative control example:** Consider implementing `lambda:VpcIds` in an SCP to help restrict creation of resources to a customer managed VPC only. See [restrict_nonvpc_deployment_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.
* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying Lambda as the build environment type in the [EnvironmentType](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-codebuild-fleet.html#cfn-codebuild-fleet-environmenttype) property of the [AWS::CodeBuild::Fleet](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-codebuild-fleet.html) resource.
* **Detective control example 1:** Consider implementing a custom AWS Config rule to check if CodeBuild fleets use Lambda as the build environment type, or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* **Detective control example 2:** Consider using CloudTrail management events to monitor the [UpdateFleet](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_UpdateFleet.html) API calls in your environment (specifically, the [environmentType](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_UpdateFleet.html#CodeBuild-UpdateFleet-request-environmentType) request parameter). If necessary, remediate with the responsive controls of your choice.

**Additional consideration 8**

Perimeter type applicability: all.

CreateFleet allows you to specify Lambda as the compute type, but it does not currently support Lambda functions creation within your Amazon VPC.

If you want to achieve data perimeter control objectives, consider implementing these additional controls:

* **Preventative control example:** Consider implementing `lambda:VpcIds` in an SCP to help restrict creation of resources to a customer managed VPC only. See [restrict_nonvpc_deployment_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.
* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying Lambda as the build environment type in the [EnvironmentType](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-codebuild-fleet.html#cfn-codebuild-fleet-environmenttype) property of the [AWS::CodeBuild::Fleet](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-codebuild-fleet.html) resource.
* **Detective control example 1:** Consider implementing a custom AWS Config rule to check if CodeBuild fleets use Lambda as the build environment type, or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* **Detective control example 2:** Consider using CloudTrail management events to monitor the [CreateFleet](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_CreateFleet.html) API calls in your environment (specifically, the [environmentType](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_CreateFleet.html#CodeBuild-CreateFleet-request-environmentType) request parameter). If necessary, remediate with the responsive controls of your choice.


**Additional consideration 9**

Perimeter type applicability: all.

CreateProject allows you to specify Lambda as the compute type, but it does not currently support Lambda functions creation within your Amazon VPC.

If you want to achieve data perimeter control objectives, consider implementing these additional controls:

* **Preventative control example:** Consider implementing `lambda:VpcIds` in an SCP to help restrict creation of resources to a customer managed VPC only. See [restrict_nonvpc_deployment_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.
* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying Lambda as the build environment type in the [Environment](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-codebuild-project.html#cfn-codebuild-project-environment) property of the [AWS::CodeBuild::Project](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-codebuild-project.html) resource.
* **Detective control example 1:** Consider implementing a custom AWS Config rule to check if CodeBuild projects use Lambda as the build environment type, or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* **Detective control example 2:** Consider using CloudTrail management events to monitor the [CreateProject](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_CreateProject.html) API calls in your environment (specifically, the [environment](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_CreateProject.html#CodeBuild-CreateProject-request-environment) request parameter). If necessary, remediate with the responsive controls of your choice.


**Additional consideration 10**

Perimeter type applicability: all.

StartBuild allows you to specify Lambda as the compute type, but it does not currently support Lambda functions creation within your Amazon VPC.

If you want to achieve data perimeter control objectives, consider implementing these additional controls:

* **Preventative control example:** Consider implementing `lambda:VpcIds` in an SCP to help restrict creation of resources to a customer managed VPC only. See [restrict_nonvpc_deployment_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.
* **Detective control example:** Consider using CloudTrail management events to monitor the [StartBuild](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html) API calls in your environment (specifically, the [environmentTypeOverride](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#CodeBuild-StartBuild-request-environmentTypeOverride) request parameter). If necessary, remediate with the responsive controls of your choice.


**Additional consideration 11**

Perimeter type applicability: all.

StartBuildBatch allows you to specify Lambda as the compute type, but it does not currently support Lambda functions creation within your Amazon VPC.

If you want to achieve data perimeter control objectives, consider implementing these additional controls:

* **Preventative control example:** Consider implementing `lambda:VpcIds` in an SCP to help restrict creation of resources to a customer managed VPC only. See [restrict_nonvpc_deployment_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.
* **Detective control example:** Consider using CloudTrail management events to monitor the [StartBuildBatch](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html) API calls in your environment (specifically, the [environmentTypeOverride](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#CodeBuild-StartBuildBatch-request-environmentTypeOverride) request parameter). If necessary, remediate with the responsive controls of your choice.

**List of service APIs reviewed against data perimeter control objectives**

* BatchDeleteBuilds
* BatchGetBuildBatches
* BatchGetBuilds
* BatchGetFleets
* BatchGetProjects
* BatchGetReportGroups
* BatchGetReports
* CreateFleet
* CreateProject
* CreateReportGroup
* DeleteBuildBatch
* DeleteFleet
* DeleteProject
* DeleteReport
* DeleteReportGroup
* DeleteResourcePolicy
* DeleteSourceCredentials
* DescribeCodeCoverages
* DescribeTestCases
* GetReportGroupTrend
* GetResourcePolicy
* ImportSourceCredentials
* InvalidateProjectCache
* ListBuildBatches
* ListBuildBatchesForProject
* ListBuilds
* ListBuildsForProject
* ListCuratedEnvironmentImages
* ListFleets
* ListProjects
* ListReportGroups
* ListReports
* ListReportsForReportGroup
* ListSharedProjects
* ListSharedReportGroups
* ListSourceCredentials
* PutResourcePolicy
* RetryBuild
* StartBuild
* StartBuildBatch
* StopBuild
* StopBuildBatch
* UpdateFleet
* UpdateProject
* UpdateProjectVisibility
* UpdateReportGroup
