# Data Perimeter Policy Examples

Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved. SPDX-License-Identifier: CC-BY-SA-4.0

## Table of Contents

* [DISCLAIMER](#disclaimer)
* [Introduction](#introduction)
* [Getting started](#getting-started)
* [Policy types](#policy-types)
* [Tagging conventions](#tagging-conventions)
* [Implementation](#implementation)
* [License summary](#license-summary)

## DISCLAIMER

The sample code; software libraries; command line tools; proofs of concept; templates; or other related technology (including any of the foregoing that are provided by our personnel) is provided to you as AWS Content under the AWS Customer Agreement, or the relevant written agreement between you and AWS (whichever applies). You should not use this AWS Content in your production accounts, or on production or other critical data. You are responsible for testing, securing, and optimizing the AWS Content, such as sample code, as appropriate for production grade use based on your specific quality control practices and standards.

## Introduction

This repository contains example policies to help you implement a [data perimeter on AWS](https://aws.amazon.com/identity/data-perimeters-on-aws/). The policy examples in this repository cover some common patterns and are for reference purposes only. Tailor and extend these examples to suit the needs of your environment.

## Getting started

A data perimeter is a set of preventive controls to help ensure that only your trusted identities are accessing trusted resources from expected networks. To get started with data perimeters on AWS, review the following resources:

* [Data perimeters on AWS](https://aws.amazon.com/identity/data-perimeters-on-aws/)
* [Blog Post Series: Establishing a Data Perimeter on AWS](https://aws.amazon.com/identity/data-perimeters-blog-post-series/)

## Policy types

You implement data perimeters primarily by using three different policy types: [service control policies (SCPs)](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html), [resource control policies (RCPs)](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_rcps.html), and [VPC endpoint policies](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints-access.html#vpc-endpoint-policies). This repo provides examples of these policy types. The following table illustrates the relationship between data perimeter objectives and policy types used to achieve them.

|Data perimeter	| Control objective| Policy type | Primary IAM capability | Policy examples |
|--- |---	|---	|---	|---	|
|Identity perimeter  |Only trusted identities can access my resources |RCP |aws:PrincipalOrgID, aws:PrincipalIsAWSService, aws:SourceOrgID|[resource_control_policies](resource_control_policies)|
|Identity perimeter  | Only trusted identities are allowed from my network |VPC endpoint policy |aws:PrincipalOrgID, aws:PrincipalIsAWSService|[vpc_endpoint_policies](vpc_endpoint_policies)|
|Resource perimeter |My identities can access only trusted resources |SCP |aws:ResourceOrgID|[service_control_policies](service_control_policies)|
|Resource perimeter |Only trusted resources can be accessed from my network |VPC endpoint policy |aws:ResourceOrgID|[vpc_endpoint_policies](vpc_endpoint_policies)|
|Network perimeter |My identities can access resources only from expected networks |SCP |aws:SourceIp, aws:SourceVpc/aws:SourceVpce, aws:ViaAWSService|[service_control_policies](service_control_policies)|
|Network perimeter |My resources can only be accessed from expected networks |RCP |aws:SourceIp, aws:SourceVpc/aws:SourceVpce, aws:ViaAWSService, aws:PrincipalIsAWSService|[resource_control_policies](resource_control_policies)|


Policy examples in this repository include various data access patterns you might need to account for when implementing a data perimeter on AWS. The README.md in the folder for each policy type contains information about the included access patterns.

## Tagging conventions

Policy examples in this repository use the `aws:PrincipalTag/tag-key` and `aws:ResourceTag/tag-key` global condition keys to control the scope of data perimeter guardrails with the following tagging conventions. You should follow your existing tagging strategy or [AWS tagging best practices](https://docs.aws.amazon.com/whitepapers/latest/tagging-best-practices/tagging-best-practices.html) when implementing in your environment.

1. Tag [AWS Identity and Access Management](https://aws.amazon.com/iam/) (IAM) principals and resources in your accounts that you would like to target with network perimeter controls with the `dp:include:network` tag key and the value set to `true`. You may want to start enforcing network perimeter controls on IAM principals used by human users to access AWS services interactively in the AWS Management Console, or programmatically with the AWS CLI, AWS Tools for PowerShell, or API.
2. Tag IAM principals and resources in your accounts that should be excluded from the network perimeter with the `dp:exclude:network` tag key and the value set to `true`. This tag key can be used for human users and applications that should be able to use AWS services from outside of your expected network, or for resources that should not have the network perimeter applied.
3. Tag IAM principals and resources in your accounts that should be excluded from the identity perimeter with the `dp:exclude:identity` tag key and the value set to `true`. This tag key is designed for human users and applications that should be able to use AWS services without being restricted by identity perimeter controls. This tag can also be used on resources that should not have the identity perimeter applied, such as those with a business reason to be accessible by a large number of external identities (public resources).
4. Tag IAM principals in your accounts that should be excluded from the resource perimeter with the `dp:exclude:resource` tag key and the value set to `true`. This tag key is designed for human users and applications that should be able to access resources that do not belong to your organization.
5. Tag IAM principals in your accounts that should be excluded from the resource perimeter with the `dp:exclude:resource:<service>` tag key and the value set to `true`. This tag key is designed for human users and applications that should be able to access service-specific resources that do not belong to your organization.
6. Tag IAM principals and resources in your accounts that should be excluded from all data perimeters with the `dp:exclude` tag key and the value set to `true`. This tag key is designed for human users, applications, and resources that should not be restricted by any perimeter control.

Because the preceding tags are used for authorization, the [data_perimeter_governance_scp](service_control_policies/data_perimeter_governance_scp.json) and [data_perimeter_governance_rcp](resource_control_policies/data_perimeter_governance_rcp.json) policy examples include statements to protect these tags from unauthorized changes. In the `data_perimeter_governance_scp` example, only principals in your organization with the `team` tag and the value set to `admin` will be able to apply and modify these tags. `data_perimeter_governance_rcp` demonstrates how to protect session tags with an exception for tags that are set by your trusted SAML identity provider(s). You can modify the example policies based on the tagging strategy and governance adopted in your organization.


Note that if you are using [AWS Control Tower](https://aws.amazon.com/controltower/) to centrally manage and govern your accounts, you might also need to exclude [AWSControlTowerExecution and other roles](https://docs.aws.amazon.com/controltower/latest/userguide/roles-how.html) that the service uses to manage accounts on your behalf.

## Implementation

To effectively use the example policies in this repository, follow these steps:

1. Determine which policy and perimeter types to implement based on the control objectives they help achieve and your security requirements.
2. Replace the placeholder values in the example policies based on your definition of trusted identities, trusted resources, and expected networks:

    * Replace `<my-org-id>` with your [AWS Organizations](https://aws.amazon.com/organizations/) organization ID.
    * Replace `<region>` with the AWS Region to which you are deploying the policy.
    * Replace `<my-corporate-cidr>` with your corporate public IP space.
    * Replace `<my-vpc>` with a list of VPC IDs that constitute your network perimeter for a resource or an AWS Organizations entity to which you are applying a policy.
    * Replace `<load-balancing-account-id>` with the ID of the AWS account that belongs to the Elastic Load Balancing services (based on the Region for your load balancer) if access logging is in use. See [Enable access logs for your Classic Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/enable-access-logs.html#attach-bucket-policy) and [Access logs for your Application Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-access-logs.html#access-logging-bucket-permissions) for a complete list of account IDs. 
    * Replace `<ecr-account-id>` with the ID of the AWS account that owns [Amazon Elastic Container Registry (Amazon ECR)](https://aws.amazon.com/ecr/) repositories that you require to be used in your environment. See ["Sid":"EnforceResourcePerimeterAWSResourcesECR"](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies#sidenforceresourceperimeterawsresourcesecr) and ["Sid": "AllowRequestsByOrgsIdentitiesToAWSResources"](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/vpc_endpoint_policies#sid-allowrequestsbyorgsidentitiestoawsresources) for more details.
    * Replace `<lambdalayer-account-id>` with the ID of the AWS account that owns [AWS Lambda](https://aws.amazon.com/lambda/) layers that you require to be used within your environment. See ["Sid":"EnforceResourcePerimeterAWSResourcesLambdaLayer"](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies#sidenforceresourceperimeterawsresourceslambdalayer) for more details.
    * Replace the following values to support third party integrations where access to third party resources or by third party identities is required:
        * Replace `<third-party-account-a>` and `<third-party-account-b>` with account IDs of third parties.
        * Replace `<action>` with specific actions required for third party integrations.
        * Replace `<third-party-resource-arn>` with the Amazon Resource Name (ARN) of the resource owned by a third party.  
        * If you do not have third party integrations that require access to your resources or networks:
            * Remove `<third-party-account-a>` and `<third-party-account-b>` from the `aws:PrincipalAccount` condition key in the resource control policy (RCP) examples.
            * Remove `"Sid": “AllowRequestsByOrgsIdentitiesToThirdPartyResources"` and `"Sid": "AllowRequestsByThirdPartyIdentitiesToThirdPartyResources"` statements from the VPC endpoint policy examples.
            * Remove the `"Sid":"EnforceResourcePerimeterThirdPartyResources"` statement from the `resource_perimeter_scp` SCP example.
    * Replace `<OIDC_provider_name_1>`, `<OIDC_provider_name_2>`, and `<my-tenant-value>` with the names of your trusted OIDC providers and tenant.        
    * Tag IAM identities and resources in your accounts in accordance with the tagging conventions for applying data perimeter controls (see the [Tagging conventions](#tagging-conventions) earlier in this document).
3. Deploy policies by using the AWS Management Console or AWS CLI. You can also automate the deployment by using your Infrastructure as Code and CI/CD solutions.
    * Implement SCPs and RCPs: 
        * To use the AWS Management Console, [create](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps_create.html) and [attach](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps_attach.html) an SCP and RCP to an account or an organizational unit (OU).
        * To use the AWS CLI, [create](https://docs.aws.amazon.com/cli/latest/reference/organizations/create-policy.html) and [attach](https://docs.aws.amazon.com/cli/latest/reference/organizations/attach-policy.html) a policy to an account or an OU.
    * Implement resource-based policies (only for services not yet supported by RCPs):
        * See [AWS services that work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) for services that support resource-based policies and follow links in the **Resource-based policies** column, or see [AWS Documentation](https://docs.aws.amazon.com/) (select the applicable service) for instructions about how to apply a resource-based policy.
    * Implement VPC endpoint policies:
        * To use the AWS Management Console, [create](https://docs.aws.amazon.com/vpc/latest/privatelink/create-interface-endpoint.html) or [configure](https://docs.aws.amazon.com/vpc/latest/privatelink/interface-endpoints.html) a VPC endpoint.
        * To use the AWS CLI, [create](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-vpc-endpoint.html) or [modify](https://docs.aws.amazon.com/cli/latest/reference/ec2/modify-vpc-endpoint.html) a VPC endpoint. 


## License Summary

The documentation is made available under the Creative Commons Attribution-ShareAlike 4.0 International License. See the LICENSE file.

The sample code within this documentation is made available under the MIT-0 license. See the LICENSE-SAMPLECODE file.
