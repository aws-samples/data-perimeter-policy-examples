# Examples of service-specific controls

Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved. SPDX-License-Identifier: CC-BY-SA-4.0

## Description

This folder contains examples of resource-based policies and resource control policies (RCPs) that enforce identity and network perimeter controls for specific AWS services.

### Resource-based policy examples:

* [api_gateway_policy](api_gateway_policy.json) - Enforces identity and network perimeter controls on [Amazon API Gateway](https://aws.amazon.com/api-gateway/) resources.
* [sns_topic_policy](sns_topic_policy.json) - Enforces identity and network perimeter controls on [Amazon Simple Notification Service (Amazon SNS)](https://aws.amazon.com/sns/) resources.


Because developers will be creating resources such as Amazon SNS topics on a regular basis, you might need to implement automation to enforce identity and network perimeter controls when those resources are created or their policies are changed. One option is to use custom [AWS Config rules](https://docs.aws.amazon.com/config/latest/developerguide/evaluate-config_develop-rules.html)_._ Alternatively, you can choose to enforce resource deployment through [AWS Service Catalog](https://aws.amazon.com/servicecatalog/?aws-service-catalog.sort-by=item.additionalFields.createdDate&aws-service-catalog.sort-order=desc) or a CI/CD pipeline. With the AWS Service Catalog approach, you can have identity and network perimeter controls built into the centrally controlled products that are made available to developers to deploy within their accounts. With the CI/CD pipeline approach, the pipeline can have built-in compliance checks that enforce identity and network perimeter controls during the deployment. If you are deploying resources with your CI/CD pipeline by using [AWS CloudFormation](https://aws.amazon.com/cloudformation/), see the blog post [Proactively keep resources secure and compliant with AWS CloudFormation Hooks](https://aws.amazon.com/blogs/mt/proactively-keep-resources-secure-and-compliant-with-aws-cloudformation-hooks/).

Note that the policy examples in this folder do not represent a complete list of valid data access patterns, and they are intended for you to tailor and extend them to suit the needs of your environment. Additionally, these examples do not grant any permissions; they only restrict access by explicitly denying specific data access patterns. You still have to grant appropriate permissions with explicit `Allow` statements in identity-based or resource-based policies.  

### Resource control policy example:

* [signin_console_policy](./signin_console_policy.json) - Enforces network perimeter controls on [AWS Management Console](https://aws.amazon.com/console/) sign-in. Restricts console access to requests originating from an expected network, and exempts designated excluded principals for break-glass access.

## Included data access patterns

### "Sid":"EnforceNetworkPerimeterSourceVPC"

This policy statement is included in the [signin_console_policy](signin_console_policy.json) and applies across all three phases of AWS Management Console sign-in:

* `signin:Authenticate` - authenticates the user to the AWS Management Console. This corresponds to the pre-authentication phase of sign-in.
* `signin:AuthorizeOAuth2Access` - authenticates through a browser to obtain an OAuth 2.0 authorization code for credential exchange.
* `signin:CreateOAuth2Token` - exchanges that authorization code for the OAuth 2.0 access and refresh tokens used to access AWS services from developer tools and applications.

It limits console access to expected networks. The exemption for excluded principals lists both the `signin:PrincipalArn` and `aws:PrincipalArn` condition keys because `signin:PrincipalArn` is only populated during the pre-authentication phase and `aws:PrincipalArn` is only populated during the post-authentication phase. Expected networks are defined as follows:

* Your organization's on-premises data centers and static egress points in AWS such as a NAT gateway that are specified by IP ranges (`<my-corporate-cidr>`) in the policy statement.
* Your organization's VPC specified by the VPC ID (`<my-vpc>`) in the policy statement.
* Excluded principals specified by their ARNs (`<excluded-principal-arn-a>` and `<excluded-principal-arn-b>`) in the policy statement are exempted from network restrictions to preserve emergency recovery access.

### "Sid":"SourceVPCRegion"

This policy statement is included in the [signin_console_policy](signin_console_policy.json) and applies across all three sign-in actions. AWS VPC IDs are unique within an AWS Region, and the same VPC ID can exist in different AWS Regions. [aws:RequestedRegion](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-requestedregion) is used together with [aws:SourceVpc](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourcevpc) to ensure that VPC-sourced console sign-in requests originate from the expected AWS Region. Replace `<my-vpc-in-this-region>` with the VPC ID of your corporate VPC and `<my-vpc-region>` with the AWS Region in which that VPC resides.