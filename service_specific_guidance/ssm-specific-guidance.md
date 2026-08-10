# Service-specific guidance: AWS Systems Manager


This document outlines service-specific guidance for implementing a data perimeter for AWS Systems Manager.

AWS Systems Manager helps you centrally view, manage, and operate nodes at scale in AWS, on-premises, and multicloud environments. With the launch of a unified console experience, Systems Manager consolidates various tools to help you complete common node tasks across AWS accounts and AWS Regions.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | Y |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | Y |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | Y |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | Y |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | Y |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | Y |

*Y - Additional considerations apply. N - No additional considerations apply.

## CreateActivation
### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [CreateActivation](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreateActivation.html) allows you to specify a service role for your hybrid activation operations. The service uses the role to make AWS API calls from your code.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json) or [network_perimeter_vpceorgid_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_vpceorgid_scp.json). These policies demonstrate how to use the `dp:include:network` tag set to `true` to restrict calls made by the hybrid activation's IAM service role. If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

## CreateAssociation
### Access to service-owned resources

**Perimeter type applicability**: resource perimeter.

**Description**: [CreateAssociation](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreateAssociation.html) might require access to service-owned documents, which are documents that are managed in service accounts.

**Additional controls**:

If you want to restrict access to trusted resources for your identities, consider implementing this control:
* Preventative control example: Consider implementing the default resource perimeter SCP, [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json), which lists service-owned documents in the `NotResource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access.

If you want to restrict access to your networks to trusted resources, consider implementing this control:
* Preventative control example: Consider implementing the service-specific VPC endpoint policy, [ssm_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/ssm_endpoint_policy.json), which lists service-owned documents in the `Resource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that might be accessed from your networks.

## CreateOpsItem
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [CreateOpsItem](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreateOpsItem.html) allows you to specify an SNS topic that does not belong to your organization as the value for the `Notifications` request parameter. Because the subsequent call against the SNS topic is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing this additional control:
* Detective control example: Consider using CloudTrail management events to monitor the [CreateOpsItem](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreateOpsItem.html) API calls in your environment (specifically, the [Notifications](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_OpsItemNotification.html) request parameter). If necessary, remediate with the responsive controls of your choice.

