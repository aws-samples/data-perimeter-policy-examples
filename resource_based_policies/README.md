# Resource-based policy examples

Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved. SPDX-License-Identifier: CC-BY-SA-4.0

## Table of Contents

* [Introduction](#introduction)
* [Description](#description)
* [Included data access patterns](#included-data-access-patterns)

## Introduction

Some AWS services support resource-based IAM policies that you can use to grant principals (including principals outside of your organization) permissions to perform actions on the resources to which they are attached.  While allowing developers to configure resource-based policies based on their application requirements, you can enforce the identity and network perimeters within those policies. This helps to prevent unintended data disclosure caused by misconfigurations and access to your resources from unexpected networks bypassing other controls that may exist in the intended network path.

## Description

This folder contains examples of resource-based policies that enforce identity and network perimeter controls. These examples do not represent a complete list of valid data access patterns, and they are intended for you to tailor and extend them to suit the needs of your environment. For your network perimeter, this folder has examples of policies for enforcing controls on IAM principals tagged with the `data-perimeter-inlclude` tag set to `true`.

Note that the example policies do not grant any permissions; they only restrict access by explicitly denying specific data access patterns. You would need to add explicit `Allow` statements to the policies to grant access as required by your use of AWS services. 

Because developers will be creating resources such as S3 buckets and AWS KMS keys on a regular basis, you might need to implement automation to enforce identity and network perimeter controls when those resources are created or their policies are changed. One option is to use custom [AWS Config rules](https://docs.aws.amazon.com/config/latest/developerguide/evaluate-config_develop-rules.html)_._ Alternatively, you can choose to enforce resource deployment through [AWS Service Catalog](https://aws.amazon.com/servicecatalog/?aws-service-catalog.sort-by=item.additionalFields.createdDate&aws-service-catalog.sort-order=desc) or a CI/CD pipeline. With the AWS Service Catalog approach, you can have identity and network perimeter controls built into the centrally controlled products that are made available to developers to deploy within their accounts. With the CI/CD pipeline approach, the pipeline can have built-in compliance checks that enforce identity and network perimeter controls during the deployment. If you are deploying resources with your CI/CD pipeline by using [AWS CloudFormation](https://aws.amazon.com/cloudformation/), see the blog post [Proactively keep resources secure and compliant with AWS CloudFormation Hooks](https://aws.amazon.com/blogs/mt/proactively-keep-resources-secure-and-compliant-with-aws-cloudformation-hooks/).

## Included data access patterns

The following policy statements are included in the resource-based policy examples, each statement representing specific data access patterns.

### "Sid": "EnforceIdentityPerimeter"

This policy statement limits access to trusted identities:

* Identities within your AWS Organizations organization are specified by the organization ID (`<my-org-id>`) in the policy statement.
* AWS services that use [service principals](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_principal.html#principal-services) to access resources on your behalf are denoted by `aws:PrincipalIsAWSService` in the policy statement. Use the `aws:SourceAccount` condition key to further restrict AWS service actions so that they can only interact with your resources when performing operations on behalf of accounts that you own.
* Trusted identities outside of your Organizations organization are specified by the account IDs of third parties (`<third-party-account-a>` and `<third-party-account-b>`) in the policy statement.

Example data access patterns:

* *CloudTrail log delivery.* [AWS CloudTrail](https://aws.amazon.com/cloudtrail/) uses its service principal, cloudtrail.amazonaws.com, to publish events to an [Amazon S3 bucket that you specify](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-create-and-update-a-trail.html). 
* *VPC Flow Logs log delivery.* [VPC Flow Logs](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html) uses its service principal, delivery.logs.amazonaws.com, to [deliver network logs to an Amazon S3 bucket](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-s3.html) that you specify. 
* *Elastic Load Balancing (ELB) access logging*. [Classic Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/enable-access-logs.html#attach-bucket-policy) and [Application Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-access-logs.html#access-logging-bucket-permissions) use AWS account credentials that belong to an AWS service to publish logs to your Amazon S3 buckets. The `aws:PrincipalAccount` condition key in the Amazon S3 bucket policy should contain the ELB account ID if access logging is enabled.

You can use `"Sid":"PreventIdPTrustModifications"` in the [data_perimeter_governance_policy_2](data_perimeter_governance_policy_2.json) to prevent your developers from establishing trusts with external identity providers.

Note that [iam_role_trust_policy.json](iam_role_trust_policy.json) policy example prevents users from assuming a role using Web Identity or OpenID Connect (OIDC) federated identity providers . If you need to support Web Identity or OpenID Connect (OIDC) federation while restricting access to your approved application (or site)  IDs, you can use additional [condition keys specific to each identity provider type](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_iam-condition-keys.html#condition-keys-wif) in your policy.

### "Sid": "EnforceNetworkPerimeter"

This policy statement limits access to expected networks for IAM principals tagged with the `data-perimeter-include` tag set to `true` and federated users. Expected networks are defined as follows:

* Your organization’s on-premises data centers and static egress points in AWS such as a NAT gateway that are specified by IP ranges (`<my-corporate-cidr>`) in the policy statement. 
* Your organization’s VPCs that are specified by VPC IDs (`<my-vpc>`) in the policy statement.  
* Networks of AWS services that use your credentials to access resources on your behalf as denoted by `aws:ViaAWSService` in the policy statement. This access pattern applies when you access data via an AWS service, and that service takes subsequent actions on your behalf by using your IAM credentials. 
* Networks of AWS services that use [service principals](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_principal.html#principal-services) to access resources on your behalf are denoted by `aws:PrincipalIsAWSService` in the policy statement. Use the `aws:SourceAccount` condition key to further restrict AWS service actions so that they can only interact with your resources when performing operations on behalf of accounts that you own.
* Networks of AWS services that use [service-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) and [service roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html) to access resources on your behalf as denoted by `arn:aws:iam::<my-account-id>:role/aws-service-role/*`  and `arn:aws:iam::*:role/aws:ec2-infrastructure` in the policy statement.
* Networks of trusted third parties are specified by their account IDs (`<third-party-account-a>` and `<third-party-account-b>`) in the policy statement.

Example data access patterns:

* *Amazon Athena query*. When an application running in your VPC [creates an Athena query](https://docs.aws.amazon.com/athena/latest/ug/getting-started.html), Athena uses the application’s credentials to make subsequent requests to Amazon S3 to read data from your bucket and return results. 
* *Elastic Beanstalk operations.* When a user [creates an application by using Elastic Beanstalk](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/applications.html), Elastic Beanstalk launches an environment and uses your user’s credentials to create and configure other AWS resources such as Amazon EC2 instances. 
* *Amazon S3 server-side encryption with AWS KMS (SSE-KMS)*. When a user uploads an object to an Amazon S3 bucket with the default [SSE-KMS encryption](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingKMSEncryption.html) enabled, Amazon S3 makes a subsequent request to [AWS Key Management Service (AWS KMS)](https://aws.amazon.com/kms/) to generate a data key from your customer master key (CMK) to encrypt the object. The call to AWS KMS is signed by using the user’s credentials. A similar pattern is observed when a user tries to download an encrypted object when Amazon S3 calls AWS KMS to decrypt the key that was used to encrypt the object. Other services such as AWS Secrets Manager use a similar integration with AWS KMS.
* *Amazon EBS volume decryption*. When you mount an encrypted Amazon EBS volume to an Amazon EC2 instance, Amazon EC2 calls AWS KMS to decrypt the data key that was used to encrypt the volume. The call to AWS KMS is signed by an IAM role, `arn:aws:iam::*:role/aws:ec2-infrastructure`, which is created in your account by Amazon EC2. 
* *AWS Identity and Access Management Access Analyzer*. AWS Identity and Access Management Access Analyzer uses the service-linked role named `AWSServiceRoleForAccessAnalyzer` to analyze resource metadata.
* *Elastic Load Balancing (ELB) access logging*. [Classic Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/enable-access-logs.html#attach-bucket-policy) and [Application Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-access-logs.html#access-logging-bucket-permissions) use AWS account credentials that belong to an AWS service to publish logs to your Amazon S3 buckets. The `aws:PrincipalAccount` condition key in the Amazon S3 bucket policy should contain the ELB account ID if access logging is enabled.

This policy statement exempts identities that are tagged with `network-perimeter-exception` set to `true` from the network perimeter guardrail. Note that it is not recommended to have this exception in the policy unless it is accompanied by `"Sid": "EnforceIdentityPerimeter"`. This helps ensure that an account outside of your Organizations organization cannot tag their identities with `network-perimeter-exception` to circumvent your network perimeter controls.
