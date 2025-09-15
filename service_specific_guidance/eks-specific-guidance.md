# Service-specific guidance: Amazon Elastic Kubernetes Service


This document outlines service-specific guidance for implementing a data perimeter for Amazon Elastic Kubernetes Service (Amazon EKS). 


Amazon EKS is a managed container orchestration service that simplifies the deployment, management, and scaling of containerized applications using Kubernetes. It provides a fully managed Kubernetes control plane, integrates seamlessly with other AWS services, and allows customers to run Kubernetes applications on AWS or on-premises with consistent operations.


The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type | Security objective | Applied on | Policy type | Additional considerations |
|----------------|-------------------|------------|-------------|------------------------|
| Identity perimeter | Only trusted identities can access my resources | Resource | RCP | N |
| Identity perimeter | Only trusted identities are allowed from my network | Network | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources | Identity | SCP | N |
| Resource perimeter | Only trusted resources can be accessed from my network | Network | VPC endpoint policy | N |
| Network perimeter | My identities can access resources only from expected networks | Identity | SCP | N |
| Network perimeter | My resources can be accessed only from expected networks | Resource | RCP | N |

*Y – Additional considerations apply. N – No additional considerations apply.
 



**List of service APIs reviewed against data perimeter control objectives**
* AssociateAccessPolicy
* AssociateEncryptionConfig
* AssociateIdentityProviderConfig
* CreateAccessEntry
* CreateAddon
* CreateCluster
* CreateFargateProfile
* CreateNodegroup
* CreatePodIdentityAssociation
* DeleteAccessEntry
* DeleteAddon
* DeleteCluster
* DeleteFargateProfile
* DeleteNodegroup
* DeletePodIdentityAssociation
* DeregisterCluster
* DescribeAccessEntry
* DescribeAddon
* DescribeAddonConfiguration
* DescribeAddonVersions
* DescribeCluster
* DescribeEksAnywhereSubscription
* DescribeFargateProfile
* DescribeIdentityProviderConfig
* DescribeInsight
* DescribeNodegroup
* DescribePodIdentityAssociation
* DescribeUpdate
* DisassociateAccessPolicy
* DisassociateIdentityProviderConfig
* ListAccessEntries
* ListAccessPolicies
* ListAddons
* ListAssociatedAccessPolicies
* ListClusters
* ListEksAnywhereSubscriptions
* ListFargateProfiles
* ListIdentityProviderConfigs
* ListInsights
* ListNodegroups
* ListPodIdentityAssociations
* ListTagsForResource
* ListUpdates
* RegisterCluster
* TagResource
* UntagResource
* UpdateAccessEntry
* UpdateAddon
* UpdateClusterConfig
* UpdateClusterVersion
* UpdateEksAnywhereSubscription
* UpdateNodegroupConfig
* UpdateNodegroupVersion
* UpdatePodIdentityAssociation