## CreateResourceDataSync
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [CreateResourceDataSync](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreateResourceDataSync.html) allows you to specify an S3 bucket that does not belong to your organization as the value for the `S3Destination` request parameter. Because the subsequent call against the S3 bucket is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [S3Destination](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-ssm-resourcedatasync.html#cfn-ssm-resourcedatasync-s3destination) property that does not belong to your organization for the [AWS::SSM::ResourceDataSync](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-ssm-resourcedatasync.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateResourceDataSync](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreateResourceDataSync.html) API calls in your environment (specifically, the [S3Destination](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_ResourceDataSyncS3Destination.html) request parameter). If necessary, remediate with the responsive controls of your choice.

## DeleteAssociation
### Access to service-owned resources

**Perimeter type applicability**: resource perimeter.

**Description**: [DeleteAssociation](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DeleteAssociation.html) might require access to service-owned documents, which are documents that are managed in service accounts.

**Additional controls**:

If you want to restrict access to trusted resources for your identities, consider implementing this control:
* Preventative control example: Consider implementing the default resource perimeter SCP, [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json), which lists service-owned documents in the `NotResource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access.

If you want to restrict access to your networks to trusted resources, consider implementing this control:
* Preventative control example: Consider implementing the service-specific VPC endpoint policy, [ssm_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/ssm_endpoint_policy.json), which lists service-owned documents in the `Resource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that might be accessed from your networks.

## GetDeployablePatchSnapshotForInstance
### S3 presigned URLs created by service principals

**Perimeter type applicability**: network perimeter on resource.

**Description**: [GetDeployablePatchSnapshotForInstance](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetDeployablePatchSnapshotForInstance.html) returns an Amazon S3 presigned URL that users can use to download AWS Systems Manager patch baseline snapshots from service-owned buckets. The presigned URL is created with the AWS Systems Manager service principal.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [GetDeployablePatchSnapshotForInstance](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetDeployablePatchSnapshotForInstance.html) permissions to select principals using an SCP. See [restrict_presignedURL_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_presignedURL_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [GetDeployablePatchSnapshotForInstance](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetDeployablePatchSnapshotForInstance.html) API calls in your environment. If necessary, remediate with the responsive controls of your choice.

## GetParameter
### Access to service-owned resources

**Perimeter type applicability**: resource perimeter.

**Description**: [GetParameter](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParameter.html) might require access to service-owned parameters, which are parameters that are managed in service accounts.

**Additional controls**:

If you want to restrict access to trusted resources for your identities, consider implementing this control:
* Preventative control example: Consider implementing the default resource perimeter SCP, [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json), which lists service-owned parameters in the `NotResource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access.

If you want to restrict access to your networks to trusted resources, consider implementing this control:
* Preventative control example: Consider implementing the service-specific VPC endpoint policy, [ssm_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/ssm_endpoint_policy.json), which lists service-owned parameters in the `Resource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that might be accessed from your networks.

## GetPatchBaseline
### Access to service-owned resources

**Perimeter type applicability**: resource perimeter.

**Description**: [GetPatchBaseline](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetPatchBaseline.html) might require access to service-owned patch baselines, which are patch baselines that are managed in service accounts.

**Additional controls**:

If you want to restrict access to trusted resources for your identities, consider implementing this control:
* Preventative control example: Consider implementing the default resource perimeter SCP, [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json), which lists service-owned patch baselines in the `NotResource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access.

If you want to restrict access to your networks to trusted resources, consider implementing this control:
* Preventative control example: Consider implementing the service-specific VPC endpoint policy, [ssm_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/ssm_endpoint_policy.json), which lists service-owned patch baselines in the `Resource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that might be accessed from your networks.

## ModifyDocumentPermission
### Service sharing mechanism

**Perimeter type applicability**: identity perimeter applied on resource; resource perimeter applied on identity.

**Description**: [ModifyDocumentPermission](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_ModifyDocumentPermission.html) allows you to share a Systems Manager document with another account.

**Additional controls**:

If you want to restrict access so that only trusted identities can view information about your resources, consider implementing these additional controls:
* Preventative control example: Consider restricting [ModifyDocumentPermission](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_ModifyDocumentPermission.html) permissions to select principals using an SCP. See [data_perimeter_governance_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/data_perimeter_governance_scp.json) for an example policy and ["Sid":"PreventExternalResourceShare"](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies#sidpreventexternalresourceshare) for a list of resources that can be granted cross-account access.
* Detective control example: Consider using CloudTrail management events to monitor the [ModifyDocumentPermission](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_ModifyDocumentPermission.html) API calls in your environment (specifically, the [AccountIdsToAdd](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_ModifyDocumentPermission.html#systemsmanager-ModifyDocumentPermission-request-AccountIdsToAdd) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access so that your identities cannot view resources that were shared with your accounts by untrusted entities, consider implementing this additional control:
* Detective control example: Consider using [DescribeDocument](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribeDocument.html) to monitor the Systems Manager documents shared with your accounts (specifically, the [Owner](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DocumentDescription.html#systemsmanager-Type-DocumentDescription-Owner) response parameter). If necessary, remediate with the responsive controls of your choice.

## PutResourcePolicy
### No RCP support

**Perimeter type applicability**: identity and network perimeter applied on resource.

**Description**: [PutResourcePolicy](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PutResourcePolicy.html) allows you to apply a resource-based policy to grant access to an `OpsItemGroup` or a `Parameter`. The service currently does not support RCPs.

**Additional controls**:

If you want to restrict access to trusted identities, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutResourcePolicy](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PutResourcePolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Policy](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-ssm-resourcepolicy.html#cfn-ssm-resourcepolicy-policy) property for the [AWS::SSM::ResourcePolicy](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-ssm-resourcepolicy.html) resource that grants permissions to untrusted identities.
* Detective control example: Consider using CloudTrail management events to monitor the [PutResourcePolicy](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PutResourcePolicy.html) API calls in your environment (specifically, the [Policy](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PutResourcePolicy.html#systemsmanager-PutResourcePolicy-request-Policy) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutResourcePolicy](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PutResourcePolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Policy](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-ssm-resourcepolicy.html#cfn-ssm-resourcepolicy-policy) property for the [AWS::SSM::ResourcePolicy](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-ssm-resourcepolicy.html) resource that grants permissions to unexpected networks.
* Detective control example: Consider using CloudTrail management events to monitor the [PutResourcePolicy](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PutResourcePolicy.html) API calls in your environment (specifically, the [Policy](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PutResourcePolicy.html#systemsmanager-PutResourcePolicy-request-Policy) request parameter). If necessary, remediate with the responsive controls of your choice.

## SendCommand
### Access to service-owned resources

**Perimeter type applicability**: resource perimeter.

**Description**: [SendCommand](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_SendCommand.html) might require access to service-owned documents, which are documents that are managed in service accounts.

**Additional controls**:

If you want to restrict access to trusted resources for your identities, consider implementing this control:
* Preventative control example: Consider implementing the default resource perimeter SCP, [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json), which lists service-owned documents in the `NotResource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access.

If you want to restrict access to your networks to trusted resources, consider implementing this control:
* Preventative control example: Consider implementing the service-specific VPC endpoint policy, [ssm_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/ssm_endpoint_policy.json), which lists service-owned documents in the `Resource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that might be accessed from your networks.

## StartAutomationExecution
### Access to service-owned resources

**Perimeter type applicability**: resource perimeter.

**Description**: [StartAutomationExecution](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_StartAutomationExecution.html) might require access to service-owned automation runbooks, which are automation runbooks that are managed in service accounts.

**Additional controls**:

If you want to restrict access to trusted resources for your identities, consider implementing this control:
* Preventative control example: Consider implementing the default resource perimeter SCP, [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json), which lists service-owned automation runbooks in the `NotResource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access.

If you want to restrict access to your networks to trusted resources, consider implementing this control:
* Preventative control example: Consider implementing the service-specific VPC endpoint policy, [ssm_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/ssm_endpoint_policy.json), which lists service-owned automation runbooks in the `Resource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that might be accessed from your networks.

### Code execution within AWS network

**Perimeter type applicability**: identity and resource perimeter on network, network perimeter.

**Description**: [StartAutomationExecution](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_StartAutomationExecution.html) allows you to create Automation runbooks to run your code, but it does not currently support resource creation within your Amazon VPC.

**Additional controls**:

If you want to achieve data perimeter control objectives, consider implementing these additional controls:
* Preventative control example: Consider restricting [StartAutomationExecution](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_StartAutomationExecution.html) permissions to select principals using an SCP. See [restrict_nonvpc_deployment_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [StartAutomationExecution](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_StartAutomationExecution.html) API calls in your environment. If necessary, remediate with the responsive controls of your choice.

## StartChangeRequestExecution
### Access to service-owned resources

**Perimeter type applicability**: resource perimeter.

**Description**: [StartChangeRequestExecution](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_StartChangeRequestExecution.html) might require access to service-owned automation runbooks, which are automation runbooks that are managed in service accounts.

**Additional controls**:

If you want to restrict access to trusted resources for your identities, consider implementing this control:
* Preventative control example: Consider implementing the default resource perimeter SCP, [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json), which lists service-owned automation runbooks in the `NotResource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access.

If you want to restrict access to your networks to trusted resources, consider implementing this control:
* Preventative control example: Consider implementing the service-specific VPC endpoint policy, [ssm_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/ssm_endpoint_policy.json), which lists service-owned automation runbooks in the `Resource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that might be accessed from your networks.

## StartSession
### Access to service-owned resources

**Perimeter type applicability**: resource perimeter.

**Description**: [StartSession](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_StartSession.html) might require access to service-owned documents, which are documents that are managed in service accounts.

**Additional controls**:

If you want to restrict access to trusted resources for your identities, consider implementing this control:
* Preventative control example: Consider implementing the default resource perimeter SCP, [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json), which lists service-owned documents in the `NotResource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access.

If you want to restrict access to your networks to trusted resources, consider implementing this control:
* Preventative control example: Consider implementing the service-specific VPC endpoint policy, [ssm_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/ssm_endpoint_policy.json), which lists service-owned documents in the `Resource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that might be accessed from your networks.

## UpdateAssociation
### Access to service-owned resources

**Perimeter type applicability**: resource perimeter.

**Description**: [UpdateAssociation](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_UpdateAssociation.html) might require access to service-owned documents, which are documents that are managed in service accounts.

**Additional controls**:

If you want to restrict access to trusted resources for your identities, consider implementing this control:
* Preventative control example: Consider implementing the default resource perimeter SCP, [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json), which lists service-owned documents in the `NotResource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access.

If you want to restrict access to your networks to trusted resources, consider implementing this control:
* Preventative control example: Consider implementing the service-specific VPC endpoint policy, [ssm_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/ssm_endpoint_policy.json), which lists service-owned documents in the `Resource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that might be accessed from your networks.

## UpdateAssociationStatus
### Access to service-owned resources

**Perimeter type applicability**: resource perimeter.

**Description**: [UpdateAssociationStatus](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_UpdateAssociationStatus.html) might require access to service-owned documents, which are documents that are managed in service accounts.

**Additional controls**:

If you want to restrict access to trusted resources for your identities, consider implementing this control:
* Preventative control example: Consider implementing the default resource perimeter SCP, [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json), which lists service-owned documents in the `NotResource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access.

If you want to restrict access to your networks to trusted resources, consider implementing this control:
* Preventative control example: Consider implementing the service-specific VPC endpoint policy, [ssm_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/ssm_endpoint_policy.json), which lists service-owned documents in the `Resource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that might be accessed from your networks.

## UpdateManagedInstanceRole
### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [UpdateManagedInstanceRole](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_UpdateManagedInstanceRole.html) allows you to specify a service role for your hybrid activation operations. The service uses the role to make AWS API calls from your code.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json) or [network_perimeter_vpceorgid_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_vpceorgid_scp.json). These policies demonstrate how to use the `dp:include:network` tag set to `true` to restrict calls made by the managed node's IAM service role. If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

## List of service APIs reviewed against data perimeter control objectives

* AddTagsToResource
* AssociateOpsItemRelatedItem
* CancelCommand
* CancelMaintenanceWindowExecution
* CreateActivation
* CreateAssociation
* CreateDocument
* CreateMaintenanceWindow
* CreateOpsItem
* CreateOpsMetadata
* CreatePatchBaseline
* CreateResourceDataSync
* DeleteActivation
* DeleteAssociation
* DeleteDocument
* DeleteInventory
* DeleteMaintenanceWindow
* DeleteOpsItem
* DeleteOpsMetadata
* DeleteParameter
* DeleteParameters
* DeletePatchBaseline
* DeleteResourceDataSync
* DeregisterPatchBaselineForPatchGroup
* DeregisterTargetFromMaintenanceWindow
* DeregisterTaskFromMaintenanceWindow
* DescribeActivations
* DescribeAssociation
* DescribeAssociationExecutionTargets
* DescribeAssociationExecutions
* DescribeAutomationExecutions
* DescribeAutomationStepExecutions
* DescribeAvailablePatches
* DescribeDocument
* DescribeDocumentPermission
* DescribeEffectiveInstanceAssociations
* DescribeEffectivePatchesForPatchBaseline
* DescribeInstanceAssociationsStatus
* DescribeInstanceInformation
* DescribeInstancePatchStates
* DescribeInstancePatchStatesForPatchGroup
* DescribeInstancePatches
* DescribeInstanceProperties
* DescribeInventoryDeletions
* DescribeMaintenanceWindowExecutionTaskInvocations
* DescribeMaintenanceWindowExecutionTasks
* DescribeMaintenanceWindowExecutions
* DescribeMaintenanceWindowSchedule
* DescribeMaintenanceWindowTargets
* DescribeMaintenanceWindowTasks
* DescribeMaintenanceWindows
* DescribeMaintenanceWindowsForTarget
* DescribeOpsItems
* DescribeParameters
* DescribePatchBaselines
* DescribePatchGroupState
* DescribePatchGroups
* DescribePatchProperties
* DescribeSessions
* DisassociateOpsItemRelatedItem
* GetAutomationExecution
* GetCalendarState
* GetCommandInvocation
* GetConnectionStatus
* GetDefaultPatchBaseline
* GetDeployablePatchSnapshotForInstance
* GetDocument
* GetExecutionPreview
* GetInventory
* GetInventorySchema
* GetMaintenanceWindow
* GetMaintenanceWindowExecution
* GetMaintenanceWindowExecutionTask
* GetMaintenanceWindowExecutionTaskInvocation
* GetMaintenanceWindowTask
* GetOpsItem
* GetOpsMetadata
* GetOpsSummary
* GetParameter
* GetParameterHistory
* GetParameters
* GetParametersByPath
* GetPatchBaseline
* GetPatchBaselineForPatchGroup
* GetServiceSetting
* LabelParameterVersion
* ListAssociationVersions
* ListAssociations
* ListCommandInvocations
* ListCommands
* ListComplianceItems
* ListComplianceSummaries
* ListDocumentMetadataHistory
* ListDocumentVersions
* ListDocuments
* ListInventoryEntries
* ListNodes
* ListNodesSummary
* ListOpsItemEvents
* ListOpsItemRelatedItems
* ListOpsMetadata
* ListResourceComplianceSummaries
* ListResourceDataSync
* ListTagsForResource
* ModifyDocumentPermission
* PutComplianceItems
* PutInventory
* PutParameter
* PutResourcePolicy
* RegisterDefaultPatchBaseline
* RegisterPatchBaselineForPatchGroup
* RegisterTargetWithMaintenanceWindow
* RegisterTaskWithMaintenanceWindow
* RemoveTagsFromResource
* ResetServiceSetting
* ResumeSession
* SendCommand
* StartAssociationsOnce
* StartAutomationExecution
* StartChangeRequestExecution
* StartExecutionPreview
* StartSession
* StopAutomationExecution
* TerminateSession
* UnlabelParameterVersion
* UpdateAssociation
* UpdateAssociationStatus
* UpdateDocument
* UpdateDocumentDefaultVersion
* UpdateMaintenanceWindow
* UpdateMaintenanceWindowTarget
* UpdateMaintenanceWindowTask
* UpdateManagedInstanceRole
* UpdateOpsMetadata
* UpdatePatchBaseline
* UpdateResourceDataSync
* UpdateServiceSetting
