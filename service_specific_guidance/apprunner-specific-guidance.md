# Service-specific guidance: AWS App Runner


This document outlines service-specific guidance for implementing a data perimeter for AWS App Runner.

AWS App Runner is an AWS service that provides a fast, simple, and cost-effective way to deploy from source code or a container image directly to a scalable and secure web application in the AWS Cloud. App Runner connects directly to your code or image repository and provides an automatic integration and delivery pipeline with fully managed operations, high performance, scalability, and security.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | N |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | Y |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | N |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | Y |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | Y |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | Y |

*Y - Additional considerations apply. N - No additional considerations apply.

## CreateService
### Code execution within a VPC

**Perimeter type applicability**: identity and resource perimeter on network, network perimeter.

**Description**: [CreateService](https://docs.aws.amazon.com/apprunner/latest/api/API_CreateService.html) allows you to enable VPC access for outgoing traffic with a custom VPC Connector.

**Additional controls**:

If you want to achieve data perimeter control objectives, consider implementing these additional controls:
* Preventative control example: Consider implementing `apprunner:VpcConnectorArn` in an SCP to help restrict creation of resources to a customer managed VPC only. See [restrict_nonvpc_deployment_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help ensure that developers specify the [NetworkConfiguration](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-apprunner-service-networkconfiguration.html) property of the [AWS::AppRunner::Service](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-apprunner-service.html) resource.
* Detective control example: Consider implementing the AWS Config rule, [apprunner-service-in-vpc](https://docs.aws.amazon.com/config/latest/developerguide/apprunner-service-in-vpc.html), to help detect misconfigurations or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateService](https://docs.aws.amazon.com/apprunner/latest/api/API_CreateService.html) API calls in your environment (specifically, the [NetworkConfiguration](https://docs.aws.amazon.com/apprunner/latest/api/API_NetworkConfiguration.html) request parameter). If necessary, remediate with the responsive controls of your choice.

### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [CreateService](https://docs.aws.amazon.com/apprunner/latest/api/API_CreateService.html) allows you to specify a service role, referred to as the instance role, for your compute instance operations. The service uses the role to make AWS API calls from your code.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json) or [network_perimeter_vpceorgid_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_vpceorgid_scp.json). These policies demonstrate how to use the `dp:include:network` tag set to `true` to restrict calls made by the instance role. If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

## UpdateService
### Code execution within a VPC

**Perimeter type applicability**: identity and resource perimeter on network, network perimeter.

**Description**: [UpdateService](https://docs.aws.amazon.com/apprunner/latest/api/API_UpdateService.html) allows you to enable VPC access for outgoing traffic with a custom VPC Connector.

**Additional controls**:

If you want to achieve data perimeter control objectives, consider implementing these additional controls:
* Preventative control example: Consider implementing `apprunner:VpcConnectorArn` in an SCP to help restrict creation of resources to a customer managed VPC only. See [restrict_nonvpc_deployment_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help ensure that developers specify the [NetworkConfiguration](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-apprunner-service-networkconfiguration.html) property of the [AWS::AppRunner::Service](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-apprunner-service.html) resource.
* Detective control example: Consider implementing the AWS Config rule, [apprunner-service-in-vpc](https://docs.aws.amazon.com/config/latest/developerguide/apprunner-service-in-vpc.html), to help detect misconfigurations or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateService](https://docs.aws.amazon.com/apprunner/latest/api/API_UpdateService.html) API calls in your environment (specifically, the [NetworkConfiguration](https://docs.aws.amazon.com/apprunner/latest/api/API_NetworkConfiguration.html) request parameter). If necessary, remediate with the responsive controls of your choice.

### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [UpdateService](https://docs.aws.amazon.com/apprunner/latest/api/API_UpdateService.html) allows you to specify a service role, referred to as the instance role, for your compute instance operations. The service uses the role to make AWS API calls from your code.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json) or [network_perimeter_vpceorgid_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_vpceorgid_scp.json). These policies demonstrate how to use the `dp:include:network` tag set to `true` to restrict calls made by the instance role. If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

## List of service APIs reviewed against data perimeter control objectives

* AssociateCustomDomain
* CreateAutoScalingConfiguration
* CreateConnection
* CreateObservabilityConfiguration
* CreateService
* CreateVpcConnector
* CreateVpcIngressConnection
* DeleteAutoScalingConfiguration
* DeleteConnection
* DeleteObservabilityConfiguration
* DeleteService
* DeleteVpcConnector
* DeleteVpcIngressConnection
* DescribeAutoScalingConfiguration
* DescribeCustomDomains
* DescribeObservabilityConfiguration
* DescribeService
* DescribeVpcConnector
* DescribeVpcIngressConnection
* DisassociateCustomDomain
* ListAutoScalingConfigurations
* ListConnections
* ListObservabilityConfigurations
* ListOperations
* ListServices
* ListServicesForAutoScalingConfiguration
* ListTagsForResource
* ListVpcConnectors
* ListVpcIngressConnections
* PauseService
* ResumeService
* StartDeployment
* TagResource
* UntagResource
* UpdateDefaultAutoScalingConfiguration
* UpdateService
* UpdateVpcIngressConnection
