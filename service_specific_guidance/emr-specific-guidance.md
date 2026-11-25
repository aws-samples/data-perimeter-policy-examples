# Service-specific guidance: Amazon Elastic MapReduce


This document outlines service-specific guidance for implementing a data perimeter for Amazon Elastic MapReduce.

Amazon EMR (previously called Amazon Elastic MapReduce) is a managed cluster platform that simplifies running big data frameworks, such as Apache Hadoop and Apache Spark, on AWS to process and analyze vast amounts of data. It also lets you transform and move large amounts of data into and out of other AWS data stores and databases, such as Amazon S3 and Amazon DynamoDB.

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

## AddJobFlowSteps
### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [AddJobFlowSteps](https://docs.aws.amazon.com/emr/latest/APIReference/API_AddJobFlowSteps.html) allows you to specify a service role, referred to as the runtime role, for your job flow step operations. The role's credentials are provided to the compute environment, which uses them to call AWS APIs on your behalf.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json) or [network_perimeter_vpceorgid_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_vpceorgid_scp.json). These policies demonstrate how to use the `dp:include:network` tag set to `true` to restrict calls made by the runtime role. If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

## CreateSecurityConfiguration
### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [CreateSecurityConfiguration](https://docs.aws.amazon.com/emr/latest/APIReference/API_CreateSecurityConfiguration.html) allows you to specify service roles, referred to as the IAM roles for EMRFS, the IAM role for Apache Ranger Engines, and the IAM role for other AWS services, for your EMR File System and Apache Ranger operations. The roles' credentials are provided to the compute environment, which uses them to call AWS APIs on your behalf.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json) or [network_perimeter_vpceorgid_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_vpceorgid_scp.json). These policies demonstrate how to use the `dp:include:network` tag set to `true` to restrict calls made by the IAM roles for EMRFS, the IAM role for Apache Ranger Engines, and the IAM role for other AWS services. If a service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

## RunJobFlow
### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [RunJobFlow](https://docs.aws.amazon.com/emr/latest/APIReference/API_RunJobFlow.html) allows you to specify a service role, referred to as the EC2 instance profile for Amazon EMR, for your Amazon EMR cluster operations. The role's credentials are provided to the compute environment, which uses them to call AWS APIs on your behalf.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the service-specific SCP, [network_perimeter_ec2_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/network_perimeter_ec2_scp.json). If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [RunJobFlow](https://docs.aws.amazon.com/emr/latest/APIReference/API_RunJobFlow.html) allows you to specify a service role, referred to as the runtime role, for your job flow step operations. The role's credentials are provided to the compute environment, which uses them to call AWS APIs on your behalf.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json) or [network_perimeter_vpceorgid_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_vpceorgid_scp.json). These policies demonstrate how to use the `dp:include:network` tag set to `true` to restrict calls made by the runtime role for your Amazon EMR steps. If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

## StartNotebookExecution
### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [StartNotebookExecution](https://docs.aws.amazon.com/emr/latest/APIReference/API_StartNotebookExecution.html) allows you to specify a service role, referred to as the execution role, for your EMR notebook execution operations. The role's credentials are provided to the compute environment, which uses them to call AWS APIs on your behalf.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json) or [network_perimeter_vpceorgid_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_vpceorgid_scp.json). These policies demonstrate how to use the `dp:include:network` tag set to `true` to restrict calls made by the EMR notebook execution's runtime role. If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

## StartSession
### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [StartSession](https://docs.aws.amazon.com/emr/latest/APIReference/API_StartSession.html) allows you to specify a service role, referred to as the session's execution role, for your Amazon EMR Spark Connect session operations. The role's credentials are provided to the compute environment, which uses them to call AWS APIs on your behalf.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json) or [network_perimeter_vpceorgid_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_vpceorgid_scp.json). These policies demonstrate how to use the `dp:include:network` tag set to `true` to restrict calls made by the session's execution role. If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

## List of service APIs reviewed against data perimeter control objectives

The service APIs covered in this review are based on botocore version [1.43.29](https://github.com/boto/botocore/tree/1.43.29/botocore).

* AddInstanceGroups
* AddJobFlowSteps
* AddTags
* CancelSteps
* CreatePersistentAppUI
* CreateSecurityConfiguration
* CreateStudio
* CreateStudioSessionMapping
* DeleteSecurityConfiguration
* DeleteStudio
* DescribeCluster
* DescribePersistentAppUI
* DescribeReleaseLabel
* DescribeSecurityConfiguration
* DescribeStep
* DescribeStudio
* GetAutoTerminationPolicy
* GetBlockPublicAccessConfiguration
* GetManagedScalingPolicy
* GetPersistentAppUIPresignedURL
* ListBootstrapActions
* ListClusters
* ListInstanceGroups
* ListInstances
* ListNotebookExecutions
* ListReleaseLabels
* ListSecurityConfigurations
* ListSteps
* ListStudioSessionMappings
* ListStudios
* ListSupportedInstanceTypes
* ModifyCluster
* ModifyInstanceGroups
* PutAutoScalingPolicy
* PutAutoTerminationPolicy
* PutBlockPublicAccessConfiguration
* PutManagedScalingPolicy
* RemoveAutoScalingPolicy
* RemoveAutoTerminationPolicy
* RemoveManagedScalingPolicy
* RemoveTags
* RunJobFlow
* SetKeepJobFlowAliveWhenNoSteps
* SetTerminationProtection
* SetUnhealthyNodeReplacement
* SetVisibleToAllUsers
* StartNotebookExecution
* StartSession
* TerminateJobFlows
* UpdateStudio
* UpdateStudioSessionMapping
