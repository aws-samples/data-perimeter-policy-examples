# Resource control policy (RCP) examples

Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved. SPDX-License-Identifier: CC-BY-SA-4.0

## Table of Contents

* [Introduction](#introduction)
* [Description](#description)
* [Included data access patterns](#included-data-access-patterns)

## Introduction

Some AWS services support resource-based policies that you can use to grant principals (including principals outside of your organization) permissions to perform actions on the resources to which they are attached. While allowing developers to configure resource-based policies based on their application requirements, you can enforce the identity and network perimeters on resources across your entire organization using resource control policies (RCPs). This helps prevent unintended access caused by misconfigurations and restrict access to your resources to expected networks only so that controls in the intended network path are not bypassed.

RCPs are a type of [AWS Organizations](https://aws.amazon.com/organizations/) organization policy that you can use to control the maximum available permissions for resources in your organization. RCPs help you enforce security invariants such as ensuring that your resources can only be accessed by identities in your organization and only from company-managed networks.

## Description

This folder contains examples of RCPs that help enforce identity and network perimeter controls on [services supported by RCPs](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_rcps.html#rcp-supported-services).

These examples do not represent a complete list of valid data access patterns, and they are intended for you to tailor and extend them to suit the needs of your environment. See [Tagging conventions](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main?tab=readme-ov-file#tagging-conventions-and-governance) used in the policy examples to control the scope of data perimeter guardrails. You can also use the `NotResource` IAM policy element to exclude specific resources from the controls.

Use the following RCP examples individually or in combination:
* [identity_perimeter_rcp](identity_perimeter_rcp.json) – Enforces identity perimeter controls on resources within your Organizations organization.
* [network_perimeter_rcp](network_perimeter_rcp.json) – Enforces network perimeter controls on resources within your Organizations organization.
* [data_perimeter_governance_rcp](data_perimeter_governance_rcp.json) – Includes controls for protecting data perimeter controls’ dependencies, such as session tags used to control their scope.

[service_specific_controls](service_specific_controls) subfolder contains policy examples you can implement as resource-based policies for select services that are not supported by RCPs and other service-specific controls.

Note that the policy examples in this folder do not grant any permissions; they only restrict access by explicitly denying specific data access patterns. You still have to grant appropriate permissions with explicit `Allow` statements in identity-based or resource-based policies.  

## Included data access patterns

The following policy statements are included in the RCP and resource-based policy examples, each statement representing specific data access patterns.

These policy statements demonstrate using `aws:ResourceTag/tag-key` to exclude specific resources from the control. Note that this key only works with resources that [support authorization based on tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html). For details on supported service actions, see the [Service Authorization Reference](https://docs.aws.amazon.com/service-authorization/latest/reference/reference.html). For resources not yet supporting `aws:ResourceTag/tag-key`, you can use `aws:ResourceAccount` or `aws:ResourceOrgPaths` to exclude resources owned by specific AWS accounts, or the `NotResource` IAM policy element to exclude specific resource Amazon Resource Names (ARNs). Alternatively, you can use a service-specific version of `aws:ResourceTag/tag-key` such as `s3:ExistingObjectTag`, if available.

### "Sid":"EnforceOrgIdentities"

This policy statement is included in the [identity_perimeter_rcp](identity_perimeter_rcp.json) and limits access to trusted identities:

* Identities within your AWS Organizations organization are specified by the organization ID (`<my-org-id>`) in the policy statement.
* AWS services that use [service principals](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_principal.html#principal-services) to access resources on your behalf are denoted by `aws:PrincipalIsAWSService` in the policy statement. See `"Sid": "EnforceConfusedDeputyProtection"` that uses the `aws:SourceOrgID` condition key to further restrict AWS services’ actions so that they can only interact with your resources on your behalf.
* Trusted identities outside of your Organizations organization are specified by the account IDs of third parties (`<third-party-account-a>` and `<third-party-account-b>`) in the policy statement.

Example data access patterns:

* *Elastic Load Balancing (ELB) access logging*. In some AWS Regions, [Classic Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/enable-access-logs.html#attach-bucket-policy) and [Application Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/enable-access-logging.html#attach-bucket-policy) use AWS account credentials that belong to an AWS service to publish logs to your Amazon S3 buckets. The `aws:PrincipalAccount` condition key in the resource control policy should contain the ELB account ID if access logging is enabled.
* *Amazon FinSpace data encryption*. To encrypt data at rest, [Amazon FinSpace](https://aws.amazon.com/finspace/) uses AWS account credentials that belong to the service to access your [AWS Key Management Service (AWS KMS)](https://aws.amazon.com/kms/) customer managed key. The `aws:PrincipalAccount` condition key in the resource control policy should contain the [FinSpace environment infrastructure account](https://docs.aws.amazon.com/finspace/latest/userguide/data-sharing-lake-formation.html). You can find the ID of the infrastructure account that's dedicated to your FinSpace environment on the environment page of the FinSpace console.

Additional considerations:
* The `aws:PrincipalOrgID` condition key is included in the request context only if the calling principal is a member of an organization. This is not the case with federated users; therefore, `sts:AssumeRoleWithSAML` and `sts:AssumeRoleWithWebIdentity` are not listed in the `Action` element of the policy statement `"Sid": "EnforceOrgIdentities"`. `sts:SetSourceIdentity` and `sts:TagSession` are also not included to prevent impact on `sts:AssumeRoleWithSAML` and `sts:AssumeRoleWithWebIdentity` that [set a source identity]( https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_monitor.html#id_credentials_temp_control-access_monitor-setup) or [pass session tags]( https://docs.aws.amazon.com/IAM/latest/UserGuide/id_session-tags.html#id_session-tags_operations). We recommend using the `"Sid":"EnforceTrustedOIDCProviders"` and `"Sid":"EnforceTrustedOIDCTenants` statements to help prevent requests from untrusted OpenID Connect (OIDC) tenants. To help ensure that only your trusted identity providers can be used for SAML federation, limit your principals’ ability to make configuration changes to the IAM SAML identity providers (see`"Sid":"PreventIdPTrustModifications"` in the [restrict_idp_configurations_scp](../service_control_policies/service_specific_controls/restrict_idp_configurations_scp.json)).<br /><br /> Another STS action, `sts:GetCallerIdentity`, is not included in this statement because [no permissions are required to perform this operation](https://docs.aws.amazon.com/STS/latest/APIReference/API_GetCallerIdentity.html). 

* If you have [Amazon CloudFront](https://aws.amazon.com/cloudfront/) distributions configured to use origin access identity (OAI) to send requests to an Amazon S3 origin, this statement will prevent CloudFront from communicating with the origin. We recommend [migrating from origin access identity (OAI) to origin access control (OAC)](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html#migrate-from-oai-to-oac). If you need to maintain OAI support during the migration, you can implement an exception using the `aws:PrincipalArn` condition key, setting the unique OAI user ARNs from your distributions as the value (`arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity <my-origin-access-identity-ID>`).

* This policy statement demonstrates using `aws:ResourceTag/tag-key` to exclude specific resources from the control. Note that this key only works with resources that [support authorization based on tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html). For details on supported service actions, see the [Service Authorization Reference](https://docs.aws.amazon.com/service-authorization/latest/reference/reference.html). For resources not yet supporting `aws:ResourceTag/tag-key`, you can use `aws:ResourceAccount` or `aws:ResourceOrgPaths` to exclude resources owned by specific AWS accounts, or the `NotResource` IAM policy element to exclude specific resource Amazon Resource Names (ARNs). Alternatively, you can use a service-specific version of `aws:ResourceTag/tag-key` such as `s3:ExistingObjectTag`, if available.

### "Sid":"EnforceTrustedOIDCProviders"

This policy statement is included in the [identity_perimeter_rcp](identity_perimeter_rcp.json) and limits access to `sts:AssumeRoleWithWebIdentity` to only federated identities associated with a specific OIDC provider. 

If you only need to support single-tenant OIDC providers, this statement is sufficient for preventing untrusted federated identities because the issuer name in the `sub` claim is unique to your organization.

If you need to support OIDC federation with providers that support multiple tenants (e.g. GitHub Actions, Salesforce), use `"Sid": "EnforceTrustedOIDCTenants"` statement in the [identity_perimeter_rcp](identity_perimeter_rcp.json) to limit access to only federated identities originating from your tenant of a trusted OIDC provider. 

### "Sid":"EnforceTrustedOIDCTenants"

This policy statement is included in the [identity_perimeter_rcp](identity_perimeter_rcp.json) and limits access to `sts:AssumeRoleWithWebIdentity` to only federated identities originating from your tenant of a trusted multi-tenant OIDC provider. 

If you need to support OIDC federation with multi-tenant providers (e.g. GitHub Actions, Salesforce), you can use [condition keys specific to each identity provider type](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_iam-condition-keys.html#condition-keys-wif) in your policy to restrict access to your tenant only. Typically, the subject (`sub`) claim of the identity provider JSON Web Token (JWT) contains a value that is used to identify your tenant within a multi-tenant system. To understand the expected values of the `sub` claim, see the documentation of your provider. 

If you need to support multiple multi-tenant OIDC providers, you need to create this statement for each provider with the `Null` condition operator so that the correct value of your tenant within each provider is enforced on each `sts:AssumeRoleWithWebIdentity` request.

If you only need to support single-tenant OIDC providers, you can remove this statement from the policy. In this case, `"Sid": "EnforceTrustedOIDCProviders"` statement in the [identity_perimeter_rcp](identity_perimeter_rcp.json) is sufficient for preventing untrusted federated identities because the issuer name in the `sub` claim is unique to your organization.


### "Sid":"EnforceConfusedDeputyProtection"

This policy statement is included in the [identity_perimeter_rcp](identity_perimeter_rcp.json) and restricts access to your resources so that AWS services can only interact with them on your behalf:

* AWS services that use service principals to access your resources on behalf of another resource that belongs to your organization, specified by the organization ID (`<my-org-id>`) in the policy statement.
* AWS services that use service principals to access your resources on behalf of another resource that belongs to trusted third parties, specified by the account IDs of third parties (`<third-party-account-a>` and `<third-party-account-b>`) in the policy statement.

This policy applies the [cross-service confused deputy protection]( https://docs.aws.amazon.com/IAM/latest/UserGuide/confused-deputy.html#cross-service-confused-deputy-prevention) only on service integrations that require the use of the `aws:SourceAccount`, `aws:SourceOrgPaths`, and `aws:SourceOrgID` condition keys. We use the `Null` condition operator with the `aws:SourceAccount` condition key so that the `"Sid": "EnforceConfusedDeputyProtection"` statement applies only to requests that require the use of the keys. If the `aws:SourceAccount` is present in the request context, the `Null` condition will evaluate to `true` and cause the `aws:SourceOrgID` to be enforced. We use `aws:SourceAccount` instead of `aws:SourceOrgID` in the `Null` condition operator so that the control still applies if the request originates from an account that doesn’t belong to an organization. When a service enables support for the `aws:SourceAccount`, `aws:SourceOrgPaths`, and `aws:SourceOrgID` condition keys, it will automatically be subject to this policy.

Example data access patterns:

* *CloudTrail log delivery.* [AWS CloudTrail](https://aws.amazon.com/cloudtrail/) uses its service principal, cloudtrail.amazonaws.com, to publish events to an [Amazon S3 bucket that you specify](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-create-and-update-a-trail.html). 
* *VPC Flow Logs log delivery.* [VPC Flow Logs](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html) uses its service principal, delivery.logs.amazonaws.com, to [deliver network logs to an Amazon S3 bucket](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-s3.html) that you specify. 
* *AWS services using service roles.* Some services assume service roles that you create to perform actions on your behalf. Not all services support `aws:SourceOrgID` enforcement on their `sts:AssumeRole` calls; however, every service performs the `iam:PassRole` action to verify that the role is in the same account as the calling service. As a result, using  `aws:SourceOrgID` for the `sts:AssumeRole` calls is not necessary. The `Null` condition operator with the `aws:SourceAccount` condition key accounts for these service integrations.
* *AWS services using AWS KMS grants.* Some services that use AWS KMS grants to encrypt/decrypt your resources don’t support `aws:SourceOrgID` enforcement on their calls against your AWS KMS keys. However, AWS KMS grants used by services include the encryption context that restricts the use of the grant so that it is only on behalf of a resource it was originally created for. The `Null` condition operator with the `aws:SourceAccount` condition key accounts for these service integrations.
* *Elastic Load Balancing (ELB) access logging*. In some AWS Regions, [Classic Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/enable-access-logs.html#attach-bucket-policy) and [Application Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-access-logs.html#access-logging-bucket-permissions) use their service principals to publish logs to your Amazon S3 buckets but don’t populate the `aws:SourceOrgID` condition key. The `Null` condition operator with the `aws:SourceAccount` condition key accounts for this service integration. The name of the log file stored in your S3 bucket always contains the account ID of the account with the configured load balancer. When you grant access to your bucket for logging, you can scope it down to the specific path in your bucket that contains the account ID. 


### "Sid":"EnforceNetworkPerimeter"

This policy statement is included in the [network_perimeter_rcp](network_perimeter_rcp.json) and limits access to expected networks for IAM principals tagged with the `dp:include:network` tag set to `true` and federated users. Expected networks are defined as follows:

* Your organization’s on-premises data centers and static egress points in AWS such as a NAT gateway that are specified by IP ranges (`<my-corporate-cidr>`) in the policy statement. 
* Your organization’s VPCs that are specified by VPC IDs (`<my-vpc>`) in the policy statement.  
* Networks of AWS services that use your credentials to access resources using [forward access sessions (FAS)]( https://docs.aws.amazon.com/IAM/latest/UserGuide/access_forward_access_sessions.html) as denoted by `aws:ViaAWSService` in the policy statement. This access pattern applies when you access data via an AWS service, and that service takes subsequent actions on your behalf by using your IAM credentials. 
* Networks of AWS services that use [service principals](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_principal.html#principal-services) to access resources on your behalf are denoted by `aws:PrincipalIsAWSService` in the policy statement. Use the `"Sid": "EnforceConfusedDeputyProtection"` in the [identity_perimeter_rcp](identity_perimeter_rcp.json) to further restrict AWS service actions so that they can only interact with your resources when performing operations on behalf of accounts that you own.
* Networks of AWS services that use [service roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html) to access resources on your behalf as denoted by `arn:aws:iam::*:role/aws:ec2-infrastructure` in the policy statement.
* Networks of trusted third parties are specified by their account IDs (`<third-party-account-a>` and `<third-party-account-b>`) in the policy statement.

Additional considerations:

* This policy statement exempts identities that are tagged with `dp:exclude:network` set to `true` from the network perimeter guardrail. Note that it is not recommended to have this exception in the policy unless it is accompanied by `"Sid": "EnforceOrgIdentities"`. This helps ensure that an account outside of your Organizations organization cannot tag their identities with `dp:exclude:network` to circumvent your network perimeter controls.

Example data access patterns:

* *Amazon Athena query*. When an application running in your VPC [creates an Athena query](https://docs.aws.amazon.com/athena/latest/ug/getting-started.html), Athena uses the application’s credentials to make subsequent requests to Amazon S3 to read data from your bucket and return results. 
* *Elastic Beanstalk operations.* When a user [creates an application by using Elastic Beanstalk](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/applications.html), Elastic Beanstalk launches an environment and uses your user’s credentials to create and configure other AWS resources such as Amazon EC2 instances. 
* *Amazon S3 server-side encryption with AWS KMS (SSE-KMS)*. When a user uploads an object to an Amazon S3 bucket with the default [SSE-KMS encryption](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingKMSEncryption.html) enabled, Amazon S3 makes a subsequent request to [AWS Key Management Service (AWS KMS)](https://aws.amazon.com/kms/) to generate a data key from your customer master key (CMK) to encrypt the object. The call to AWS KMS is signed by using the user’s credentials and comes from the service network. A similar pattern is observed when a user tries to download an encrypted object when Amazon S3 calls AWS KMS to decrypt the key that was used to encrypt the object. Other services such as AWS Secrets Manager use a similar integration with AWS KMS.
* *Amazon EBS volume decryption*. When you mount an encrypted Amazon EBS volume to an Amazon EC2 instance, Amazon EC2 calls AWS KMS to decrypt the data key that was used to encrypt the volume. The call to AWS KMS is signed by an IAM role, `arn:aws:iam::*:role/aws:ec2-infrastructure`, which is created in your account by Amazon EC2, and comes from the service network.
* *Elastic Load Balancing (ELB) access logging*. In some AWS Regions, [Classic Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/enable-access-logs.html#attach-bucket-policy) and [Application Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-access-logs.html#access-logging-bucket-permissions) use AWS account credentials that belong to an AWS service to publish logs to your Amazon S3 buckets. Because the call to Amazon S3 comes from the service network, the `aws:PrincipalAccount` condition key in the resource control policy should contain the ELB account ID if access logging is enabled.
* *Amazon FinSpace data encryption*. To encrypt data at rest, [Amazon FinSpace](https://aws.amazon.com/finspace/) uses AWS account credentials that belong to the service to access your AWS KMS customer managed key. Because the call to AWS KMS comes from the service network, the `aws:PrincipalAccount` condition key in the resource control policy should contain the [FinSpace environment infrastructure account](https://docs.aws.amazon.com/finspace/latest/userguide/data-sharing-lake-formation.html). You can find the ID of the infrastructure account that's dedicated to your FinSpace environment on the environment page of the FinSpace console.

### "Sid":"ProtectDataPerimeterSessionTags"

This statement is included in the [data_perimeter_governance_rcp](data_perimeter_governance_rcp.json) and prevents your trusted third parties from passing session tags used for data perimeter authorization controls while assuming a role in your account (`sts:AssumeRole`). You can apply this statement when using SAML federation in your organization.