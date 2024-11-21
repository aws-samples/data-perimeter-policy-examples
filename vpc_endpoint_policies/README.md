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

This policy statement allows [AWS service principals](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_principal.html#principal-services) to send requests to AWS owned resources on your behalf through a VPC endpoint. The `aws:PrincipalIsAWSService` IAM condition key is used to denote this in the policy. Though AWS services rarely use their service principals to make calls from your VPCs, some services that operate within your network might need this statement to be present in the VPC endpoint policies to ensure normal operations. 

Example data access patterns:

* *AWS CloudFormation wait condition and custom resource.* [AWS CloudFormation](https://aws.amazon.com/cloudformation/) maintains [Amazon Simple Storage Service (Amazon S3)](https://aws.amazon.com/s3/) [service-owned buckets in each AWS Region](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-vpce-bucketnames.html#cfn-setting-up-vpc-considerations) to monitor responses to a custom resource request or a wait condition. If your CloudFormation template includes [custom resources](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-custom-resources.html) deployed in a VPC or [wait conditions](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-waitcondition.html) for resources deployed in your VPC, the Amazon S3 VPC endpoint policy must allow users to send responses to those buckets owned by AWS. CloudFormation uses the `cloudformation.amazonaws.com` service principal to create a presigned Amazon S3 URL, which is used to send requests to Amazon S3.
* *Access to Amazon Q Developer Pro buckets.* [Amazon Q Developer](https://aws.amazon.com/q/developer/) uploads artifacts to [AWS owned S3 buckets](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/security_iam_manage-access-with-policies.html#data-perimeters) to deliver its functionality. Access to these buckets is via a presigned URL created by an AWS service principal.

### "Sid":"AllowRequestsToAWSOwnedResources"

This policy statement allows access to specific AWS owned resources through a VPC endpoint. You can list ARNs of AWS owned resources in the `Resource` element of the statement. You can further restrict access by specifying allowed actions in the `Action` element of the statement. 

Example data access patterns:

* *Amazon Elastic Compute Cloud (Amazon EC2) Linux patching.* [Kernel Live Patching on Amazon Linux 2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/al2-live-patching.html) and [Kernel Live Patching on Amazon Linux 2023](https://docs.aws.amazon.com/linux/al2023/ug/live-patching.html) allows you to apply security vulnerability and critical bug patches to a running Linux kernel. To download packages from Amazon Linux repositories hosted on AWS owned Amazon S3 buckets, [Amazon EC2](https://aws.amazon.com/ec2/) makes an unauthenticated call to Amazon S3. The call originates from your VPC and passes through the Amazon S3 VPC endpoint. 

    * [AWS owned buckets for Kernel Live Patching on Amazon Linux 2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/al2-live-patching.html):

        * `arn:aws:s3:::packages.<region>.amazonaws.com/*`,
        * `arn:aws:s3:::repo.<region>.amazonaws.com/*`,
        * `arn:aws:s3:::amazonlinux.<region>.amazonaws.com/*`,
        * `arn:aws:s3:::amazonlinux-2-repos-<region>/*`

    * [AWS owned bucket for Kernel Live Patching on Amazon Linux 2023](https://docs.aws.amazon.com/linux/al2023/ug/live-patching.html):
        * `arn:aws:s3:::al2023-<region>/*`
        * `arn:aws:s3:::al2023-repos-<region>-de612dc2/*`


* *Amazon EMR patching and Spark logging.* Amazon EMR uses Amazon Linux repositories that are hosted in AWS owned Amazon S3 buckets to [launch and manage instances within Amazon EMR clusters](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-default-ami.html). Amazon EMR also collects [Spark event logs](https://docs.aws.amazon.com/emr/latest/ManagementGuide/app-history-spark-UI.html) in an AWS owned system bucket. To do so, Amazon EMR makes unauthenticated calls to Amazon S3. These calls originate from your VPC and pass through the Amazon S3 VPC endpoint. 

    * [AWS owned buckets](https://docs.aws.amazon.com/emr/latest/ManagementGuide/private-subnet-iampolicy.html):

        * `arn:aws:s3:::packages.<region>.amazonaws.com/*`,
        * `arn:aws:s3:::repo.<region>.amazonaws.com/*`,
        * `arn:aws:s3:::amazonlinux.<region>.amazonaws.com/*`,
        * `arn:aws:s3:::amazonlinux-2-repos-<region>/*`,
        * `arn:aws:s3:::repo.<region>.emr.amazonaws.com/*`,
        * `arn:aws:s3:::prod.<region>.appinfo.src/*`

* *AWS Systems Manager operations.* When using [AWS Systems Manager](https://aws.amazon.com/systems-manager/) capabilities such as patching, [SSM Agent](https://docs.aws.amazon.com/systems-manager/latest/userguide/ssm-agent.html) running on your Amazon EC2 instances could make unauthenticated requests to various AWS owned Amazon S3 buckets to perform its operations. These calls originate from your VPC and pass through the Amazon S3 VPC endpoint. 

    * [AWS owned buckets](https://docs.aws.amazon.com/systems-manager/latest/userguide/ssm-agent-minimum-s3-permissions.html):

        * `arn:aws:s3:::aws-ssm-<region>/*`,
        * `arn:aws:s3:::aws-windows-downloads-<region>/*`,
        * `arn:aws:s3:::amazon-ssm-<region>/*`,
        * `arn:aws:s3:::amazon-ssm-packages-<region>/*`,
        * `arn:aws:s3:::<region>-birdwatcher-prod/*`,
        * `arn:aws:s3:::aws-ssm-distributor-file-<region>/*`,
        * `arn:aws:s3:::aws-ssm-document-attachments-<region>/*`,
        * `arn:aws:s3:::patch-baseline-snapshot-<region>/*`,
        * `arn:aws:s3:::aws-patchmanager-macos-<region>/*`

* *Using Amazon CloudWatch agent.* When you install the [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/) agent on your Amazon EC2 instances, Amazon EC2 makes an unauthenticated call to Amazon S3 to download the package from an AWS owned Amazon S3 bucket. The call originates from your VPC and passes through the Amazon S3 VPC endpoint. 

    * [AWS owned buckets](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/download-cloudwatch-agent-commandline.html):

        * `arn:aws:s3:::amazoncloudwatch-agent-<region>/*`,
        * `arn:aws:s3:::amazoncloudwatch-agent/*`



* *Using AWS CodeDeploy agent.* When you install the [AWS CodeDeploy](https://aws.amazon.com/codedeploy/) agent on your Amazon EC2 instances, Amazon EC2 makes an unauthenticated call to Amazon S3 to download the agent from an AWS owned Amazon S3 bucket. The call originates from your VPC and passes through the Amazon S3 VPC endpoint. 

    * [AWS owned bucket](https://docs.aws.amazon.com/codedeploy/latest/userguide/instances-ec2-configure.html):

        * `arn:aws:s3:::aws-codedeploy-<region>/*`



* *Using EC2 Image Builder components*. [EC2 Image Builder](https://aws.amazon.com/image-builder/) uses a publicly available Amazon S3 bucket to store and access managed resources, such as components. It also downloads the AWSTOE component management application from a separate Amazon S3 bucket. The call to Amazon S3 is unauthenticated and passes through the Amazon S3 VPC endpoint. 

    * [AWS owned buckets](https://docs.aws.amazon.com/imagebuilder/latest/userguide/vpc-interface-endpoints.html):

        * `arn:aws:s3:::ec2imagebuilder-toe-<region>-prod/*`,
        * `arn:aws:s3:::ec2imagebuilder-managed-resources-<region>-prod/components/*`


* *Creation of containers with Amazon ECR images.* [Amazon Elastic Container Registry (Amazon ECR)](https://aws.amazon.com/ecr/) uses AWS owned Amazon S3 buckets to store Amazon ECR private image layers. When your containers download images from Amazon ECR, they must access Amazon ECR to get the image manifest and then Amazon S3 to download the actual image layers. A call to Amazon S3 is signed using a presigned URL, which is created by an Amazon ECR account and not the service principal. The call originates from your VPC and passes through the Amazon S3 VPC endpoint.  

    * [AWS owned bucket](https://docs.aws.amazon.com/AmazonECR/latest/userguide/vpc-endpoints.html#ecr-minimum-s3-perms):

        * `arn:aws:s3:::prod-<region>-starport-layer-bucket/*`

* *AWS Application Migration Service source server replication.* [AWS Replication Agent](https://docs.aws.amazon.com/mgn/latest/ug/agent-installation.html) allows you to add source servers to the AWS Application Migration service to monitor their migration lifecycle and data replication state. To download the agent installer and components hosted on AWS owned Amazon S3 buckets, Application Migration Service uses presigned URLs create by an Application Migration Service account. These calls originate from your VPC and pass through the Amazon S3 VPC endpoint.

    * [AWS owned buckets]( https://docs.aws.amazon.com/mgn/latest/ug/Network-Requirements.html):

        * `arn:aws:s3:::aws-mgn-clients-<region>/*`,
        * `arn:aws:s3:::aws-mgn-clients-hashes-<region>/*`,
        * `arn:aws:s3:::aws-mgn-internal-<region>/*`,
        * `arn:aws:s3:::aws-mgn-internal-hashes-<region>/*`,
        * `arn:aws:s3:::aws-application-migration-service-<region>/*`,
        * `arn:aws:s3:::aws-application-migration-service-hashes-<region>/*`,
        * `arn:aws:s3:::amazon-ssm-<region>/*`

* *AWS Elastic Disaster Recovery operations.* Elastic Disaster Recovery uses AWS owned Amazon S3 buckets to store and access managed resources used to perform operations. The [AWS Replication Agent installer](https://docs.aws.amazon.com/drs/latest/userguide/agent-installation.html) uses a presigned URL, which is signed by the service account, to make these requests to various service-owned Amazon S3 buckets. These calls originate from your VPC and pass through the Amazon S3 VPC endpoint.
    * [AWS owned buckets](https://docs.aws.amazon.com/drs/latest/userguide/Network-Requirements.html):

        * `arn:aws:s3:::aws-drs-clients-<region>/*`,
        * `arn:aws:s3:::aws-drs-clients-hashes-<region>/*`,
        * `arn:aws:s3:::aws-drs-internal-<region>/*`,
        * `arn:aws:s3:::aws-drs-internal-hashes-<region>/*`,
        * `arn:aws:s3:::aws-elastic-disaster-recovery-<region>/*`,
        * `arn:aws:s3:::aws-elastic-disaster-recovery-hashes-<region>/*`

* *AWS CloudFormation wait conditions.* CloudFormation uses AWS owned Amazon S3 buckets to to monitor responses to a [custom resource](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-custom-resources.html) request or a [wait condition](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-waitcondition.html). If a template includes custom resources or wait conditions in a VPC, the VPC endpoint policy must allow users to send responses to the following buckets:

    * [AWS owned buckets](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-vpce-bucketnames.html#cfn-setting-up-vpc-considerations)

        * `arn:aws:s3:::cloudformation-custom-resource-response-<RegionWithoutDashes>/*`, (_Note_: `<RegionWithoutDashes>` does not contain dashes, for example, `uswest2` instead of `us-west-2`.)
        * `arn:aws:s3:::cloudformation-waitcondition-<region>/*`,

* *ACM for AWS Nitro Enclaves.* ACM for AWS Nitro Enclaves uses an AWS owned Amazon S3 bucket to distribute a certificate to an EC2-hosted web server.

    * [AWS owned buckets](https://docs.aws.amazon.com/enclaves/latest/user/nitro-enclave-refapp.html#role-cert)

        * `arn:aws:s3:::aws-ec2-enclave-certificate-<region>/*`

* *AWS CodeArtifact operations.* CodeArtifact uses AWS owned Amazon S3 buckets to host the artifacts and redirects HTTP requests for an artifact repository URL to a presigned URL backed by one of their buckets.

    * [AWS owned buckets](https://docs.aws.amazon.com/codeartifact/latest/ug/create-s3-gateway-endpoint.html)

        * `arn:aws:s3:::assets-<CodeArtifaction-Region-Account>-<region>/*`

### "Sid":"AllowRequestsByOrgsIdentitiesToAWSResources"

This policy statement allows identities from your Organizations organization to send requests through a VPC endpoint to AWS owned resources. You can list ARNs of AWS owned resources in the `Resource` element of the statement. You can further restrict access by specifying allowed actions in the `Action` element of the statement. 

Example data access patterns:

* *Using Systems Manager public parameters, documents and automation runbooks owned by Amazon.* AWS automation or custom applications may need to access Systems Manager [public parameters](https://docs.aws.amazon.com/systems-manager/latest/userguide/parameter-store-public-parameters.html), [documents owned by Amazon](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-ssm-docs.html) or [automation runbooks](https://docs.aws.amazon.com/systems-manager/latest/userguide/running-automations.html) to support their operations. For example, Amazon EC2 publishes information about Amazon Machine Images (AMIs) as public parameters. Depending on the application and service in use, they can originate calls to access these parameters from your VPC and pass them through the Systems Manager VPC endpoint. 

    * [AWS owned parameters](https://docs.aws.amazon.com/systems-manager/latest/userguide/parameter-store-finding-public-parameters.html):

        * `arn:aws:ssm:<region>::parameter/aws/*`
        
    * [AWS owned documents](https://docs.aws.amazon.com/systems-manager/latest/userguide/security_iam_service-with-iam.html#security_iam_service-with-iam-id-based-policies-actions):

        * `arn:aws:ssm:<region>::document/*`

    * [AWS owned automation runbooks](https://docs.aws.amazon.com/systems-manager/latest/userguide/security_iam_service-with-iam.html#security_iam_service-with-iam-id-based-policies-actions):

        * `arn:aws:ssm:<region>::automation-definition/*`

* *Using EC2 Image builder components.* Image Builder uses the [AWS Task Orchestrator and Executor (AWSTOE)](https://docs.aws.amazon.com/imagebuilder/latest/userguide/toe-component-manager.html) component management application running on its Amazon EC2 build and test instances. If you are using Amazon managed components in your recipe, AWSTOE uses an instance profile to make a call to Image Builder to get those components, which passes through the Image Builder VPC endpoint. 

    * [AWS owned component](https://docs.aws.amazon.com/imagebuilder/latest/userguide/vpc-interface-endpoints.html):

        * `arn:aws:imagebuilder:<region>:aws:component/*`

* *Using Elastic Kubernetes Service add-ons.* [Amazon Elastic Kubernetes Service (Amazon EKS)](https://aws.amazon.com/eks/) uses an Amazon EKS managed Amazon Elastic Container Registry (Amazon ECR) private repository to host Docker container images for [Amazon EKS add-ons](https://docs.aws.amazon.com/eks/latest/userguide/eks-add-ons.html). Each Region has a dedicated private repository. If you use Amazon EKS managed node groups or want to have your Amazon EKS nodes pull the container images from the Amazon EKS private repository, your Amazon ECR VPC endpoint (com.amazonaws.region.ecr.api) policy must allow your principals to access the repository for the Region in which you are operating. 
    * [AWS owned repositories for EKS add-ons](https://docs.aws.amazon.com/eks/latest/userguide/add-ons-images.html):
        * In the policy example, replace `<ecr-account-id>` with the 12-digit account ID of the AWS account that hosts the private registry. These are the first 12 digits of the respective registry from the table on the [add-ons images documentation page](https://docs.aws.amazon.com/eks/latest/userguide/add-ons-images.html).


* *Using GuardDuty EKS Runtime Monitoring* [Amazon GuardDuty](https://aws.amazon.com/guardduty/) uses an Amazon EKS managed Amazon Elastic Container Registry (Amazon ECR) private repository to host Docker container images for [GuardDuty EKS Runtime Monitoring](https://docs.aws.amazon.com/guardduty/latest/ug/guardduty-eks-runtime-monitoring.html). Each Region has a dedicated private repository. If you use Amazon EKS managed node groups or want to have your Amazon EKS nodes pull the container images from the Amazon EKS private repository, your Amazon ECR VPC endpoint (com.amazonaws.region.ecr.api) policy must allow your principals to access the repository for the Region in which you are operating.      
    * [AWS owned repositories for GuardDuty EKS Runtime Monitoring](https://docs.aws.amazon.com/guardduty/latest/ug/eks-runtime-agent-ecr-image-uri.html):
        * In the policy example, replace `<ecr-account-id>` with the 12-digit account ID of the AWS account that hosts the private registry. These are the first 12 digits of the respective registry from the table on the [GuardDuty EKS Runtime Monitoring documentation page](https://docs.aws.amazon.com/guardduty/latest/ug/eks-runtime-agent-ecr-image-uri.html).


* *Using Amazon SageMaker Pre-built Docker images* [Amazon SageMaker](https://aws.amazon.com/sagemaker/) uses an Amazon SageMaker managed Amazon Elastic Container Registry (Amazon ECR) private repository to host some pre-built Docker container images for use with Amazon SageMaker. Each Region has a dedicated private repository. If you wish use these pre-built Docker container images directly from SageMaker's managed repository, your Amazon ECR VPC endpoint (com.amazonaws.region.ecr.api) policy must allow your principals to access the repository for the Region in which you are operating.      
    * [AWS owned repositories for Amazon SageMaker pre-built Docker container images](https://docs.aws.amazon.com/sagemaker/latest/dg-ecr-paths/sagemaker-algo-docker-registry-paths.html):
        * In the policy example, replace `<ecr-account-id>` with the 12-digit account ID of the AWS account that hosts the private registry. These are the first 12 digits of the respective registry from the table on the [Amazon SageMaker Documentation](https://docs.aws.amazon.com/sagemaker/latest/dg-ecr-paths/sagemaker-algo-docker-registry-paths.html). Note the 12-digit account ID may be different for each AWS region, and there is a seperate page for each AWS region.

* *Amazon Elastic Compute Cloud (Amazon EC2).* You can use Amazon owned AMIs to launch instances. `ec2:Owner` condition key value set to `amazon` is required for your users and applications to launch instances from all AMIs owned by Amazon, or certain trusted and verified partners.


### "Sid":"AllowRequestsByThirdPartyIdentitiesToThirdPartyResources"

This policy statement allows trusted identities outside of your Organizations organization to send requests to trusted resources owned by an account that does not belong to your organization. List ARNs of resources in the `Resource` element of the statement. Further restrict access by specifying allowed actions in the `Action` element of the statement. An example valid use case is a third party integration that requires you to allow your applications to upload or download objects from a third party S3 bucket by using third party generated presigned Amazon S3 URLs. In this case, the principal that generates the presigned URL will belong to the third party AWS account. 

### "Sid":"AllowRequestsByOrgsIdentitiesToThirdPartyResources"

This policy statement allows identities from your Organizations organization to send requests to trusted resources owned by an account that does not belong to your organization. List ARNs of resources in the `Resource` element of the statement. Further restrict access by specifying allowed actions in the `Action` element of the statement. 

### "Sid":"AllowRequestsByOrgsIdentitiesToAnyResources"

This policy statement allows identities from your Organizations organization that are tagged with the `dp:exclude:resource` tag set to `true` to access any resource. Before adding this statement to your VPC endpoint policy, ensure that you have strong tagging governance in place and a valid data-access pattern that warrants its implementation that is not already covered by previously described statements. If you include this statement in your policy, ensure that you always have this access restricted to principals in your Organizations organization by using the `aws:PrincipalOrgID` condition key. This prevents access by identities outside your organization tagged with the same tag key and value. 

