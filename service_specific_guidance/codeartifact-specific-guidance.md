
# Service-specific guidance: AWS CodeArtifact


This document outlines service-specific guidance for implementing a data perimeter for AWS CodeArtifact. 

AWS CodeArtifact is a fully managed artifact repository service that makes it easy for organizations to securely store, publish, and share software packages used in their software development process. It integrates with commonly used build tools and package managers, allowing developers to easily retrieve dependencies while maintaining control over package access and versioning.


The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type | Security objective | Applied on | Policy type | Additional considerations |
|----------------|-------------------|------------|-------------|------------------------|
| Identity perimeter | Only trusted identities can access my resources | Resource | RCP | Y |
| Identity perimeter | Only trusted identities are allowed from my network | Network | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources | Identity | SCP | N |
| Resource perimeter | Only trusted resources can be accessed from my network | Network | VPC endpoint policy | N |
| Network perimeter | My identities can access resources only from expected networks | Identity | SCP | N |
| Network perimeter | My resources can be accessed only from expected networks | Resource | RCP | Y |

*Y – Additional considerations apply. N – No additional considerations apply.
 



**Additional consideration 1**

Perimeter type applicability: identity and network perimeter applied on resource.
        
PutRepositoryPermissionsPolicy allows you to apply a resource-based policy to grant access to a repository. The service currently doesn’t support RCPs.


If you want to restrict access to trusted identities and expected networks, consider implementing these additional controls:

* **Preventative control example**: Consider restricting [PutRepositoryPermissionsPolicy](https://docs.aws.amazon.com/codeartifact/latest/APIReference/API_PutRepositoryPermissionsPolicy.html) permissions to administrators only using an SCP. See [restrict_resource_policy_configurations_scp.json](../service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [PermissionsPolicyDocument](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-codeartifact-repository.html#cfn-codeartifact-repository-permissionspolicydocument) property for the [AWS::CodeArtifact::Repository](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-codeartifact-repository.html) resource that grants permissions to untrusted identities or unexpected networks. 
* **Detective control example:** Consider using CloudTrail management events to monitor the [PutRepositoryPermissionsPolicy](https://docs.aws.amazon.com/codeartifact/latest/APIReference/API_PutRepositoryPermissionsPolicy.html) API calls in your environment (specifically, the [policyDocument](https://docs.aws.amazon.com/codeartifact/latest/APIReference/API_PutRepositoryPermissionsPolicy.html#codeartifact-PutRepositoryPermissionsPolicy-request-policyDocument) request parameter). If necessary, remediate with the responsive controls of your choice.



**Additional consideration 2**

Perimeter type applicability: identity and network perimeter applied on resource.
        
PutDomainPermissionsPolicy allows you to apply a resource-based policy to grant access to a domain. The service currently doesn’t support RCPs.


If you want to restrict access to trusted identities and expected networks, consider implementing these additional controls:

* **Preventative control example**: Consider restricting [PutDomainPermissionsPolicy](https://docs.aws.amazon.com/codeartifact/latest/APIReference/API_PutDomainPermissionsPolicy.html) permissions to administrators only using an SCP. See [restrict_resource_policy_configurations_scp.json](../service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [PermissionsPolicyDocument](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-codeartifact-domain.html#cfn-codeartifact-domain-permissionspolicydocument) property for the [AWS::CodeArtifact::Domain](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-codeartifact-domain.html) resource that grants permissions to untrusted identities or unexpected networks. 
* **Detective control example:** Consider using CloudTrail management events to monitor the [PutDomainPermissionsPolicy](https://docs.aws.amazon.com/codeartifact/latest/APIReference/API_PutDomainPermissionsPolicy.html) API calls in your environment (specifically, the [policyDocument](https://docs.aws.amazon.com/codeartifact/latest/APIReference/API_PutDomainPermissionsPolicy.html#codeartifact-PutDomainPermissionsPolicy-request-policyDocument) request parameter). If necessary, remediate with the responsive controls of your choice. 


**List of service APIs reviewed against data perimeter control objectives**

* AssociateExternalConnection
* CopyPackageVersions
* CreateDomain
* CreatePackageGroup
* CreateRepository
* DeleteDomain
* DeleteDomainPermissionsPolicy
* DeletePackage
* DeletePackageGroup
* DeletePackageVersions
* DeleteRepository
* DeleteRepositoryPermissionsPolicy
* DescribeDomain
* DescribePackage
* DescribePackageGroup
* DescribePackageVersion
* DescribeRepository
* DisassociateExternalConnection
* DisposePackageVersions
* GetAssociatedPackageGroup
* GetAuthorizationToken
* GetDomainPermissionsPolicy
* GetPackageVersionAsset
* GetRepositoryEndpoint
* GetRepositoryPermissionsPolicy
* ListAssociatedPackages
* ListDomains
* ListPackageGroups
* ListPackageVersionAssets
* ListPackageVersionDependencies
* ListPackageVersions
* ListPackages
* ListRepositories
* ListRepositoriesInDomain
* ListSubPackageGroups
* ListTagsForResource
* PublishPackageVersion
* PutDomainPermissionsPolicy
* PutPackageOriginConfiguration
* PutRepositoryPermissionsPolicy
* TagResource
* UntagResource
* UpdatePackageGroup
* UpdatePackageGroupOriginConfiguration
* UpdatePackageVersionsStatus
* UpdateRepository
