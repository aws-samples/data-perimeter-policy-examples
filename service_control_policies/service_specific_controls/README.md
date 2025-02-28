# Examples of service-specific controls

Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved. SPDX-License-Identifier: CC-BY-SA-4.0

## Description

This folder contains examples of SCPs that enforce network perimeter controls targeting specific services. These examples do not represent a complete list of valid data access patterns, and they are intended for you to tailor and extend them to suit the needs of your environment. 

Policy examples in this folder enforce network perimeter controls on specific service roles and IAM users. Some AWS services use [service roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-role)  to perform tasks on your behalf. Some service roles are designed to be used by a service to directly call other services on your behalf as well as to make API calls from your code (for example, an [AWS Lambda](https://aws.amazon.com/lambda/)  function role is used to publish logs to [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/)  and to make calls to AWS APIs from the Lambda function code). Because these services allow code execution, it is possible for a user to obtain the credentials associated with a service role. Therefore, you may want to enforce the use of such credentials from expected networks only. You may also want to restrict the use of [IAM users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users.html) to expected networks only. We recommend using [IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) instead of IAM users with long-term access keys, as these access keys remain valid until manually revoked and therefore present a higher security risk. If your organization continues to use IAM users, implementing network perimeter controls can help mitigate potential security risks. 

Use the following example SCPs individually or in combination:

* [network_perimeter_ec2_policy](/service_control_policies/service_specific_controls/network_perimeter_ec2_policy.json) – Enforces network perimeter controls on service roles used by Amazon EC2 instances.
* [network_perimeter_iam_users_policy](/service_control_policies/service_specific_controls/network_perimeter_iam_users_policy.json) - Enforces network perimeter controls on IAM users with long-term access keys.
* [network_perimeter_lambda_policy](/service_control_policies/service_specific_controls/network_perimeter_lambda_policy.json) - Enforces network perimeter controls on service roles used by AWS Lambda.

Note that the SCP examples in this repository use a [deny list strategy](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps_strategies.html), which means that you also need a FullAWSAccess policy or other policy attached to your AWS Organizations organization entities to allow actions. You still also need to grant appropriate permissions to your principals by using identity-based or resource-based policies.

#### Services and actions that require an exception to the network perimeter.
* Some AWS services have resources that are accessible from within your VPC through network interfaces or run inside your VPC, and use IAM for authentication. To account for this access pattern, you should list relevant actions in the `NotAction` element of the statements in the examples and use network security controls such as [security groups](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html), [access control lists](https://docs.aws.amazon.com/AmazonS3/latest/userguide/acl-overview.html), and firewalls such as [AWS Network Firewall](https://aws.amazon.com/network-firewall/) to control the networks and IP addresses that can access these resources.
    * `dax:GetItem`, `dax:BatchGetItem`, `dax:Query`, `dax:Scan`, `dax:PutItem`, `dax:UpdateItem`, `dax:DeleteItem`, `dax:BatchWriteItem`, and `dax:ConditionCheckItem` – Required for [Amazon DynamoDB Accelerator (DAX)](https://aws.amazon.com/dynamodb/dax/) operations. At runtime, the DAX client directs all of your application's DynamoDB API requests to the DAX cluster, which runs in your VPC. Even though these requests originate from your VPC, they do not traverse a VPC endpoint. 
    * `neptune-db:*` – Required for [Amazon Neptune](https://aws.amazon.com/neptune/). Amazon Neptune databases are deployed in your VPC and are accessed over a network interface in the VPC. The `neptune-db` IAM namespace is only used to access the Neptune database in your VPCs with [IAM authentication](https://docs.aws.amazon.com/neptune/latest/userguide/iam-auth-connecting.html) and is not used with AWS APIs.
    * `elasticfilesystem:ClientMount`,`elasticfilesystem:RootAccess`,`elasticfilesystem:ClientWrite` – Required to use [Amazon Elastic File System (EFS)](https://aws.amazon.com/efs/) with [IAM authorization](https://docs.aws.amazon.com/efs/latest/ug/mounting-IAM-option.html). These IAM actions are only used to access Amazon EFS file systems from within your VPC via a network interface. To save space in the policy example, these three IAM actions are written with a wildcard character as `elasticfilesystem:Client*`.
    * `rds-db:Connect` – Required to use [Amazon Relational Database Service (RDS)](https://aws.amazon.com/rds/) with [IAM authentication](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.IAMDBAuth.html). Amazon RDS databases are deployed in your VPC and are accessed over a network interface in the VPC. The `rds-db` IAM namespace is only used for authentication to RDS databases. 
    * `kafka-cluster:*` – Required to use [Amazon Managed Streaming for Apache Kafka (MSK)](https://aws.amazon.com/msk/) with [IAM access control](https://docs.aws.amazon.com/msk/latest/developerguide/iam-access-control.html). The `kafka-cluster` IAM namespace is only used to access Amazon MSK clusters in your VPCs with IAM authentication.
    * `es:ESHttpGet`, `es:ESHttpPut`,`es:ESHttpDelete`,`es:ESHttpPost`,`es:ESHttpPatch`,`es:ESHttpHead`  – Required to use [Amazon OpenSearch Service](https://aws.amazon.com/opensearch-service/) with [IAM authentication for OpenSearch Domains](https://docs.aws.amazon.com/opensearch-service/latest/developerguide/ac.html#ac-types-resource). These IAM actions are only used to access OpenSearch domains. When an OpenSearch domain is deployed with "VPC Access" selected, requests to that OpenSearch domain traverse a network interface in your VPC and does not traverse a VPC endpoint. If you are using IAM authentication with an OpenSearch domain that is configured to be accessible in "public" mode over the Internet, you can use the `aws:SourceIp` condition key to help control from which networks the OpenSearch domain can be accessed. To save space in the policy example, these IAM actions are written with a wildcard character as `es:ES*`.

## Included data access patterns

The following policy statements are included in the SCP examples, each statement representing specific data access patterns.

### "Sid":"EnforceNetworkPerimeterOnEC2Roles"

This policy statement is included in the [network_perimeter_ec2_policy](network_perimeter_ec2_policy.json) and limits access to expected networks for service roles used by Amazon EC2 instance profiles. Expected networks are defined as follows:
* Your on-premises data centers and static egress points in AWS such as a NAT gateway that are specified by IP ranges (`<my-corporate-cidr>`) in the policy statement. 
* Your organization’s VPCs that are specified by VPC IDs (`<my-vpc>`) in the policy statement.  
* Networks of AWS services that use [forward access sessions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_forward_access_sessions.html) to access resources on your behalf as denoted by `aws:ViaAWSService` in the policy statement. This access pattern applies when you access data via an AWS service, and that service takes subsequent actions on your behalf by using your IAM credentials. 
* Networks of AWS services when AWS services interact with [KMS](https://aws.amazon.com/kms/) encrypted AMIs, volumes, or snapshots as denoted by the `aws:PrincipalArn` condition key with a value of `arn:aws:iam:::role/aws:ec2-infrastructure`. 

The `ec2:SourceInstanceARN` condition key is used to target role sessions that are created for applications running on your Amazon EC2 instances. 

### "Sid":" EnforceNetworkPerimeterOnIAMUsers "

This policy statement is included in the [network_perimeter_iam_users_policy](/service_control_policies/service_specific_controls/network_perimeter_iam_users_policy.json) and limits access to expected networks for IAM users. Expected networks are defined as follows:
*	Your on-premises data centers and static egress points in AWS such as a NAT gateway that are specified by IP ranges (<my-corporate-cidr>) in the policy statement.
*	Your organization’s VPCs that are specified by VPC IDs (<my-vpc>) in the policy statement.
*	Networks of AWS services that use [forward access sessions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_forward_access_sessions.html) to access resources on your behalf as denoted by `aws:ViaAWSService` in the policy statement. This access pattern applies when you access data via an AWS service, and that service takes subsequent actions on your behalf by using your IAM credentials.


### "Sid":"EnforceNetworkPerimeterOnLambdaRoles"

This policy statement is included in the [network_perimeter_lambda_policy](/service_control_policies/service_specific_controls/network_perimeter_lambda_policy.json) and limits access to expected networks for service roles used by AWS Lambda. Expected networks are defined as follows:
*   Your on-premises data centers and static egress points in AWS such as a NAT gateway that are specified by IP ranges (<my-corporate-cidr>) in the policy statement.
*   Your organization’s VPCs that are specified by VPC IDs (<my-vpc>) in the policy statement.
*   Networks of AWS services that use [forward access sessions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_forward_access_sessions.html) to access resources on your behalf as denoted by `aws:ViaAWSService` in the policy statement. This access pattern applies when you access data via an AWS service, and that service takes subsequent actions on your behalf by using your IAM credentials.
*   AWS Lambda networks when the service interacts with [CloudWatch Logs]( https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html), [AWS X-Ray](https://aws.amazon.com/xray/), and [Amazon EFS]( https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html), as denoted by the `NotAction` element with the actions `xray:PutTraceSegments`,`logs:CreateLogGroup`,`logs:CreateLogStream`,`logs:PutLogEvents`, `elasticfilesystem:ClientMount`.

The [` lambda:SourceFunctionArn `](https://docs.aws.amazon.com/lambda/latest/dg/permissions-source-function-arn.html) condition key is used to target role sessions that are created for your function's execution environment.
