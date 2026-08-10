# Service-specific guidance: Amazon Elastic Kubernetes Service


This document outlines service-specific guidance for implementing a data perimeter for Amazon Elastic Kubernetes Service.

Amazon EKS is a managed container orchestration service that simplifies the deployment, management, and scaling of containerized applications using Kubernetes. It provides a fully managed Kubernetes control plane, integrates seamlessly with other AWS services, and allows customers to run Kubernetes applications on AWS or on-premises with consistent operations.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | N |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | N |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | N |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | Y |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | N |

*Y - Additional considerations apply. N - No additional considerations apply.

## CreateFargateProfile
### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [CreateFargateProfile](https://docs.aws.amazon.com/eks/latest/APIReference/API_CreateFargateProfile.html) allows you to specify a service role, referred to as the Pod execution role, for your Pod operations. The service uses the role to make AWS API calls from your code.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json) or [network_perimeter_vpceorgid_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_vpceorgid_scp.json). These policies demonstrate how to use the `dp:include:network` tag set to `true` to restrict calls made by the Pod execution role. If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

## CreateNodegroup
### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [CreateNodegroup](https://docs.aws.amazon.com/eks/latest/APIReference/API_CreateNodegroup.html) allows you to specify a service role, referred to as the node IAM role, for your node operations. The service uses the role to make AWS API calls from your code.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json) or [network_perimeter_vpceorgid_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_vpceorgid_scp.json). These policies demonstrate how to use the `dp:include:network` tag set to `true` to restrict calls made by the node IAM role. If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

## CreatePodIdentityAssociation
### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [CreatePodIdentityAssociation](https://docs.aws.amazon.com/eks/latest/APIReference/API_CreatePodIdentityAssociation.html) allows you to specify a service role, referred to as the Pod Identity role, for your Pod operations. The service uses the role to make AWS API calls from your code.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json) or [network_perimeter_vpceorgid_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_vpceorgid_scp.json). These policies demonstrate how to use the `dp:include:network` tag set to `true` to restrict calls made by the Pod Identity IAM role. If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

## UpdatePodIdentityAssociation
### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [UpdatePodIdentityAssociation](https://docs.aws.amazon.com/eks/latest/APIReference/API_UpdatePodIdentityAssociation.html) allows you to specify a service role, referred to as the EKS Pod Identity role, for your Pod operations. The service uses the role to make AWS API calls from your code.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json) or [network_perimeter_vpceorgid_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_vpceorgid_scp.json). These policies demonstrate how to use the `dp:include:network` tag set to `true` to restrict calls made by the EKS Pod Identity role. If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

## List of service APIs reviewed against data perimeter control objectives

* AssociateAccessPolicy
* CreateAccessEntry
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
* DescribeClusterVersions
* DescribeEksAnywhereSubscription
* DescribeFargateProfile
* DescribeInsight
* DescribeInsightsRefresh
* DescribeNodegroup
* DescribePodIdentityAssociation
* DescribeUpdate
* DisassociateAccessPolicy
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
* StartInsightsRefresh
* TagResource
* UntagResource
* UpdateAccessEntry
* UpdateClusterConfig
* UpdateClusterVersion
* UpdateNodegroupConfig
* UpdatePodIdentityAssociation
