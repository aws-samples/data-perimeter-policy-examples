# Service-specific guidance: AWS CodeBuild


This document outlines service-specific guidance for implementing a data perimeter for AWS CodeBuild.

AWS CodeBuild is a fully managed build service in the cloud that compiles source code, runs unit tests, and produces artifacts that are ready to deploy, eliminating the need to provision, manage, and scale your own build servers.

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

## CreateFleet
### Code execution within a VPC

**Perimeter type applicability**: identity and resource perimeter on network, network perimeter.

**Description**: [CreateFleet](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_CreateFleet.html) allows you to associate compute fleets with an Amazon VPC to run your code.

**Additional controls**:

If you want to achieve data perimeter control objectives, consider implementing these additional controls:
* Preventative control example: Consider implementing `codebuild:vpcConfig.vpcId` in an SCP to help restrict creation of resources to a customer managed VPC only. See [restrict_nonvpc_deployment_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help ensure that developers specify the [FleetVpcConfig](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-codebuild-fleet-vpcconfig.html) property of the [AWS::CodeBuild::Fleet](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-codebuild-fleet.html) resource.
* Detective control example: Consider implementing a custom AWS Config rule to help detect misconfigurations or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateFleet](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_CreateFleet.html) API calls in your environment (specifically, the [vpcConfig](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_VpcConfig.html) request parameter). If necessary, remediate with the responsive controls of your choice.

## CreateProject
### Code execution within a VPC

**Perimeter type applicability**: identity and resource perimeter on network, network perimeter.

**Description**: [CreateProject](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_CreateProject.html) allows you to associate projects with an Amazon VPC to run your code.

**Additional controls**:

