# VPC endpoint policy examples

Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved. SPDX-License-Identifier: CC-BY-SA-4.0

## Table of Contents

* [Introduction](#introduction)
* [Description](#description)
* [Included data access patterns](#included-data-access-patterns)

## Introduction

VPC endpoints allow you to apply identity and resource perimeter controls by using VPC endpoint policies. These controls mitigate the risk of unintended data disclosure via noncorporate credentials (for example, developers bringing their personal credentials into your network and uploading corporate data to their personal accounts), and prevent your principals from accessing data stores that are not approved by your company.

## Description

This folder contains examples of VPC endpoint policies that enforce identity and resource perimeter controls while allowing select AWS services to operate on your behalf. These examples do not represent a complete list of valid data access patterns, and they are intended for you to tailor and extend them to suit the needs of your environment. Not all VPC endpoint policy examples contained in this folder include all data access patterns described in the following section. When crafting a VPC endpoint policy for a service that is not covered in this repository, you can start with the [default_endpoint_policy.json](default_endpoint_policy.json) and include relevant statements based on your requirements.

For all AWS services where we have provided an example VPC endpoint policy such as SSM or EC2, we strongly recommend starting with those policies instead of the default VPC endpoint policy. There are already service-specific exceptions present within them to allow these services to access their required resources over a VPC endpoint.

Note that VPC endpoint policies do not grant any permissions; instead, they establish a boundary that is the maximum access allowed through the endpoint. You still need to grant appropriate access by using identity-based or resource-based policies.

The methodology you use to deploy these policies will depend on the deployment mechanisms you use to create and manage AWS accounts. For example, you might choose to use [AWS Control Tower](https://aws.amazon.com/controltower/) and the [Customizations for AWS Control Tower solution (CfCT)](https://docs.aws.amazon.com/controltower/latest/userguide/customize-landing-zone.html) to govern your AWS environment at scale. You can use CfCT or your custom CI/CD pipeline to deploy VPC endpoints and VPC endpoint policies that include your identity and resource perimeter controls. 

## Included data access patterns

The following policy statements are included in the VPC endpoint policy examples, each statement representing a specific data access pattern.

### "Sid":"AllowRequestsByOrgsIdentitiesToOrgsResources"

This policy statement allows identities from your AWS Organizations organization to send requests through a VPC endpoint to resources that belong to your organization. 

### "Sid":"AllowRequestsByAWSServicePrincipals"

This policy statement allows [AWS service principals](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_principal.html#principal-services) to send requests to service-owned resources on your behalf through a VPC endpoint. The `aws:PrincipalIsAWSService` IAM condition key is used to denote this in the policy. Though AWS services rarely use their service principals to make calls from your VPCs, some services that operate within your network might need this statement to be present in the VPC endpoint policies to ensure normal operations. See the [service_owned_resources](../service_owned_resources.md) for a list of service-owned resources that can be accessed by AWS service principals.

### "Sid":"AllowRequestsToAWSOwnedResources"

This policy statement allows access to specific service-owned resources through a VPC endpoint. You can list ARNs of service-owned resources in the `Resource` element of the statement. You can further restrict access by specifying allowed actions in the `Action` element of the statement. See the [service_owned_resources](../service_owned_resources.md) for a list of service-owned resources.

### "Sid":"AllowRequestsByOrgsIdentitiesToAWSResources"

This policy statement allows identities from your Organizations organization to send requests through a VPC endpoint to service-owned resources. You can list ARNs of service-owned resources in the `Resource` element of the statement. You can further restrict access by specifying allowed actions in the `Action` element of the statement. See the [service_owned_resources](../service_owned_resources.md) for a list of service-owned resources that can be accessed by your IAM credentials.

### "Sid":"AllowRequestsByThirdPartyIdentitiesToThirdPartyResources"

This policy statement allows trusted identities outside of your Organizations organization to send requests to trusted resources owned by an account that does not belong to your organization. List ARNs of resources in the `Resource` element of the statement. Further restrict access by specifying allowed actions in the `Action` element of the statement. An example valid use case is a third party integration that requires you to allow your applications to upload or download objects from a third party S3 bucket by using third party generated presigned Amazon S3 URLs. In this case, the principal that generates the presigned URL will belong to the third party AWS account. 

### "Sid":"AllowRequestsByOrgsIdentitiesToThirdPartyResources"

This policy statement allows identities from your Organizations organization to send requests to trusted resources owned by an account that does not belong to your organization. List ARNs of resources in the `Resource` element of the statement. Further restrict access by specifying allowed actions in the `Action` element of the statement. 

### "Sid":"AllowRequestsByOrgsIdentitiesToAnyResources"

This policy statement allows identities from your Organizations organization that are tagged with the `dp:exclude:resource` tag set to `true` to access any resource. Before adding this statement to your VPC endpoint policy, ensure that you have strong tagging governance in place and a valid data-access pattern that warrants its implementation that is not already covered by previously described statements. If you include this statement in your policy, ensure that you always have this access restricted to principals in your Organizations organization by using the `aws:PrincipalOrgID` condition key. This prevents access by identities outside your organization tagged with the same tag key and value. 

