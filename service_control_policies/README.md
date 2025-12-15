# Service control policy (SCP) examples

Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved. SPDX-License-Identifier: CC-BY-SA-4.0

## Table of Contents

* [Introduction](#introduction)
* [Description](#description)
* [Included data access patterns](#included-data-access-patterns)

## Introduction

SCPs are a type of [AWS Organizations](https://aws.amazon.com/organizations/) organization policy that you can use to establish the maximum access that can be delegated by account administrators to principals within your organization. SCPs allow you to enforce security invariants such as ensuring that your identities can access only company-approved data stores and that your corporate credentials can be used only from company-managed networks.

## Description

This folder contains examples of SCPs that enforce resource and network perimeter controls. These examples do not represent a complete list of valid data access patterns, and they are intended for you to tailor and extend them to suit the needs of your environment. 

Use the following example SCPs individually or in combination:

* [resource_perimeter_scp](resource_perimeter_scp.json) – Enforces resource perimeter controls on all principals within your Organizations organization.
* [network_perimeter_vpceorgid_scp](network_perimeter_vpceorgid_scp.json), [network_perimeter_sourcevpc_scp](network_perimeter_sourcevpc_scp.json) – Enforces network perimeter controls on IAM principals tagged with the `dp:include:network` tag set to `true`.
* [data_perimeter_governance_scp](data_perimeter_governance_scp.json) – Include statements to secure tags that are used for authorization controls. This SCP also include statements that should be included in your data perimeter to account for specific data access patterns that are not covered by primary data perimeter controls. 

[service_specific_controls](service_specific_controls) subfolder contains policy examples you might want to use when implementing a data perimeter for a service.

Note that the SCP examples in this repository use a [deny list strategy](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps_strategies.html), which means that you also need a FullAWSAccess policy or other policy attached to your AWS Organizations organization entities to allow actions. You still also need to grant appropriate permissions to your principals by using identity-based or resource-based policies.

For your network perimeter, this folder has examples of policies for enforcing controls on specific service roles and IAM principals tagged with the `dp:include:network` tag set to `true`. See [Tagging conventions](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main?tab=readme-ov-file#tagging-conventions-and-governance) used in the policy examples to control the scope of data perimeter guardrails.

## Included data access patterns

The following policy statements are included in the SCP examples, each statement representing specific data access patterns.

### "Sid":"EnforceResourcePerimeterAWSResources"

This policy statement is included in the [resource_perimeter_scp](resource_perimeter_scp.json) and limits access to trusted resources that include [service_owned_resources](../service_owned_resources.md):

* Resources that belong to your Organizations organization specified by the organization ID (`<my-org-id>`) in the policy statement.
* Resources owned by AWS services. To permit access to service-owned resources through the resource perimeter, relevant resources are listed in the `NotResource` element of the policy. See the [service_owned_resources](../service_owned_resources.md) for a list of resources owned by AWS services.
* Trusted resources that belong to an account outside of your Organizations organization. To permit access to a resource owned by an external account through the resource perimeter, relevant service resources have to be listed in the `NotResource` element of this statement (`<third-party-resource>`). Access to these resources are further restricted in the `"Sid":"EnforceResourcePerimeterThirdPartyResources"`.

### "Sid":"EnforceResourcePerimeterAWSResourcesS3"

This policy statement is included in the [resource_perimeter_scp](resource_perimeter_scp.json) and limits access to trusted [Amazon Simple Storage Service (Amazon S3)](https://aws.amazon.com/s3/) resources:

* Amazon S3 resources that belong to your Organizations organization as specified by the organization ID (`<my-org-id>`) in the policy statement.
* Amazon S3 resources owned by AWS services that might be accessed by your identities and applications via AWS services using forward access sessions (FAS). To account for this access pattern, relevant S3 resources are first listed in the NotResource element of the `"Sid":"EnforceResourcePerimeterAWSResources"` statement. The `"Sid":"EnforceResourcePerimeterAWSResourcesS3"` then uses the aws:CalledVia condition key to restrict these access to relevant AWS services only. See the [service_owned_resources](../service_owned_resources.md) for a list of Amazon S3 resources owned by AWS services.

### "Sid":"EnforceResourcePerimeterThirdPartyResources"

This policy statement is included in the [resource_perimeter_scp](resource_perimeter_scp.json) and limits access to trusted resources that include third party resources:

* Resources that belong to your Organizations organization and are specified by the organization ID (`<my-org-id>`) in the policy statement.
* Trusted resources that belong to an account outside of your Organizations organization are specified by account IDs of third parties (`<third-party-account-a>` and `<third-party-account-b>`) in the policy statement. Further restrict access by specifying allowed resources in the Resource element of the policy statement. These resources also have to be listed in the `NotResource` element of `"Sid":"EnforceResourcePerimeterAWSResources"`.

### "Sid":"EnforceNetworkPerimeterVpceOrgID"

This policy statement is included in the [network_perimeter_vpceorgid_scp](network_perimeter_vpceorgid_scp.json). This policy statement limits access to expected networks with the aws:VpceOrgID condition key for [supported services](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-vpceorgid) and for IAM principals tagged with the `dp:include:network` tag set to `true`. If all of the services you use are supported by aws:VpceOrgID, you can disregard the `Sid":"EnforceNetworkPerimeterSourceVPC` and `"Sid":"SourceVPCRegion"`. Expected networks are defined as follows:

*   Your on-premises data centers and static egress points in AWS such as a NAT gateway that are specified by IP ranges (`<my-corporate-cidr>`) in the policy statement.
*   Your organization’s VPCs that are specified by the organization ID (`<my-org-id>`) in the policy statement.
*   Networks of AWS services that use [forward access sessions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_forward_access_sessions.html) to access resources on your behalf as denoted by `aws:ViaAWSService` in the policy statement. This access pattern applies when you access data via an AWS service, and that service takes subsequent actions on your behalf by using your IAM credentials.
*   Networks of AWS services when AWS services interact with [KMS](https://aws.amazon.com/kms/) encrypted AMIs, volumes, or snapshots as denoted by the `aws:PrincipalArn` condition key with a value of `arn:aws:iam:::role/aws:ec2-infrastructure`. 
*   Networks of trusted third parties are specified by their account IDs (`<third-party-account-a>` and `<third-party-account-b>`) in the policy statement. 

### "Sid":"EnforceNetworkPerimeterSourceVPC"

This policy statement is included in the [network_perimeter_sourcevpc_scp](network_perimeter_sourcevpc_scp.json). Use the `aws:SourceVpc` with `aws:RequestedRegion` for services not supported by [aws:VpceOrgID](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-vpceorgid). See [Establishing a data perimeter on AWS: Allow access to company data only from expected networks](https://aws.amazon.com/blogs/security/establishing-a-data-perimeter-on-aws-allow-access-to-company-data-only-from-expected-networks/) for more details. This policy statement limits access to expected networks for IAM principals tagged with the `dp:include:network` tag set to `true`. Expected networks are defined as follows:

* Your organization’s on-premises data centers and static egress points in AWS such as NAT gateways that are specified by IP ranges (<my-corporate-cidr>) in the policy statement.
* Your organization’s VPCs specified by VPC IDs (<my-vpc>) in the policy statement.
* Networks of AWS services that use [forward access sessions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_forward_access_sessions.html) to access resources on your behalf as denoted by `aws:ViaAWSService` in the policy statement. This access pattern applies when you access data via an AWS service and that service takes subsequent actions on your behalf by using your IAM identity credentials.
* Networks of AWS services when AWS services interact with [KMS](https://aws.amazon.com/kms/) encrypted AMIs, volumes, or snapshots as denoted by the `aws:PrincipalArn` condition key with a value of `arn:aws:iam:::role/aws:ec2-infrastructure`.

### "Sid":"SourceVPCRegion"

This policy statement is included in the [network_perimeter_sourcevpc_scp](network_perimeter_sourcevpc_scp.json) and limits access to expected networks for IAM principals tagged with the `dp:include:network` tag set to `true`. Expected networks are defined as follows:

* AWS VPC IDs are unique within a AWS Region, and same VPC ID can exist in different AWS Regions.[aws:RequestedRegion](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-requestedregion) is used with [aws:SourceVpc](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourcevpc) to limit access to expected networks with the `aws:SourceVpc` condition along with the region from which the request originates. Replace `<my-vpc-in-this-region>` with the VPC ID specified under `"Sid":"EnforceNetworkPerimeterSourceVPC"` and `<my-vpc-region>` with the VPC region.


#### Services and actions that require an exception to the network perimeter.
* Some AWS services have resources that are accessible from within your VPC through network interfaces or run inside your VPC, and use IAM for authentication. To account for this access pattern, you should list relevant actions in the `NotAction` element of this statement and use network security controls such as [security groups](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html), [access control lists](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html), and firewalls such as [AWS Network Firewall](https://aws.amazon.com/network-firewall/) to control the networks and IP addresses that can access these resources.
    * `dax:GetItem`, `dax:BatchGetItem`, `dax:Query`, `dax:Scan`, `dax:PutItem`, `dax:UpdateItem`, `dax:DeleteItem`, `dax:BatchWriteItem`, and `dax:ConditionCheckItem` – Required for [Amazon DynamoDB Accelerator (DAX)](https://aws.amazon.com/dynamodb/dax/) operations. At runtime, the DAX client directs all of your application's DynamoDB API requests to the DAX cluster, which runs in your VPC. Even though these requests originate from your VPC, they do not traverse a VPC endpoint. 
    * `neptune-db:*` – Required for [Amazon Neptune](https://aws.amazon.com/neptune/). Amazon Neptune databases are deployed in your VPC and are accessed over a network interface in the VPC. The `neptune-db` IAM namespace is only used to access the Neptune database in your VPCs with [IAM authentication](https://docs.aws.amazon.com/neptune/latest/userguide/iam-auth-connecting.html) and is not used with AWS APIs.
    * `elasticfilesystem:ClientMount`,`elasticfilesystem:RootAccess`,`elasticfilesystem:ClientWrite` – Required to use [Amazon Elastic File System (EFS)](https://aws.amazon.com/efs/) with [IAM authorization](https://docs.aws.amazon.com/efs/latest/ug/mounting-IAM-option.html). These IAM actions are only used to access Amazon EFS file systems from within your VPC via a network interface. To save space in the policy example, these three IAM actions are written with a wildcard character as `elasticfilesystem:Client*`.
    * `rds-db:Connect` – Required to use [Amazon Relational Database Service (RDS)](https://aws.amazon.com/rds/) with [IAM authentication](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.IAMDBAuth.html). Amazon RDS databases are deployed in your VPC and are accessed over a network interface in the VPC. The `rds-db` IAM namespace is only used for authentication to RDS databases. 
    * `kafka-cluster:*` – Required to use [Amazon Managed Streaming for Apache Kafka (MSK)](https://aws.amazon.com/msk/) with [IAM access control](https://docs.aws.amazon.com/msk/latest/developerguide/iam-access-control.html). The `kafka-cluster` IAM namespace is only used to access Amazon MSK clusters in your VPCs with IAM authentication.
    * `es:ESHttpGet`, `es:ESHttpPut`,`es:ESHttpDelete`,`es:ESHttpPost`,`es:ESHttpPatch`,`es:ESHttpHead`  – Required to use [Amazon OpenSearch Service](https://aws.amazon.com/opensearch-service/) with [IAM authentication for OpenSearch Domains](https://docs.aws.amazon.com/opensearch-service/latest/developerguide/ac.html#ac-types-resource). These IAM actions are only used to access OpenSearch domains. When an OpenSearch domain is deployed with "VPC Access" selected, requests to that OpenSearch domain traverse a network interface in your VPC and does not traverse a VPC endpoint. If you are using IAM authentication with an OpenSearch domain that is configured to be accessible in "public" mode over the Internet, you can use the `aws:SourceIp` condition key to help control from which networks the OpenSearch domain can be accessed. To save space in the policy example, these IAM actions are written with a wildcard character as `es:ES*`.
    * `s3-outposts:*` - Required for [Amazon S3 on Outposts](https://docs.aws.amazon.com/AmazonS3/latest/s3-outposts/S3OutpostsGS.html). S3 buckets on Outposts operate differently from other S3 buckets because they run on AWS Outposts infrastructure and are accessed through network interfaces in [your VPC or on-premises network](https://docs.aws.amazon.com/AmazonS3/latest/s3-outposts/S3OutpostsNetworking.html) and not over an AWS API endpoint, or VPC endpoint. Operations against S3 Outposts buckets or objects within them, such as GetObject do not support AWS global condition keys for network context, such as `aws:SourceIp` or `aws:SourceVPC`. While we exempt the entire `s3-outposts` IAM namespace in this example for simplicity, only operations against S3 Outposts buckets and objects in those buckets require this exemption.


#### Example data access patterns

* *Athena query*. When an application running in your VPC [creates an Athena query](https://docs.aws.amazon.com/athena/latest/ug/getting-started.html), Athena uses the application’s credentials to make subsequent requests to Amazon S3 to read data from your bucket and return results using [forward access sessions (FAS)](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_forward_access_sessions.html). 
* *Elastic Beanstalk operations*. When a user [creates an application by using Elastic Beanstalk](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/applications.html), Elastic Beanstalk launches an environment and uses your IAM credentials to create and configure other AWS resources such as Amazon EC2 instances. 
* *Amazon S3 server-side encryption with AWS KMS (SSE-KMS)*. When you upload an object to an Amazon S3 bucket with the default [SSE-KMS encryption](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingKMSEncryption.html) enabled, Amazon S3 makes a subsequent FAS request to AWS KMS in order to generate a data key from your CMK to encrypt the object. The call to AWS KMS is signed by using your credentials. A similar pattern is observed when a user tries to download an encrypted object when Amazon S3 calls AWS KMS to decrypt the key that was used to encrypt the object. Other services such as AWS Secrets Manager use a similar integration with AWS KMS.
* *AWS Data Exchange publishing and subscribing* (described in  `"Sid":"EnforceResourcePerimeterAWSResourcesS3"` earlier in this document).
* *AWS Service Catalog operations* (described in `"Sid":"EnforceResourcePerimeterAWSResourcesS3"` earlier in this document).
* *KMS Encrypted AMIs, Volumes, and Snapshots* When an EC2 instance attempts to interact with an AWS KMS encrypted AMI, volume, or snapshot, a KMS key grant is issued to the [instance's identity-only role](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-encryption-requirements.html#ebs-encryption-instance-permissions). The identity-only role is a special IAM role, `arn:aws:iam:::role/aws:ec2-infrastructure`, that is used by the instance to interact with encrypted AMIs, volumes, or snapshots on your behalf. This role is used to make requests to AWS KMS from AWS networks.

### "Sid":"PreventRAMExternalResourceShare"

This statement is included in the [data_perimeter_governance_scp](data_perimeter_governance_scp.json) and denies the creation of or updates to [AWS Resource Access Manager (AWS RAM)](https://aws.amazon.com/ram/) resource shares that allow sharing with external principals.

[Some AWS resources](https://docs.aws.amazon.com/ram/latest/userguide/shareable.html#shareable-r53.) allow cross-account sharing via AWS RAM instead of resource-based policies. By default, AWS RAM shares allow sharing outside of an Organizations organization. You can explicitly [restrict sharing of resources outside of AWS Organizations](https://docs.aws.amazon.com/ram/latest/userguide/working-with-sharing-create.html) and then limit AWS RAM actions based on this configuration.

### "Sid":"PreventExternalResourceShare"

This statement is included in the [data_perimeter_governance_scp](data_perimeter_governance_scp.json) and restricts resource sharing by capabilities that are embedded into services.

Some AWS services use neither resource-based policies nor AWS RAM.

Example data access patterns:

* [Amazon EC2 AMI](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/sharing-amis.html): You can share AMIs with other accounts or make them public with the `ModifyImageAttribute` and `ModifyFPGAImageAttribute` APIs.
* [Amazon EC2 network interface](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_CreateNetworkInterfacePermission.html): You can share EC2 network interfaces with other accounts with the `CreateNetworkInterfacePermission` API. 
* [Amazon EC2 elastic IP](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/transfer-EIPs-intro-ec2.html): You can transfer an Elastic IP address from one AWS account to another with the `EnableAddressTransfer` API.
* [Amazon EBS snapshots](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-modifying-snapshot-permissions.html): You can share Amazon EBS snapshots with other accounts, or you can make them public with the `ModifySnapshotAttribute` API.
* [VPC endpoint connections](https://docs.aws.amazon.com/vpc/latest/privatelink/configure-endpoint-service.html): You can grant permissions to another account to connect to your VPC endpoint service with the `ModifyVpcEndpointServicePermissions` API.
* [Systems Manager documents (SSM documents)](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-ssm-docs.html): You can share SSM documents with other accounts or make them public with the `ModifyDocumentPermission` API.
* [Amazon RDS snapshots](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ShareSnapshot.html): You can share RDS and RDS cluster snapshots with other accounts or make them public with the `ModifyDBSnapshotAttribute` and `ModifyDBClusterSnapshotAttribute` APIs.
* [Amazon Redshift datashare](https://docs.aws.amazon.com/redshift/latest/dg/authorize-datashare-console.html): You can authorize the sharing of a datashare with other accounts with the `AuthorizeDataShare` API. You can also share a snapshot with other accounts with `AuthorizeSnapshotAccess` API.
* [Amazon Redshift cluster](https://docs.aws.amazon.com/redshift/latest/APIReference/API_AuthorizeEndpointAccess.html): You can grant access to an Amazon Redshift cluster to other accounts with the `AuthorizeEndpointAccess` API. 
* [AWS Directory Service directory](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ms_ad_directory_sharing.html): You can share a directory with other accounts with the `ShareDirectory` API.
* [AWS Direct Connect gateway](https://docs.aws.amazon.com/directconnect/latest/UserGuide/multi-account-associate-vgw.html): You can associate a Direct Connect gateway with a virtual private gateway that is owned by another AWS account with the `CreateDirectConnectGatewayAssociationProposal` API.
* [Amazon Detective graph](https://docs.aws.amazon.com/detective/latest/userguide/accounts.html): A Detective administrator account can invite other accounts to join a behavior graph with the `CreateMembers` API.
* [AWS Glue Data Catalog](https://docs.aws.amazon.com/lake-formation/latest/dg/granting-catalog-perms-TBAC.html) databases: You can grant data catalog permissions to another account by using the AWS Lake Formation tag-based access control method with the `GrantPermissions` and `BatchGrantPermissions` APIs.
* [Amazon AppStream 2.0 image](https://docs.aws.amazon.com/appstream2/latest/developerguide/administer-images.html#share-image-with-another-account): You can share an Amazon AppStream 2.0 image that you own with other accounts with the `UpdateImagePermissions` API.
* [Amazon Macie member accounts](https://docs.aws.amazon.com/macie/latest/user/accounts-mgmt-invitations-administer.html): You can add member accounts to your Macie administrator account with the `CreateInvitations` API.
* [AWS Security Hub member accounts](https://docs.aws.amazon.com/securityhub/latest/userguide/account-management-manual.html): You can create and invite a member to your Security Hub administrator account with the `CreateMembers` and `InviteMembers` APIs.
* [Amazon GuardDuty member accounts](https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_invitations.html): You can create and invite a member to your GuardDuty administrator account with the `CreateMembers` and `InviteMembers` APIs.
* [AWS Audit Manager assessment framework shares](https://docs.aws.amazon.com/audit-manager/latest/userguide/share-custom-framework.html): You can create a share request for a custom framework in Audit Manager with the `StartAssessmentFrameworkShare` API.
* [Amazon DocumentDB cluster snapshots](https://docs.aws.amazon.com/documentdb/latest/developerguide/backup_restore-share_cluster_snapshots.html ): You can share an Amazon Document DB manual cluster snapshot with other accounts or make them public with the `ModifyDBClusterSnapshotAttribute` API. 
* [Amazon WorkSpaces image](https://docs.aws.amazon.com/workspaces/latest/adminguide/share-custom-image.html): You can share custom WorkSpaces images with other accounts with the `UpdateWorkspaceImagePermission` API.
* [Amazon CloudWatch sink](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Unified-Cross-Account.html): You can share observability data with other accounts with the `CreateLink` API.
* [AWS Service Catalog portfolio](https://docs.aws.amazon.com/servicecatalog/latest/adminguide/catalogs_portfolios_sharing.html): AWS Service Catalog portfolios can be shared with other AWS accounts with the `CreatePortfolioShare` and `UpdatePortfolioShare` APIs.
* [AWS Config aggregator](https://docs.aws.amazon.com/config/latest/developerguide/aggregate-data.html): The `PutConfigurationAggregator` API allows you to select another account to add to your AWS Config aggregator. Additionally, the `PutAggregationAuthorization` API allows you to authorize another account to collect data from your account.
* [AWS Fault Injection experiment template](https://docs.aws.amazon.com/fis/latest/userguide/multi-account.html): You create a multi-account experiment template by specifying other accounts with the `CreateTargetAccountConfiguration` API. 
* [AWS Global Accelerator attachment](https://docs.aws.amazon.com/global-accelerator/latest/dg/cross-account-resources.create-attachment.html): You can add a resource from another account as an endpoint for an accelerator with the `CreateCrossAccountAttachment` API.
* [AWS Cloud9 shared environment](https://docs.aws.amazon.com/cloud9/latest/user-guide/share-environment.html): You can share AWS Cloud9 development environment with users from other accounts with the `CreateEnvironmentMembership` API.
* [Amazon Connect dataset](https://docs.aws.amazon.com/connect/latest/APIReference/API_BatchAssociateAnalyticsDataSet.html#connect-BatchAssociateAnalyticsDataSet-request-DataSetIds): You can associate a list of Amazon Connect instance analytics datasets to a target account using `BatchAssociateAnalyticsDataSet` API.
* [Amazon Redshift Serverless snapshot](https://docs.aws.amazon.com/redshift-serverless/latest/APIReference/API_PutResourcePolicy.html): You can share snapshots across AWS accounts using `PutResourcePolicy` API.

### "Sid": “RestrictKMSGrantsCreationToAWSServices”
This statement is included in the [data_perimeter_governance_scp](data_perimeter_governance_scp.json) and restricts creation of [AWS KMS grants](https://docs.aws.amazon.com/kms/latest/developerguide/grants.html) to administrators and AWS services only.

The `CreateGrant` API allows you to add a grant for another account to use your KMS key. Use this statement to help ensure that only trusted identities can view information about your keys.

### "Sid":"ProtectActionsNotSupportedByPrimaryDPControls"

This statement is included in [data_perimeter_governance_scp](data_perimeter_governance_scp.json) and restricts actions that are not supported by primary data perimeter controls, such as those listed in the [ResourceOrgID condition key page](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-resourceorgid). 

Example data access patterns:

* [Transit gateway peering connections](https://docs.aws.amazon.com/vpc/latest/tgw/tgw-peering.html): You can create and manage a TGW peering connection with another account with the  `CreateTransitGatewayPeeringAttachment`,  `AcceptTransitGatewayPeeringAttachment`, `RejectTransitGatewayPeeringAttachment`, and `DeleteTransitGatewayPeeringAttachment` APIs. 
* [VPC peering connections](https://docs.aws.amazon.com/vpc/latest/peering/create-vpc-peering-connection.html): You can create and manage a VPC peering connection with another account with the `CreateVpcPeeringConnection`,  `AcceptVpcPeeringConnection`, `RejectVpcPeeringConnection`, and `DeleteVpcPeeringConnection` APIs.
* [VPC endpoint connections](https://docs.aws.amazon.com/vpc/latest/privatelink/configure-endpoint-service.html): You can create and manage an endpoint service connection with another account with the `CreateVpcEndpoint`, `AcceptVpcEndpointConnections`, and   `RejectVpcEndpointConnections` APIs. 
* [Amazon EBS snapshots](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-modifying-snapshot-permissions.html): You can copy a snapshot shared from a different account with the `CopySnapshot` API.
* [Amazon Route 53 private hosted zone](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/hosted-zone-private-associate-vpcs-different-accounts.html): You can associate and manage a VPC with a private hosted zone in a different account with the `CreateVPCAssociationAuthorization`,  `AssociateVPCWithHostedZone`, `DisassociateVPCFromHostedZone`, `ListHostedZonesByVPC`, and `DeleteVPCAssociationAuthorization`  APIs.
* [Amazon Macie member accounts](https://docs.aws.amazon.com/macie/latest/user/accounts-mgmt-invitations-administer.html): You can accept a Macie membership invitation that was received from a different account with the `AcceptInvitation` API.
* [AWS Security Hub member accounts](https://docs.aws.amazon.com/securityhub/latest/userguide/account-management-manual.html): You can accept a Security Hub membership invitation that was received from a different account with the `AcceptAdministratorInvitation` API.
* [Amazon GuardDuty member accounts](https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_invitations.html): You can accept a GuardDuty membership invitation that was received from a different account with the `AcceptAdministratorInvitation`API.
* [AWS Audit Manager assessment framework shares](https://docs.aws.amazon.com/audit-manager/latest/userguide/share-custom-framework.html): You can accept a share request for a custom framework from a different account with the `UpdateAssessmentFrameworkShare` API.
* [Amazon OpenSearch cross-cluster search connections](https://docs.aws.amazon.com/opensearch-service/latest/developerguide/cross-cluster-search.html): You can accept a cross-cluster search connection request from a different account with the `AcceptInboundConnection` API.
* [AWS Directory Service directory sharing](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ms_ad_directory_sharing.html): You can accept a directory sharing request that was sent from a different account with the `AcceptSharedDirectory` API.

You can also consider using service-specific condition keys such as `ec2:AccepterVpc` and `ec2:RequesterVpc` to restrict actions that are not supported by primary data perimeter controls (See [Work within a specific account](https://docs.aws.amazon.com/vpc/latest/peering/security-iam.html#vpc-peering-iam-account)).

### "Sid":"ProtectDataPerimeterTags"

This statement is included in the [data_perimeter_governance_scp](data_perimeter_governance_scp.json) and prevents the attaching, detaching, and modifying of tags used for authorization controls within the data perimeter.

### "Sid":"PreventLambdaFunctionURLAuthNone"

This statement is included in the [data_perimeter_governance_scp](data_perimeter_governance_scp.json) and denies the creation of Lambda functions that have `lambda:FunctionUrlAuthType` set to `NONE`.

[Lambda function URLs](https://docs.aws.amazon.com/lambda/latest/dg/lambda-urls.html) is a feature that lets you add HTTPS endpoints to your Lambda functions. When configuring a function URL for a new or existing function, you can set the `AuthType` parameter to `NONE`, which means Lambda won’t check for any IAM SigV4 signatures before invoking the function. If a function’s resource-based policy explicitly allows for public access, the function is open to unauthenticated requests.