If you want to achieve data perimeter control objectives, consider implementing these additional controls:
* Preventative control example: Consider implementing `codebuild:vpcConfig.vpcId` in an SCP to help restrict creation of resources to a customer managed VPC only. See [restrict_nonvpc_deployment_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help ensure that developers specify the [VpcConfig](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-codebuild-project.html#cfn-codebuild-project-vpcconfig) property of the [AWS::CodeBuild::Project](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-codebuild-project.html) resource.
* Detective control example: Consider implementing a custom AWS Config rule to help detect misconfigurations or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateProject](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_CreateProject.html) API calls in your environment (specifically, the [vpcConfig](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_VpcConfig.html) request parameter). If necessary, remediate with the responsive controls of your choice.

### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [CreateProject](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_CreateProject.html) allows you to specify a service role for your build project operations. The service uses the role to make AWS API calls from your code.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json) or [network_perimeter_vpceorgid_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_vpceorgid_scp.json). These policies demonstrate how to use the `dp:include:network` tag set to `true` to restrict calls made by the project's service role. If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

### Code execution within AWS network

**Perimeter type applicability**: identity and resource perimeter on network, network perimeter.

**Description**: [CreateProject](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_CreateProject.html) allows you to specify Lambda as the compute type, but it does not currently support Lambda functions creation within your Amazon VPC.

**Additional controls**:

If you want to achieve data perimeter control objectives, consider implementing these additional controls:
* Preventative control example: Consider implementing `codebuild:vpcConfig.vpcId` in an SCP to help restrict creation of resources to a customer managed VPC only. See [restrict_nonvpc_deployment_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying Lambda as the build environment type in the [Environment](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-codebuild-project.html#cfn-codebuild-project-environment) property of the [AWS::CodeBuild::Project](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-codebuild-project.html) resource.
* Detective control example: Consider implementing a custom AWS Config rule to help detect misconfigurations or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateProject](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_CreateProject.html) API calls in your environment (specifically, the [environment](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_ProjectEnvironment.html) request parameter). If necessary, remediate with the responsive controls of your choice.

## StartBuild
### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [StartBuild](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html) allows you to specify a service role for your CodeBuild project operations. The service uses the role to make AWS API calls from your code.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json) or [network_perimeter_vpceorgid_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_vpceorgid_scp.json). These policies demonstrate how to use the `dp:include:network` tag set to `true` to restrict calls made by the CodeBuild service role. If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

### Code execution within AWS network

**Perimeter type applicability**: identity and resource perimeter on network, network perimeter.

**Description**: [StartBuild](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html) allows you to specify Lambda as the compute type, but it does not currently support Lambda functions creation within your Amazon VPC.

**Additional controls**:

If you want to achieve data perimeter control objectives, consider implementing these additional controls:
* Preventative control example: Consider implementing `codebuild:vpcConfig.vpcId` in an SCP to help restrict creation of resources to a customer managed VPC only. See [restrict_nonvpc_deployment_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [StartBuild](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html) API calls in your environment (specifically, the [environmentTypeOverride](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuild.html#CodeBuild-StartBuild-request-environmentTypeOverride) request parameter). If necessary, remediate with the responsive controls of your choice.

## StartBuildBatch
### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [StartBuildBatch](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html) allows you to specify a service role for your CodeBuild project operations. The service uses the role to make AWS API calls from your code.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json) or [network_perimeter_vpceorgid_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_vpceorgid_scp.json). These policies demonstrate how to use the `dp:include:network` tag set to `true` to restrict calls made by the CodeBuild service role. If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

### Code execution within AWS network

**Perimeter type applicability**: identity and resource perimeter on network, network perimeter.

**Description**: [StartBuildBatch](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html) allows you to specify Lambda as the compute type, but it does not currently support Lambda functions creation within your Amazon VPC.

**Additional controls**:

If you want to achieve data perimeter control objectives, consider implementing these additional controls:
* Preventative control example: Consider implementing `codebuild:vpcConfig.vpcId` in an SCP to help restrict creation of resources to a customer managed VPC only. See [restrict_nonvpc_deployment_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [StartBuildBatch](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html) API calls in your environment (specifically, the [environmentTypeOverride](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_StartBuildBatch.html#CodeBuild-StartBuildBatch-request-environmentTypeOverride) request parameter). If necessary, remediate with the responsive controls of your choice.

## UpdateFleet
### Code execution within a VPC

**Perimeter type applicability**: identity and resource perimeter on network, network perimeter.

**Description**: [UpdateFleet](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_UpdateFleet.html) allows you to associate compute fleets with an Amazon VPC to run your code.

**Additional controls**:

If you want to achieve data perimeter control objectives, consider implementing these additional controls:
* Preventative control example: Consider implementing `codebuild:vpcConfig.vpcId` in an SCP to help restrict creation of resources to a customer managed VPC only. See [restrict_nonvpc_deployment_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help ensure that developers specify the [FleetVpcConfig](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-codebuild-fleet-vpcconfig.html) property of the [AWS::CodeBuild::Fleet](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-codebuild-fleet.html) resource.
* Detective control example: Consider implementing a custom AWS Config rule to help detect misconfigurations or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateFleet](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_UpdateFleet.html) API calls in your environment (specifically, the [vpcConfig](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_VpcConfig.html) request parameter). If necessary, remediate with the responsive controls of your choice.

## UpdateProject
### Code execution within a VPC

**Perimeter type applicability**: identity and resource perimeter on network, network perimeter.

**Description**: [UpdateProject](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_UpdateProject.html) allows you to associate projects with an Amazon VPC to run your code.

**Additional controls**:

If you want to achieve data perimeter control objectives, consider implementing these additional controls:
* Preventative control example: Consider implementing `codebuild:vpcConfig.vpcId` in an SCP to help restrict creation of resources to a customer managed VPC only. See [restrict_nonvpc_deployment_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help ensure that developers specify the [VpcConfig](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-codebuild-project.html#cfn-codebuild-project-vpcconfig) property of the [AWS::CodeBuild::Project](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-codebuild-project.html) resource.
* Detective control example: Consider implementing a custom AWS Config rule to help detect misconfigurations or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateProject](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_UpdateProject.html) API calls in your environment (specifically, the [vpcConfig](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_VpcConfig.html) request parameter). If necessary, remediate with the responsive controls of your choice.

### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [UpdateProject](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_UpdateProject.html) allows you to specify a service role for your build project operations. The service uses the role to make AWS API calls from your code.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json) or [network_perimeter_vpceorgid_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_vpceorgid_scp.json). These policies demonstrate how to use the `dp:include:network` tag set to `true` to restrict calls made by the CodeBuild service role. If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

### Code execution within AWS network

**Perimeter type applicability**: identity and resource perimeter on network, network perimeter.

**Description**: [UpdateProject](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_UpdateProject.html) allows you to specify Lambda as the compute type, but it does not currently support Lambda functions creation within your Amazon VPC.

**Additional controls**:

If you want to achieve data perimeter control objectives, consider implementing these additional controls:
* Preventative control example: Consider implementing `codebuild:vpcConfig.vpcId` in an SCP to help restrict creation of resources to a customer managed VPC only. See [restrict_nonvpc_deployment_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying Lambda as the build environment type in the [Environment](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-codebuild-project.html#cfn-codebuild-project-environment) property of the [AWS::CodeBuild::Project](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-codebuild-project.html) resource.
* Detective control example: Consider implementing a custom AWS Config rule to help detect misconfigurations or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateProject](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_UpdateProject.html) API calls in your environment (specifically, the [environment](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_UpdateProject.html#CodeBuild-UpdateProject-request-environment) request parameter). If necessary, remediate with the responsive controls of your choice.

## UpdateProjectVisibility
### Service sharing mechanism

**Perimeter type applicability**: identity perimeter applied on resource; resource perimeter applied on identity.

**Description**: [UpdateProjectVisibility](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_UpdateProjectVisibility.html) allows you to make build results, logs, and artifacts available to the general public.

**Additional controls**:

If you want to restrict access so that only trusted identities can view information about your resources, consider implementing these additional controls:
* Preventative control example: Consider implementing `codebuild:projectVisibility` in an SCP to help prevent sharing with untrusted identities. See [data_perimeter_governance_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/data_perimeter_governance_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Visibility](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-codebuild-project.html#cfn-codebuild-project-visibility) property that grants permissions to untrusted identities for the [AWS::CodeBuild::Project](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-codebuild-project.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateProjectVisibility](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_UpdateProjectVisibility.html) API calls in your environment (specifically, the [projectVisibility](https://docs.aws.amazon.com/codebuild/latest/APIReference/API_UpdateProjectVisibility.html#CodeBuild-UpdateProjectVisibility-request-projectVisibility) request parameter). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

* BatchDeleteBuilds
* BatchGetBuildBatches
* BatchGetBuilds
* BatchGetCommandExecutions
* BatchGetFleets
* BatchGetProjects
* BatchGetReportGroups
* BatchGetReports
* BatchGetSandboxes
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
* ListCommandExecutionsForSandbox
* ListCuratedEnvironmentImages
* ListFleets
* ListProjects
* ListReportGroups
* ListReports
* ListReportsForReportGroup
* ListSandboxes
* ListSandboxesForProject
* ListSharedProjects
* ListSharedReportGroups
* ListSourceCredentials
* PutResourcePolicy
* RetryBuild
* StartBuild
* StartBuildBatch
* StartCommandExecution
* StartSandbox
* StartSandboxConnection
* StopBuild
* StopBuildBatch
* StopSandbox
* UpdateFleet
* UpdateProject
* UpdateProjectVisibility
* UpdateReportGroup
