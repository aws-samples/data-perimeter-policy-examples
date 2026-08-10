# Service-specific guidance: Amazon Elastic File System


This document outlines service-specific guidance for implementing a data perimeter for Amazon Elastic File System.

Amazon Elastic File System (Amazon EFS) provides serverless, fully elastic file storage so that you can share file data without provisioning or managing storage capacity and performance. Amazon EFS supports the Network File System version 4 (NFSv4.1 and NFSv4.0) protocol and is accessible across most types of Amazon Web Services compute instances, including Amazon EC2, Amazon ECS, Amazon EKS, AWS Lambda, and AWS Fargate.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | Y |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | N |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | N |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | Y |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | Y |

*Y - Additional considerations apply. N - No additional considerations apply.

## ClientMount
### IAM authentication in VPCs

**Perimeter type applicability**: network perimeter.

**Description**: [ClientMount](https://docs.aws.amazon.com/service-authorization/latest/reference/list_efs.html#list_efs-actions-as-permissions) is used to access file systems in your VPCs with IAM authentication.

**Additional controls**:

If you want to restrict access to expected networks for your identities:
* Preventative control example: Consider implementing the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json), which lists [ClientMount](https://docs.aws.amazon.com/service-authorization/latest/reference/list_efs.html#list_efs-actions-as-permissions) in the `NotAction` element. See [Services and actions that require an exception to the network perimeter](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies#services-and-actions-that-require-an-exception-to-the-network-perimeter) for other IAM actions that should be listed in the `NotAction` when enforcing network perimeter controls across a broad set of services.

If you want to restrict access to your resources to expected networks, consider implementing this additional control:
* Preventative control example: Consider implementing standard infrastructure security controls to restrict which networks and IP addresses can access file systems. These controls include [security groups](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html), [network access control lists](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html), and firewalls such as [AWS Network Firewall](https://aws.amazon.com/network-firewall/). Use the `elasticfilesystem:AccessedViaMountTarget` condition key to help ensure that file systems are accessed via mount targets so that infrastructure security controls remain effective.

## ClientRootAccess
### IAM authentication in VPCs

**Perimeter type applicability**: network perimeter.

**Description**: [ClientRootAccess](https://docs.aws.amazon.com/service-authorization/latest/reference/list_efs.html#list_efs-actions-as-permissions) is used to access file systems in your VPCs with IAM authentication.

**Additional controls**:

If you want to restrict access to expected networks for your identities:
* Preventative control example: Consider implementing the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json), which lists [ClientRootAccess](https://docs.aws.amazon.com/service-authorization/latest/reference/list_efs.html#list_efs-actions-as-permissions) in the `NotAction` element. See [Services and actions that require an exception to the network perimeter](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies#services-and-actions-that-require-an-exception-to-the-network-perimeter) for other IAM actions that should be listed in the `NotAction` when enforcing network perimeter controls across a broad set of services.

If you want to restrict access to your resources to expected networks, consider implementing this additional control:
* Preventative control example: Consider implementing standard infrastructure security controls to restrict which networks and IP addresses can access file systems. These controls include [security groups](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html), [network access control lists](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html), and firewalls such as [AWS Network Firewall](https://aws.amazon.com/network-firewall/). Use the `elasticfilesystem:AccessedViaMountTarget` condition key to help ensure that file systems are accessed via mount targets so that infrastructure security controls remain effective.

## ClientWrite
### IAM authentication in VPCs

**Perimeter type applicability**: network perimeter.

**Description**: [ClientWrite](https://docs.aws.amazon.com/service-authorization/latest/reference/list_efs.html#list_efs-actions-as-permissions) is used to access file systems in your VPCs with IAM authentication.

**Additional controls**:

If you want to restrict access to expected networks for your identities:
* Preventative control example: Consider implementing the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json), which lists [ClientWrite](https://docs.aws.amazon.com/service-authorization/latest/reference/list_efs.html#list_efs-actions-as-permissions) in the `NotAction` element. See [Services and actions that require an exception to the network perimeter](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies#services-and-actions-that-require-an-exception-to-the-network-perimeter) for other IAM actions that should be listed in the `NotAction` when enforcing network perimeter controls across a broad set of services.

If you want to restrict access to your resources to expected networks, consider implementing this additional control:
* Preventative control example: Consider implementing standard infrastructure security controls to restrict which networks and IP addresses can access file systems. These controls include [security groups](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html), [network access control lists](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html), and firewalls such as [AWS Network Firewall](https://aws.amazon.com/network-firewall/). Use the `elasticfilesystem:AccessedViaMountTarget` condition key to help ensure that file systems are accessed via mount targets so that infrastructure security controls remain effective.

## PutFileSystemPolicy
### No RCP support

**Perimeter type applicability**: identity and network perimeter applied on resource.

**Description**: [PutFileSystemPolicy](https://docs.aws.amazon.com/efs/latest/APIReference/API_PutFileSystemPolicy.html) allows you to apply a resource-based policy to grant access to a file system. The service currently does not support RCPs.

**Additional controls**:

If you want to restrict access to trusted identities, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutFileSystemPolicy](https://docs.aws.amazon.com/efs/latest/APIReference/API_PutFileSystemPolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [FileSystemPolicy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-efs-filesystem.html#cfn-efs-filesystem-filesystempolicy) property for the [AWS::EFS::FileSystem](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-efs-filesystem.html) resource that grants permissions to untrusted identities.
* Detective control example: Consider using [AWS Identity and Access Management Access Analyzer](https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html) external access analyzers to help identify file systems in your accounts that are shared with untrusted identities. If necessary, remediate with the responsive controls of your choice.
* Detective control example: Consider using CloudTrail management events to monitor the [PutFileSystemPolicy](https://docs.aws.amazon.com/efs/latest/APIReference/API_PutFileSystemPolicy.html) API calls in your environment (specifically, the [Policy](https://docs.aws.amazon.com/efs/latest/APIReference/API_PutFileSystemPolicy.html#API_PutFileSystemPolicy_RequestSyntax) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutFileSystemPolicy](https://docs.aws.amazon.com/efs/latest/APIReference/API_PutFileSystemPolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [FileSystemPolicy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-efs-filesystem.html#cfn-efs-filesystem-filesystempolicy) property for the [AWS::EFS::FileSystem](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-efs-filesystem.html) resource that grants permissions to unexpected networks.
* Detective control example: Consider using CloudTrail management events to monitor the [PutFileSystemPolicy](https://docs.aws.amazon.com/efs/latest/APIReference/API_PutFileSystemPolicy.html) API calls in your environment (specifically, the [Policy](https://docs.aws.amazon.com/efs/latest/APIReference/API_PutFileSystemPolicy.html#API_PutFileSystemPolicy_RequestSyntax) request parameter). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

* CreateAccessPoint
* CreateFileSystem
* CreateMountTarget
* CreateReplicationConfiguration
* CreateTags
* DeleteAccessPoint
* DeleteFileSystem
* DeleteFileSystemPolicy
* DeleteMountTarget
* DeleteReplicationConfiguration
* DeleteTags
* DescribeAccessPoints
* DescribeAccountPreferences
* DescribeBackupPolicy
* DescribeFileSystemPolicy
* DescribeFileSystems
* DescribeLifecycleConfiguration
* DescribeMountTargetSecurityGroups
* DescribeMountTargets
* DescribeReplicationConfigurations
* DescribeTags
* ListTagsForResource
* ModifyMountTargetSecurityGroups
* PutAccountPreferences
* PutBackupPolicy
* PutFileSystemPolicy
* PutLifecycleConfiguration
* TagResource
* UntagResource
* UpdateFileSystem
* UpdateFileSystemProtection
