# Service-specific guidance: Amazon Elastic Container Service


This document outlines service-specific guidance for implementing a data perimeter for Amazon Elastic Container Service.

Amazon Elastic Container Service (Amazon ECS) is a fully managed container orchestration service that helps you easily deploy, manage, and scale containerized applications. You can run and scale your container workloads across AWS Regions in the cloud, and on-premises, without the complexity of managing a control plane.

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

## RegisterTaskDefinition
### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [RegisterTaskDefinition](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RegisterTaskDefinition.html) allows you to specify a service role, referred to as the task role and task execution role for your container operations. The service uses the role to make AWS API calls from your code.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json) or [network_perimeter_vpceorgid_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_vpceorgid_scp.json). These policies demonstrate how to use the `dp:include:network` tag set to `true` to restrict calls made by the task role and task execution role. If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

## RunTask
### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [RunTask](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_RunTask.html) allows you to specify a service role, referred to as the task role and task execution role for your container operations. The service uses the role to make AWS API calls from your code.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json) or [network_perimeter_vpceorgid_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_vpceorgid_scp.json). These policies demonstrate how to use the `dp:include:network` tag set to `true` to restrict calls made by the task role and task execution role. If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

## StartTask
### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [StartTask](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_StartTask.html) allows you to specify a service role, referred to as the task role and task execution role for your container operations. The service uses the role to make AWS API calls from your code.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json) or [network_perimeter_vpceorgid_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_vpceorgid_scp.json). These policies demonstrate how to use the `dp:include:network` tag set to `true` to restrict calls made by the task role and task execution role. If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

## List of service APIs reviewed against data perimeter control objectives

* CreateCluster
* CreateService
* CreateTaskSet
* DeleteAccountSetting
* DeleteAttributes
* DeleteCapacityProvider
* DeleteCluster
* DeleteService
* DeleteTaskDefinitions
* DeleteTaskSet
* DeregisterContainerInstance
* DeregisterTaskDefinition
* DescribeCapacityProviders
* DescribeClusters
* DescribeContainerInstances
* DescribeServiceDeployments
* DescribeServiceRevisions
* DescribeServices
* DescribeTaskDefinition
* DescribeTaskSets
* DescribeTasks
* DiscoverPollEndpoint
* GetTaskProtection
* ListAccountSettings
* ListAttributes
* ListClusters
* ListContainerInstances
* ListServiceDeployments
* ListServices
* ListServicesByNamespace
* ListTagsForResource
* ListTaskDefinitionFamilies
* ListTaskDefinitions
* ListTasks
* PutAccountSetting
* PutAccountSettingDefault
* PutAttributes
* PutClusterCapacityProviders
* RegisterContainerInstance
* RegisterTaskDefinition
* RunTask
* StartTask
* StopServiceDeployment
* StopTask
* TagResource
* UntagResource
* UpdateCluster
* UpdateClusterSettings
* UpdateContainerAgent
* UpdateContainerInstancesState
* UpdateService
* UpdateServicePrimaryTaskSet
* UpdateTaskProtection
* UpdateTaskSet
