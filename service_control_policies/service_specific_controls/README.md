# Examples of service-specific controls

Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved. SPDX-License-Identifier: CC-BY-SA-4.0

## Description

This folder contains examples of SCPs with service-specific controls you might want to enforce when implementing a data perimeter for a service. These examples do not represent a complete list of valid data access patterns, and they are intended for you to tailor and extend them to suit the needs of your environment. 

Use the following example SCPs individually or in combination:

* [network_perimeter_ec2_scp](network_perimeter_ec2_scp.json) – Enforces network perimeter controls on service roles used by Amazon EC2 instances.
* [network_perimeter_iam_users_scp](network_perimeter_iam_users_scp.json) - Enforces network perimeter controls on IAM users with long-term access keys.
* [network_perimeter_lambda_scp](network_perimeter_lambda_scp.json) - Enforces network perimeter controls on service roles used by AWS Lambda.
* [network_perimeter_glue_scp](network_perimeter_glue_scp.json) - Enforces network perimeter controls on service roles used by AWS Glue jobs.
* [restrict_nonvpc_deployment_scp](restrict_nonvpc_deployment_scp.json) - Enforces deployment of resources in a customer managed Amazon VPC.
* [restrict_idp_configurations_scp](restrict_idp_configurations_scp.json) - Restricts the ability to make configuration changes to the IAM SAML identity providers.
* [restrict_untrusted_endpoints_scp](restrict_untrusted_endpoints_scp.json) - Prevent untrusted non-AWS resources from being configured as targets for service operations.
* [restrict_presignedURL_scp](restrict_presignedURL_scp.json) - Restricts actions that create Amazon S3 presigned URLs that are presigned by a service principal.
* [restrict_resource_policy_configurations_scp](restrict_resource_policy_configurations_scp.json) - Restricts the ability to configure resource-based policies.

Note that the SCP examples in this repository use a [deny list strategy](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps_strategies.html), which means that you also need a FullAWSAccess policy or other policy attached to your AWS Organizations organization entities to allow actions. You still also need to grant appropriate permissions to your principals by using identity-based or resource-based policies.

