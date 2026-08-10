# Service-specific guidance: Amazon EMR Serverless


This document outlines service-specific guidance for implementing a data perimeter for Amazon EMR Serverless.

Amazon EMR Serverless is a deployment option for Amazon EMR that provides a serverless runtime environment. It simplifies the operation of analytics applications that use the latest open-source frameworks, such as Apache Spark and Apache Hive, without requiring customers to configure, optimize, secure, or operate clusters.

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

## CreateApplication
### Code execution within a VPC

**Perimeter type applicability**: identity and resource perimeter on network, network perimeter.

**Description**: [CreateApplication](https://docs.aws.amazon.com/emr-serverless/latest/APIReference/API_CreateApplication.html) allows you to associate applications with an Amazon VPC to run your code.

**Additional controls**:

If you want to achieve data perimeter control objectives, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help ensure that developers specify the [NetworkConfiguration](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-emrserverless-application.html#cfn-emrserverless-application-networkconfiguration) property of the [AWS::EMRServerless::Application](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-emrserverless-application.html) resource.
* Detective control example: Consider implementing a custom AWS Config rule to help detect EMR Serverless applications not associated with a VPC or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateApplication](https://docs.aws.amazon.com/emr-serverless/latest/APIReference/API_CreateApplication.html) API calls in your environment (specifically, the [networkConfiguration](https://docs.aws.amazon.com/emr-serverless/latest/APIReference/API_NetworkConfiguration.html) request parameter). If necessary, remediate with the responsive controls of your choice.

## StartJobRun
### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [StartJobRun](https://docs.aws.amazon.com/emr-serverless/latest/APIReference/API_StartJobRun.html) allows you to specify a service role, referred to as the job runtime role, for your job run operations. The service uses the role to make AWS API calls from your code.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json) or [network_perimeter_vpceorgid_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_vpceorgid_scp.json). These policies demonstrate how to use the `dp:include:network` tag set to `true` to restrict calls made by the job runtime role. If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

## StartSession
### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [StartSession](https://docs.aws.amazon.com/emr-serverless/latest/APIReference/API_StartSession.html) allows you to specify a service role for your interactive session operations. The service uses the role to make AWS API calls from your code.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json) or [network_perimeter_vpceorgid_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_vpceorgid_scp.json). These policies demonstrate how to use the `dp:include:network` tag set to `true` to restrict calls made by the session's execution role. If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

## UpdateApplication
### Code execution within a VPC

**Perimeter type applicability**: identity and resource perimeter on network, network perimeter.

**Description**: [UpdateApplication](https://docs.aws.amazon.com/emr-serverless/latest/APIReference/API_UpdateApplication.html) allows you to associate applications with an Amazon VPC to run your code.

**Additional controls**:

If you want to achieve data perimeter control objectives, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help ensure that developers specify the [NetworkConfiguration](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-emrserverless-application.html#cfn-emrserverless-application-networkconfiguration) property of the [AWS::EMRServerless::Application](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-emrserverless-application.html) resource.
* Detective control example: Consider implementing a custom AWS Config rule to help detect EMR Serverless applications not associated with a VPC or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateApplication](https://docs.aws.amazon.com/emr-serverless/latest/APIReference/API_UpdateApplication.html) API calls in your environment (specifically, the [networkConfiguration](https://docs.aws.amazon.com/emr-serverless/latest/APIReference/API_NetworkConfiguration.html) request parameter). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

* CancelJobRun
* CreateApplication
* DeleteApplication
* GetApplication
* GetJobRun
* ListApplications
* ListJobRunAttempts
* ListJobRuns
* ListTagsForResource
* StartApplication
* StartJobRun
* StartSession
* StopApplication
* TagResource
* UntagResource
* UpdateApplication
