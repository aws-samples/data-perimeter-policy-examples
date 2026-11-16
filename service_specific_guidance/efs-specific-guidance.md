# Service-specific guidance: Amazon Elastic File System


This document outlines service-specific guidance for implementing a data perimeter for Amazon Elastic File System.

Amazon Elastic File System (Amazon EFS) provides serverless, fully elastic file storage so that you can share file data without provisioning or managing storage capacity and performance. Amazon EFS supports the Network File System version 4 (NFSv4.1 and NFSv4.0) protocol and is accessible across most types of Amazon Web Services compute instances, including Amazon EC2, Amazon ECS, Amazon EKS, AWS Lambda, and AWS Fargate.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | N |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | N |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | N |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | Y |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | Y |

*Y - Additional considerations apply. N - No additional considerations apply.

## ClientMount
### IAM authentication in VPCs

**Perimeter type applicability**: network perimeter.

**Description**: [ClientMount](https://docs.aws.amazon.com/service-authorization/latest/reference/list_efs.html#list_efs-permission-only-actions) is used to access file systems in your VPCs with IAM authentication.

**Additional controls**:

If you want to restrict access to expected networks for your identities:
* Preventative control example: Consider implementing the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json), which lists [ClientMount](https://docs.aws.amazon.com/service-authorization/latest/reference/list_efs.html#list_efs-permission-only-actions) in the `NotAction` element. See [Services and actions that require an exception to the network perimeter](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies#services-and-actions-that-require-an-exception-to-the-network-perimeter) for other IAM actions that should be listed in the `NotAction` when enforcing network perimeter controls across a broad set of services.

If you want to restrict access to your resources to expected networks, consider implementing this additional control:
* Preventative control example: Consider implementing standard infrastructure security controls to restrict which networks and IP addresses can access file systems. These controls include [security groups](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html), [network access control lists](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html), and firewalls such as [AWS Network Firewall](https://aws.amazon.com/network-firewall/). Use the `elasticfilesystem:AccessedViaMountTarget` condition key to help ensure that file systems are accessed via mount targets so that infrastructure security controls remain effective.

## ClientRootAccess
### IAM authentication in VPCs

**Perimeter type applicability**: network perimeter.

**Description**: [ClientRootAccess](https://docs.aws.amazon.com/service-authorization/latest/reference/list_efs.html#list_efs-permission-only-actions) is used to access file systems in your VPCs with IAM authentication.

**Additional controls**:

If you want to restrict access to expected networks for your identities:
* Preventative control example: Consider implementing the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json), which lists [ClientRootAccess](https://docs.aws.amazon.com/service-authorization/latest/reference/list_efs.html#list_efs-permission-only-actions) in the `NotAction` element. See [Services and actions that require an exception to the network perimeter](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies#services-and-actions-that-require-an-exception-to-the-network-perimeter) for other IAM actions that should be listed in the `NotAction` when enforcing network perimeter controls across a broad set of services.

If you want to restrict access to your resources to expected networks, consider implementing this additional control:
* Preventative control example: Consider implementing standard infrastructure security controls to restrict which networks and IP addresses can access file systems. These controls include [security groups](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html), [network access control lists](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html), and firewalls such as [AWS Network Firewall](https://aws.amazon.com/network-firewall/). Use the `elasticfilesystem:AccessedViaMountTarget` condition key to help ensure that file systems are accessed via mount targets so that infrastructure security controls remain effective.

## ClientWrite
### IAM authentication in VPCs

**Perimeter type applicability**: network perimeter.

**Description**: [ClientWrite](https://docs.aws.amazon.com/service-authorization/latest/reference/list_efs.html#list_efs-permission-only-actions) is used to access file systems in your VPCs with IAM authentication.

**Additional controls**:

If you want to restrict access to expected networks for your identities:
* Preventative control example: Consider implementing the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json), which lists [ClientWrite](https://docs.aws.amazon.com/service-authorization/latest/reference/list_efs.html#list_efs-permission-only-actions) in the `NotAction` element. See [Services and actions that require an exception to the network perimeter](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies#services-and-actions-that-require-an-exception-to-the-network-perimeter) for other IAM actions that should be listed in the `NotAction` when enforcing network perimeter controls across a broad set of services.

If you want to restrict access to your resources to expected networks, consider implementing this additional control:
* Preventative control example: Consider implementing standard infrastructure security controls to restrict which networks and IP addresses can access file systems. These controls include [security groups](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html), [network access control lists](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html), and firewalls such as [AWS Network Firewall](https://aws.amazon.com/network-firewall/). Use the `elasticfilesystem:AccessedViaMountTarget` condition key to help ensure that file systems are accessed via mount targets so that infrastructure security controls remain effective.

## List of service APIs reviewed against data perimeter control objectives

The service APIs covered in this review are based on botocore version [1.43.29](https://github.com/boto/botocore/tree/1.43.29/botocore).

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