See [Tagging conventions](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main?tab=readme-ov-file#tagging-conventions-and-governance) used in the policy examples to control the scope of data perimeter guardrails.

#### Network perimeter controls

Network perimeter policy examples in this folder enforce the controls on specific service roles and IAM users. 

* Some AWS services use [service roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-role)  to perform tasks on your behalf. Some service roles are designed to be used by a service to directly call other services on your behalf as well as to make API calls from your code (for example, an [AWS Lambda](https://aws.amazon.com/lambda/) function role is used to publish logs to [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/) and to make calls to AWS APIs from the Lambda function code). Because these services allow code execution, it is possible for a user to obtain the credentials associated with a service role. Therefore, you may want to enforce the use of such credentials from expected networks only. 
* You may also want to restrict the use of [IAM users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users.html) to expected networks only. We recommend using [IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) instead of IAM users with long-term access keys, as these access keys remain valid until manually revoked and therefore present a higher security risk. If your organization continues to use IAM users, implementing network perimeter controls can help mitigate potential security risks. 
* This folder has examples of policies for enforcing controls on specific service roles and IAM principals tagged with the `dp:include:network` tag set to `true` and service-specific condition keys.

For description of actions listed in the `NotAction` element in the example policies, see [Services and actions that require an exception to the network perimeter](../#services-and-actions-that-require-an-exception-to-the-network-perimeter).

## Included data access patterns

The following policy statements are included in the SCP examples, each statement representing specific data access patterns.

### "Sid":"EnforceNetworkPerimeterOnEC2Roles"

This policy statement is included in the [network_perimeter_ec2_scp](network_perimeter_ec2_scp.json) and limits access to expected networks for service roles used by Amazon EC2 instance profiles. Expected networks are defined as follows:
* Your on-premises data centers and static egress points in AWS such as a NAT gateway that are specified by IP ranges (`<my-corporate-cidr>`) in the policy statement. 
* Your organization’s VPCs that are specified by VPC IDs (`<my-vpc>`) in the policy statement.  
* Networks of AWS services that use [forward access sessions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_forward_access_sessions.html) to access resources on your behalf as denoted by `aws:ViaAWSService` in the policy statement. This access pattern applies when you access data via an AWS service, and that service takes subsequent actions on your behalf by using your IAM credentials. 
* Networks of AWS services when AWS services interact with [KMS](https://aws.amazon.com/kms/) encrypted AMIs, volumes, or snapshots as denoted by the `aws:PrincipalArn` condition key with a value of `arn:aws:iam:::role/aws:ec2-infrastructure`. 

The `ec2:SourceInstanceARN` condition key is used to target role sessions that are created for applications running on your Amazon EC2 instances. 

 Additional considerations:
  
* We recommend that you use [aws:SourceVpc](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourcevpc) with [aws:RequestedRegion](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-requestedregion) as AWS VPC IDs are unique within a AWS Region, and same VPC ID can exist in different AWS Regions. Replace `<my-vpc-in-this-region>` with the VPC ID specified under `"Sid":"EnforceNetworkPerimeterOnEC2Roles"` and `<my-vpc-region>` with the VPC region.

### "Sid":"EnforceNetworkPerimeterOnIAMUsers"

This policy statement is included in the [network_perimeter_iam_users_scp](/service_control_policies/service_specific_controls/network_perimeter_iam_users_scp.json) and limits access to expected networks for IAM users. Expected networks are defined as follows:
*	Your on-premises data centers and static egress points in AWS such as a NAT gateway that are specified by IP ranges (`<my-corporate-cidr>`) in the policy statement.
*	Your organization’s VPCs that are specified by VPC IDs (`<my-vpc>`) in the policy statement.
*	Networks of AWS services that use [forward access sessions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_forward_access_sessions.html) to access resources on your behalf as denoted by `aws:ViaAWSService` in the policy statement. This access pattern applies when you access data via an AWS service, and that service takes subsequent actions on your behalf by using your IAM credentials.

 Additional considerations:
  
* We recommend that you use [aws:SourceVpc](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourcevpc) with [aws:RequestedRegion](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-requestedregion) as AWS VPC IDs are unique within a AWS Region, and same VPC ID can exist in different AWS Regions. Replace `<my-vpc-in-this-region>` with the VPC ID specified under `"Sid":"EnforceNetworkPerimeterOnIAMUsers"` and `<my-vpc-region>` with the VPC region.


### "Sid":"EnforceNetworkPerimeterOnLambdaRoles"

This policy statement is included in the [network_perimeter_lambda_scp](/service_control_policies/service_specific_controls/network_perimeter_lambda_scp.json) and limits access to expected networks for service roles used by AWS Lambda. Expected networks are defined as follows:
*   Your on-premises data centers and static egress points in AWS such as a NAT gateway that are specified by IP ranges (`<my-corporate-cidr>`) in the policy statement.
*   Your organization’s VPCs that are specified by the organization ID (`<my-org-id>`) in the policy statement.
*   Networks of AWS services that use [forward access sessions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_forward_access_sessions.html) to access resources on your behalf as denoted by `aws:ViaAWSService` in the policy statement. This access pattern applies when you access data via an AWS service, and that service takes subsequent actions on your behalf by using your IAM credentials.
*   AWS Lambda networks when the service interacts with [CloudWatch Logs]( https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html), [AWS X-Ray](https://aws.amazon.com/xray/), and [Amazon EFS]( https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html), as denoted by the `NotAction` element with the actions `xray:PutTraceSegments`,`logs:CreateLogGroup`,`logs:CreateLogStream`,`logs:PutLogEvents`, `elasticfilesystem:ClientMount`.

The [` lambda:SourceFunctionArn `](https://docs.aws.amazon.com/lambda/latest/dg/permissions-source-function-arn.html) condition key is used to target role sessions that are created for your function's execution environment.

### "Sid":"EnforceNetworkPerimeterOnGlueRoles"

This policy statement is included in the [network_perimeter_glue_scp](/service_control_policies/service_specific_controls/network_perimeter_glue_scp.json) and limits access to expected networks for service roles used by AWS Glue jobs. Expected networks are defined as follows:
*   Your on-premises data centers and static egress points in AWS such as a NAT gateway that are specified by IP ranges (`<my-corporate-cidr>`) in the policy statement.
*   Your organization’s VPCs that are specified by VPC IDs (`<my-vpc>`) in the policy statement.
*   Networks of AWS services that use [forward access sessions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_forward_access_sessions.html) to access resources on your behalf as denoted by `aws:ViaAWSService` in the policy statement. This access pattern applies when you access data via an AWS service, and that service takes subsequent actions on your behalf by using your IAM credentials.
*   AWS Glue networks when the service interacts with [CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html), as denoted by the `NotAction` element with the actions `logs:CreateLogGroup`,`logs:CreateLogStream`,`logs:PutLogEvents`.

The [`glue:CredentialIssuingService`](https://docs.aws.amazon.com/glue/latest/dg/security_iam_id-based-policy-examples.html#glue-identity-based-policy-context-key-glue) condition key is used to target role sessions that are created for your job's execution environment.

 Additional considerations:
  
* We recommend that you use [aws:SourceVpc](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourcevpc) with [aws:RequestedRegion](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-requestedregion) as AWS VPC IDs are unique within a AWS Region, and same VPC ID can exist in different AWS Regions. Replace `<my-vpc-in-this-region>` with the VPC ID specified under `"Sid":"EnforceNetworkPerimeterOnGlueRoles"` and `<my-vpc-region>` with the VPC region.

### "Sid":"PreventIdPTrustModifications"

This statement is included in the [restrict_idp_configurations_scp](restrict_idp_configurations_scp.json) and prevents users from making configuration changes to the IAM SAML [identity providers](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_saml.html), IAM OIDC [identity providers](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_oidc.html), and [AWS IAM Roles Anywhere](https://aws.amazon.com/iam/roles-anywhere/) [trust anchors](https://docs.aws.amazon.com/rolesanywhere/latest/userguide/getting-started.html). It also prevents creation of an [account instance of IAM Identity Center]( https://docs.aws.amazon.com/singlesignon/latest/userguide/account-instances-identity-center.html).  

###  "Sid":"PreventDeploymentCodeStarConnections", "Sid": "PreventDeploymentSSMAutomationRunbook"

These statements are included in the [restrict_nonvpc_deployment_scp](restrict_nonvpc_deployment_scp.json) and limit the use of [AWS CodeStar Connections](https://docs.aws.amazon.com/codestar-connections/latest/APIReference/Welcome.html) and [AWS Systems Manager Automation](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-automation.html).

AWS services such as AWS Systems Manager Automation do not support deployment within a VPC and provide direct access to the internet that is not controlled by your VPC. You can block the use of such services by using SCPs or implementing your own proxy solution to inspect egress traffic.

### "Sid":"PreventNonVPCDeploymentSageMaker", "Sid":"PreventNonVPCDeploymentGlueJob", "Sid":"PreventNonVPCDeploymentCloudShell", "Sid":"PreventNonVPCDeploymentLambda", "Sid":"PreventNonVPCDeploymentAppRunner", and "Sid":"PreventNonVPCDeploymentCodeBuild"

These statements are included in the [restrict_nonvpc_deployment_scp](restrict_nonvpc_deployment_scp.json) and explicitly deny relevant [Amazon SageMaker](https://aws.amazon.com/sagemaker/), [AWS Glue](https://aws.amazon.com/glue/), [AWS CloudShell](https://aws.amazon.com/cloudshell/), [AWS Lambda](https://aws.amazon.com/lambda/), [AWS AppRunner](https://aws.amazon.com/apprunner/), and [AWS CodeBuild](https://aws.amazon.com/codebuild/) operations unless they have VPC configurations specified in the requests. Use these statements to enforce deployment in a VPC for these services.

Services such as Lambda, AWS Glue, CloudShell, App Runner, and SageMaker support different deployment models. For example, [Amazon SageMaker Studio](https://aws.amazon.com/pm/sagemaker/) and [SageMaker notebook instances](https://docs.aws.amazon.com/sagemaker/latest/dg/nbi.html) allow direct internet access by default. However, they provide you with the capability to configure them to run within your VPC so that you can inspect requests by using VPC endpoint policies (against identity and resource perimeter controls) and enforce the network perimeter.


### "Sid": "PreventNonVpcOnlySageMakerDomain"

This statement is included in the [restrict_nonvpc_deployment_scp](restrict_nonvpc_deployment_scp.json) and prevents users from creating [Amazon SageMaker domains](https://docs.aws.amazon.com/sagemaker/latest/dg/sm-domain.html) that can access the Internet through a VPC managed by SageMaker, or updating SageMaker domains to allow access to the Internet through a VPC managed by SageMaker.    
For more details, see the definition of the parameter [`AppNetworkAccessType`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateDomain.html#sagemaker-UpdateDomain-request-AppNetworkAccessType) in the Amazon SageMaker API Reference.


### "Sid": "PreventDirectInternetAccessSageMakerNotebook"

This statement is included in the [restrict_nonvpc_deployment_scp](restrict_nonvpc_deployment_scp.json) and prevents users from creating [Amazon SageMaker Notebooks Instances](https://docs.aws.amazon.com/sagemaker/latest/dg/nbi.html) that can access the Internet through a VPC managed by SageMaker.        
For more details, see [Connect a Notebook Instance in a VPC to External Resources](https://docs.aws.amazon.com/sagemaker/latest/dg/appendix-notebook-and-internet-access.html) in the Amazon SageMaker documentation.

### "Sid": "PreventUntrustedSNSEmailSubscriptions"

This statement is included in the [restrict_untrusted_endpoints_scp](restrict_untrusted_endpoints_scp.json) and prevents users from subscribing email addresses that belong to domains other than the one denoted by `<trusted_email_domain>` to an SNS topic. See [Amazon SNS policy keys](https://docs.aws.amazon.com/sns/latest/dg/sns-using-identity-based-policies.html#sns-policy-keys) for more details.

### "Sid": "PreventEventBridgeAPIDestinations"

This statement is included in the [restrict_untrusted_endpoints_scp](restrict_untrusted_endpoints_scp.json) and prevents users from using API destinations as the target for an event bus rule. See [Using IAM policy conditions in Amazon EventBridge](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-use-conditions.html#limiting-access-to-targets) for more details.

### "Sid": "PreventUntrustedStepFunctionsHTTPSAPI"

This statement is included in the [restrict_untrusted_endpoints_scp](restrict_untrusted_endpoints_scp.json) and prevents Step Functions from invoking HTTPS APIs that don't belong to your organization. See [IAM permissions to run an HTTP Task](https://docs.aws.amazon.com/step-functions/latest/dg/call-https-apis.html#connect-http-task-permissions) for more details.

### "Sid": "PreventUntrustedSESv1Emails"
This statement is included in the [restrict_untrusted_endpoints_scp](restrict_untrusted_endpoints_scp.json) to restrict Simple Email Service from sending emails to addresses external to your organization. See [Conditions specific to sending authorization](https://docs.aws.amazon.com/ses/latest/dg/sending-authorization-policy-examples.html#sending-authorization-policy-conditions) for more details.

### "Sid": "PreventUntrustedSESv2Emails"
This statement is included in the [restrict_untrusted_endpoints_scp](restrict_untrusted_endpoints_scp.json) to restrict Simple Email Service API V2 (sesv2) from sending emails to addresses external to your organization. See [Conditions specific to sending authorization](https://docs.aws.amazon.com/ses/latest/dg/sending-authorization-policy-examples.html#sending-authorization-policy-conditions) for more details.

### "Sid": "PreventUntrustedSESVerificationEmails"
This statement is included in the [restrict_untrusted_endpoints_scp](restrict_untrusted_endpoints_scp.json) to restrict access to ses:SendCustomVerificationEmail using aws:PrincipalTag condition key as ses:Recipients condition doesnt apply to ses:SendCustomVerificationEmail

### "Sid": "PreventCreationOfServicePresignedURL"

This statement is included in the [restrict_presignedURL_scp](restrict_presignedURL_scp.json) and prevents users from making API requests that return Amazon S3 presigned URLs that are presigned by a service principal.

### "Sid": "PreventResourcePolicyConfigurations"

This statement is included in the [restrict_resource_policy_configurations_scp](restrict_resource_policy_configurations_scp.json) and prevents users from configuring resource-based policies for services that are not yet supported by RCPs. See the AWS Organizations User Guide for the [List of AWS services that support RCPs](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_rcps.html#rcp-supported-services).