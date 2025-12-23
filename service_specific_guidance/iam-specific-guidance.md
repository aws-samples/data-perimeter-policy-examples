# Service-specific guidance: AWS Identity and Access Management


This document outlines service-specific guidance for implementing a data perimeter for AWS Identity and Access Management. 


AWS Identity and Access Management (IAM) is a web service that helps you securely control access to AWS resources. It enables you to manage users, groups, and permissions, allowing you to specify who can access which services and resources, and under what conditions. IAM provides fine-grained access control and supports multi-factor authentication for enhanced security.


The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type | Security objective | Applied on | Policy type | Additional considerations |
|----------------|-------------------|------------|-------------|------------------------|
| Identity perimeter | Only trusted identities can access my resources | Resource | RCP | N |
| Identity perimeter | Only trusted identities are allowed from my network | Network | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources | Identity | SCP | Y |
| Resource perimeter | Only trusted resources can be accessed from my network | Network | VPC endpoint policy | Y |
| Network perimeter | My identities can access resources only from expected networks | Identity | SCP | N |
| Network perimeter | My resources can be accessed only from expected networks | Resource | RCP | N |

*Y – Additional considerations apply. N – No additional considerations apply.
 


**Additional consideration 1**

Perimeter type applicability: resource perimeter.
        
GenerateServiceLastAccessedDetails might require access to service-owned policies, which are policies that are managed in service accounts.

See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access. 
If you want to restrict access to trusted resources, see [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json) for information about how the default resource perimeter SCP accounts for such an access pattern. Use the service-specific policy, [iam_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/iam_endpoint_policy.json) to enforce the resource perimeter on your network.


**Additional consideration 2**

Perimeter type applicability: resource perimeter.
        
GetPolicy might require access to service-owned policies, which are policies that are managed in service accounts.

See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access. 
If you want to restrict access to trusted resources, see [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json) for information about how the default resource perimeter SCP accounts for such an access pattern. Use the service-specific policy, [iam_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/iam_endpoint_policy.json) to enforce the resource perimeter on your network.


**Additional consideration 3**

Perimeter type applicability: resource perimeter.
        
ListPolicyVersions might require access to service-owned policies, which are policies that are managed in service accounts.

See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access. 
If you want to restrict access to trusted resources, see [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json) for information about how the default resource perimeter SCP accounts for such an access pattern. Use the service-specific policy, [iam_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/iam_endpoint_policy.json) to enforce the resource perimeter on your network.


**Additional consideration 4**

Perimeter type applicability: resource perimeter.
        
ListEntitiesForPolicy might require access to service-owned policies, which are policies that are managed in service accounts.

See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access. 
If you want to restrict access to trusted resources, see [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json) for information about how the default resource perimeter SCP accounts for such an access pattern. Use the service-specific policy, [iam_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/iam_endpoint_policy.json) to enforce the resource perimeter on your network.


**Additional consideration 5**

Perimeter type applicability: resource perimeter.
        
GetPolicyVersion might require access to service-owned policies, which are policies that are managed in service accounts.

See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access. 
If you want to restrict access to trusted resources, see [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json) for information about how the default resource perimeter SCP accounts for such an access pattern. Use the service-specific policy, [iam_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/iam_endpoint_policy.json) to enforce the resource perimeter on your network.





**List of service APIs reviewed against data perimeter control objectives**
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
