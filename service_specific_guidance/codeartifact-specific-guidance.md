# Service-specific guidance: AWS CodeArtifact


This document outlines service-specific guidance for implementing a data perimeter for AWS CodeArtifact.

AWS CodeArtifact is a secure, highly scalable, managed artifact repository service that helps organizations to store and share software packages for application development. It works with popular build tools and package managers such as the NuGet CLI, Maven, Gradle, npm, yarn, pip, and twine.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | N |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | N |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | N |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | N |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | N |

*Y - Additional considerations apply. N - No additional considerations apply.

## List of service APIs reviewed against data perimeter control objectives

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
