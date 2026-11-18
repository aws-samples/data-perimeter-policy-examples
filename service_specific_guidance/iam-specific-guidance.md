# Service-specific guidance: AWS Identity and Access Management


This document outlines service-specific guidance for implementing a data perimeter for AWS Identity and Access Management.

AWS Identity and Access Management (IAM) is a web service that helps you securely control access to AWS resources. With IAM, you can manage permissions that control which AWS resources users can access.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | N |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | Y |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | Y |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | Y |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | Y |

*Y - Additional considerations apply. N - No additional considerations apply.

## GenerateServiceLastAccessedDetails
### Access to service-owned resources

**Perimeter type applicability**: resource perimeter.

**Description**: [GenerateServiceLastAccessedDetails](https://docs.aws.amazon.com/IAM/latest/APIReference/API_GenerateServiceLastAccessedDetails.html) might require access to service-owned policies, which are policies that are managed in service accounts.

**Additional controls**:

If you want to restrict access to trusted resources for your identities, consider implementing this control:
* Preventative control example: Consider implementing the default resource perimeter SCP, [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json), which lists service-owned policies in the `NotResource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access.

If you want to restrict access to your networks to trusted resources, consider implementing this control:
* Preventative control example: Consider implementing the service-specific VPC endpoint policy, [iam_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/iam_endpoint_policy.json), which lists service-owned policies in the `Resource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that might be accessed from your networks.

## GetPolicy
### Access to service-owned resources

**Perimeter type applicability**: resource perimeter.

**Description**: [GetPolicy](https://docs.aws.amazon.com/IAM/latest/APIReference/API_GetPolicy.html) might require access to service-owned policies, which are policies that are managed in service accounts.

**Additional controls**:

If you want to restrict access to trusted resources for your identities, consider implementing this control:
* Preventative control example: Consider implementing the default resource perimeter SCP, [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json), which lists service-owned policies in the `NotResource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access.

If you want to restrict access to your networks to trusted resources, consider implementing this control:
* Preventative control example: Consider implementing the service-specific VPC endpoint policy, [iam_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/iam_endpoint_policy.json), which lists service-owned policies in the `Resource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that might be accessed from your networks.

## GetPolicyVersion
### Access to service-owned resources

**Perimeter type applicability**: resource perimeter.

**Description**: [GetPolicyVersion](https://docs.aws.amazon.com/IAM/latest/APIReference/API_GetPolicyVersion.html) might require access to service-owned policies, which are policies that are managed in service accounts.

**Additional controls**:

If you want to restrict access to trusted resources for your identities, consider implementing this control:
* Preventative control example: Consider implementing the default resource perimeter SCP, [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json), which lists service-owned policies in the `NotResource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access.

If you want to restrict access to your networks to trusted resources, consider implementing this control:
* Preventative control example: Consider implementing the service-specific VPC endpoint policy, [iam_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/iam_endpoint_policy.json), which lists service-owned policies in the `Resource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that might be accessed from your networks.

## ListEntitiesForPolicy
### Access to service-owned resources

**Perimeter type applicability**: resource perimeter.

**Description**: [ListEntitiesForPolicy](https://docs.aws.amazon.com/IAM/latest/APIReference/API_ListEntitiesForPolicy.html) might require access to service-owned policies, which are policies that are managed in service accounts.

**Additional controls**:

If you want to restrict access to trusted resources for your identities, consider implementing this control:
* Preventative control example: Consider implementing the default resource perimeter SCP, [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json), which lists service-owned policies in the `NotResource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access.

If you want to restrict access to your networks to trusted resources, consider implementing this control:
* Preventative control example: Consider implementing the service-specific VPC endpoint policy, [iam_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/iam_endpoint_policy.json), which lists service-owned policies in the `Resource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that might be accessed from your networks.

## ListPolicyVersions
### Access to service-owned resources

**Perimeter type applicability**: resource perimeter.

**Description**: [ListPolicyVersions](https://docs.aws.amazon.com/IAM/latest/APIReference/API_ListPolicyVersions.html) might require access to service-owned policies, which are policies that are managed in service accounts.

**Additional controls**:

If you want to restrict access to trusted resources for your identities, consider implementing this control:
* Preventative control example: Consider implementing the default resource perimeter SCP, [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json), which lists service-owned policies in the `NotResource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access.

If you want to restrict access to your networks to trusted resources, consider implementing this control:
* Preventative control example: Consider implementing the service-specific VPC endpoint policy, [iam_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/iam_endpoint_policy.json), which lists service-owned policies in the `Resource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that might be accessed from your networks.

## PassRole
### IAM authentication in VPCs

**Perimeter type applicability**: network perimeter.

**Description**: [PassRole](https://docs.aws.amazon.com/service-authorization/latest/reference/list_iam.html#list_iam-permission-only-actions) is used to support accessing resources in your VPCs with IAM authentication.

**Additional controls**:

If you want to restrict access to expected networks for your identities:
* Preventative control example: Consider implementing the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json), which lists [PassRole](https://docs.aws.amazon.com/service-authorization/latest/reference/list_iam.html#list_iam-permission-only-actions) in the `NotAction` element. See [Services and actions that require an exception to the network perimeter](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies#services-and-actions-that-require-an-exception-to-the-network-perimeter) for other IAM actions that should be listed in the `NotAction` when enforcing network perimeter controls across a broad set of services.

If you want to restrict access to your resources to expected networks, consider implementing this additional control:
* Preventative control example: Consider implementing standard infrastructure security controls to restrict which networks and IP addresses can access resources in your VPCs, such as Amazon DynamoDB Accelerator (DAX) clusters, Amazon EFS file systems, Amazon MSK clusters, Amazon OpenSearch Service domains, and Amazon RDS databases. These controls include [security groups](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html), [network access control lists](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html), and firewalls such as [AWS Network Firewall](https://aws.amazon.com/network-firewall/).

## List of service APIs reviewed against data perimeter control objectives

The service APIs covered in this review are based on botocore version [1.43.29](https://github.com/boto/botocore/tree/1.43.29/botocore).

* AddClientIDToOpenIDConnectProvider
* AddRoleToInstanceProfile
* AddUserToGroup
* AttachGroupPolicy
* AttachRolePolicy
* AttachUserPolicy
* CreateAccessKey
* CreateGroup
* CreateInstanceProfile
* CreateLoginProfile
* CreateOpenIDConnectProvider
* CreatePolicy
* CreatePolicyVersion
* CreateRole
* CreateSAMLProvider
* CreateServiceSpecificCredential
* CreateUser
* CreateVirtualMFADevice
* DeleteAccessKey
* DeleteAccountPasswordPolicy
* DeleteGroup
* DeleteGroupPolicy
* DeleteInstanceProfile
* DeleteLoginProfile
* DeleteOpenIDConnectProvider
* DeletePolicy
* DeletePolicyVersion
* DeleteRole
* DeleteRolePermissionsBoundary
* DeleteRolePolicy
* DeleteSAMLProvider
* DeleteSSHPublicKey
* DeleteServerCertificate
* DeleteServiceSpecificCredential
* DeleteSigningCertificate
* DeleteUser
* DeleteUserPermissionsBoundary
* DeleteUserPolicy
* DeleteVirtualMFADevice
* DetachGroupPolicy
* DetachRolePolicy
* DetachUserPolicy
* DisableOrganizationsRootCredentialsManagement
* DisableOrganizationsRootSessions
* DisableOutboundWebIdentityFederation
* EnableOutboundWebIdentityFederation
* GenerateCredentialReport
* GenerateServiceLastAccessedDetails
* GetAccessKeyLastUsed
* GetAccountAuthorizationDetails
* GetAccountPasswordPolicy
* GetAccountSummary
* GetContextKeysForCustomPolicy
* GetContextKeysForPrincipalPolicy
* GetCredentialReport
* GetGroup
* GetGroupPolicy
* GetInstanceProfile
* GetLoginProfile
* GetOpenIDConnectProvider
* GetOutboundWebIdentityFederationInfo
* GetPolicy
* GetPolicyVersion
* GetRole
* GetRolePolicy
* GetSAMLProvider
* GetSSHPublicKey
* GetServerCertificate
* GetServiceLastAccessedDetails
* GetServiceLastAccessedDetailsWithEntities
* GetUser
* GetUserPolicy
* ListAccessKeys
* ListAccountAliases
* ListAttachedGroupPolicies
* ListAttachedRolePolicies
* ListAttachedUserPolicies
* ListDelegationRequests
* ListEntitiesForPolicy
* ListGroupPolicies
* ListGroups
* ListGroupsForUser
* ListInstanceProfileTags
* ListInstanceProfiles
* ListInstanceProfilesForRole
* ListMFADeviceTags
* ListMFADevices
* ListOpenIDConnectProviderTags
* ListOpenIDConnectProviders
* ListOrganizationsFeatures
* ListPolicies
* ListPoliciesGrantingServiceAccess
* ListPolicyTags
* ListPolicyVersions
* ListRolePolicies
* ListRoleTags
* ListRoles
* ListSAMLProviderTags
* ListSAMLProviders
* ListSSHPublicKeys
* ListServerCertificateTags
* ListServerCertificates
* ListServiceSpecificCredentials
* ListSigningCertificates
* ListUserPolicies
* ListUserTags
* ListUsers
* ListVirtualMFADevices
* PutGroupPolicy
* PutRolePermissionsBoundary
* PutRolePolicy
* PutUserPermissionsBoundary
* PutUserPolicy
* RemoveClientIDFromOpenIDConnectProvider
* RemoveRoleFromInstanceProfile
* RemoveUserFromGroup
* ResetServiceSpecificCredential
* SetDefaultPolicyVersion
* SetSecurityTokenServicePreferences
* SimulateCustomPolicy
* SimulatePrincipalPolicy
* TagInstanceProfile
* TagMFADevice
* TagOpenIDConnectProvider
* TagPolicy
* TagRole
* TagSAMLProvider
* TagServerCertificate
* TagUser
* UntagInstanceProfile
* UntagMFADevice
* UntagOpenIDConnectProvider
* UntagPolicy
* UntagRole
* UntagSAMLProvider
* UntagUser
* UpdateAccessKey
* UpdateAccountPasswordPolicy
* UpdateAssumeRolePolicy
* UpdateGroup
* UpdateLoginProfile
* UpdateOpenIDConnectProviderThumbprint
* UpdateRole
* UpdateRoleDescription
* UpdateSAMLProvider
* UpdateSSHPublicKey
* UpdateServerCertificate
* UpdateServiceSpecificCredential
* UpdateSigningCertificate
* UpdateUser
* UploadSSHPublicKey
* UploadServerCertificate
* UploadSigningCertificate
