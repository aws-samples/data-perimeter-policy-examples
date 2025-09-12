
# Service-specific guidance: AWS App Runner


This document outlines service-specific guidance for implementing a data perimeter for AWS App Runner. 

AWS App Runner is a fully managed service that makes it easy to deploy containerized web applications and APIs at scale. It automatically builds and deploys your code, handles load balancing, scaling, and provides a secure HTTPS endpoint, allowing developers to focus on their application code rather than infrastructure management.


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
        
UpdateService allows you to enable VPC access for outgoing traffic with a custom VPC Connector.

If you want to achieve data perimeter control objectives, consider implementing these additional controls:

* **Preventative control example:** Consider implementing `apprunner:VpcConnectorArn` in an SCP to help restrict creation of resources to a customer managed VPC only. See [restrict_nonvpc_deployment_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.
* **Proactive control example:** Consider implementing CloudFormation Hooks to help ensure that developers specify the [NetworkConfiguration](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-apprunner-service.html#cfn-apprunner-service-networkconfiguration) property of the [AWS::AppRunner::Service](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-apprunner-service.html) resource.
* **Detective control example 1:** Consider implementing the AWS Config rule, [apprunner-service-in-vpc](https://docs.aws.amazon.com/config/latest/developerguide/apprunner-service-in-vpc.html), to help detect misconfigurations or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* **Detective control example 2:** Consider using CloudTrail management events to monitor the [UpdateService](https://docs.aws.amazon.com/apprunner/latest/api/API_UpdateService.html) API calls in your environment (specifically, the [NetworkConfiguration](https://docs.aws.amazon.com/apprunner/latest/api/API_UpdateService.html#apprunner-UpdateService-request-NetworkConfiguration) request parameter). If necessary, remediate with the responsive controls of your choice.


**Additional consideration 2**

Perimeter type applicability: all.
        
CreateService allows you to enable VPC access for outgoing traffic with a custom VPC Connector.

If you want to achieve data perimeter control objectives, consider implementing these additional controls:

* **Preventative control example:** Consider implementing `apprunner:VpcConnectorArn` in an SCP to help restrict creation of resources to a customer managed VPC only. See [restrict_nonvpc_deployment_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.
* **Proactive control example:** Consider implementing CloudFormation Hooks to help ensure that developers specify the [NetworkConfiguration](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-apprunner-service.html#cfn-apprunner-service-networkconfiguration) property of the [AWS::AppRunner::Service](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-apprunner-service.html) resource.
* **Detective control example 1:** Consider implementing the AWS Config rule, [apprunner-service-in-vpc](https://docs.aws.amazon.com/config/latest/developerguide/apprunner-service-in-vpc.html), to help detect misconfigurations or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* **Detective control example 2:** Consider using CloudTrail management events to monitor the [CreateService](https://docs.aws.amazon.com/apprunner/latest/api/API_CreateService.html) API calls in your environment (specifically, the [NetworkConfiguration](https://docs.aws.amazon.com/apprunner/latest/api/API_CreateService.html#apprunner-CreateService-request-NetworkConfiguration) request parameter). If necessary, remediate with the responsive controls of your choice.





**List of service APIs reviewed against data perimeter control objectives**

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
