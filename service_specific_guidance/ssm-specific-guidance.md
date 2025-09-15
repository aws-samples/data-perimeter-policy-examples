# Service-specific guidance: AWS Systems Manager


This document outlines service-specific guidance for implementing a data perimeter for AWS Systems Manager. 


AWS Systems Manager is a comprehensive management solution that helps you centrally manage and automate operational tasks across your AWS resources and on-premises infrastructure. It provides a unified interface for viewing operational data, automating processes, maintaining security and compliance, and streamlining resource management across your environments. With Systems Manager, you can efficiently monitor, patch, and configure your systems at scale.


The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type | Security objective | Applied on | Policy type | Additional considerations |
|----------------|-------------------|------------|-------------|------------------------|
| Identity perimeter | Only trusted identities can access my resources | Resource | RCP | Y |
| Identity perimeter | Only trusted identities are allowed from my network | Network | VPC endpoint policy | Y |
| Resource perimeter | My identities can access only trusted resources | Identity | SCP | Y |
| Resource perimeter | Only trusted resources can be accessed from my network | Network | VPC endpoint policy | Y |
| Network perimeter | My identities can access resources only from expected networks | Identity | SCP | Y |
| Network perimeter | My resources can be accessed only from expected networks | Resource | RCP | Y |

*Y – Additional considerations apply. N – No additional considerations apply.
 


**Additional consideration 1**

Perimeter type applicability: resource perimeter.
        
SendCommand might require access to service-owned documents, which are documents that are managed in service accounts.

See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access. 

If you want to restrict access to trusted resources, see [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json) for information about how the default resource perimeter SCP accounts for such an access pattern. Use the service-specific policy,  [ssm_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/ssm_endpoint_policy.json) to enforce the resource perimeter on your network.


**Additional consideration 2**

Perimeter type applicability: network perimeter.
        
GetDeployablePatchSnapshotForInstance returns an Amazon S3 presigned URL that users can use to download AWS Systems Manager patch snapshots from service-owned buckets. The presigned URL is created with a Systems Manager service account identity.

If you want to restrict access to expected networks, consider implementing these additional controls:

* **Preventative control example:** Consider restricting [GetDeployablePatchSnapshotForInstance](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetDeployablePatchSnapshotForInstance.html) permissions to administrators or specific applications only using an SCP. See [restrict_presignedUrl_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_presignedURL_scp.json) for an example policy.
* **Detective control example:** Consider using CloudTrail management events to monitor the [GetDeployablePatchSnapshotForInstance](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetDeployablePatchSnapshotForInstance.html) API calls in your environment. If necessary, remediate with the responsive controls of your choice.


**Additional consideration 3**

Perimeter type applicability: resource perimeter.
        
GetParameter might require access to service-owned parameters, which are parameters that are managed in service accounts.

See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access. 

If you want to restrict access to trusted resources, see [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json) for information about how the default resource perimeter SCP accounts for such an access pattern. Use the service-specific policy, [ssm_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/ssm_endpoint_policy.json) to enforce the resource perimeter on your network.


**Additional consideration 4**

Perimeter type applicability: resource perimeter applied on identity.
        
CreateOpsItem allows you to specify an SNS topic that does not belong to your organization as the value for the Notifications request parameter. Because the subsequent Publish call against the topic is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls:

* **Detective control example:** Consider using CloudTrail management events to monitor the [CreateOpsItem](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreateOpsItem.html) API calls in your environment (specifically, the [Notifications](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreateOpsItem.html#systemsmanager-CreateOpsItem-request-Notifications) request parameter). If necessary, remediate with the responsive controls of your choice.


**Additional consideration 5**

Perimeter type applicability: resource perimeter.
        
CreateAssociation might require access to service-owned documents, which are documents that are managed in service accounts.

See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access. 

If you want to restrict access to trusted resources, see [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json) for information about how the default resource perimeter SCP accounts for such an access pattern. Use the service-specific policy,  [ssm_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/ssm_endpoint_policy.json) to enforce the resource perimeter on your network.


**Additional consideration 6**

Perimeter type applicability: resource perimeter.
        
StartSession might require access to service-owned documents, which are documents that are managed in service accounts.

See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access. 

If you want to restrict access to trusted resources, see [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json) for information about how the default resource perimeter SCP accounts for such an access pattern. Use the service-specific policy,  [ssm_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/ssm_endpoint_policy.json) to enforce the resource perimeter on your network.


**Additional consideration 7**

Perimeter type applicability: resource perimeter.
        
StartChangeRequestExecution might require access to service-owned automation runbooks, which are automation runbooks that are managed in service accounts.


See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access. 

If you want to restrict access to trusted resources, see [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json) for information about how the default resource perimeter SCP accounts for such an access pattern. Use the service-specific policy,  [ssm_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/ssm_endpoint_policy.json) to enforce the resource perimeter on your network.


**Additional consideration 8**

Perimeter type applicability: resource perimeter applied on identity.
        
CreateResourceDataSync allows you to specify an S3 bucket that does not belong to your organization as the value for the S3Destination request parameter. Because the subsequent PutObject call against the bucket is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls:

* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [S3Destination](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-ssm-resourcedatasync.html#cfn-ssm-resourcedatasync-s3destination) property that does not belong to your organization for the [AWS::SSM::ResourceDataSync](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-ssm-resourcedatasync.html) resource.

* **Detective control example:** Consider using CloudTrail management events to monitor the [CreateResourceDataSync](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreateResourceDataSync.html) API calls in your environment (specifically, the [S3Destination](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreateResourceDataSync.html#systemsmanager-CreateResourceDataSync-request-S3Destination) request parameter). If necessary, remediate with the responsive controls of your choice.


**Additional consideration 9**

Perimeter type applicability: resource perimeter.
        
StartAutomationExecution might require access to service-owned automation runbooks, which are automation runbooks that are managed in service accounts.

See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access. 

If you want to restrict access to trusted resources, see [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json) for information about how the default resource perimeter SCP accounts for such an access pattern. Use the service-specific policy,  [ssm_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/ssm_endpoint_policy.json) to enforce the resource perimeter on your network.


**Additional consideration 10**

Perimeter type applicability: identity perimeter applied on resource; resource perimeter applied on identity.
        
ModifyDocumentPermission allows you to share a document with another account.

See ["Sid":"PreventExternalResourceShare"](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies#sidpreventexternalresourceshare) for a list of resources that can be granted cross-account access.

If you want to restrict access so that only trusted identities can take actions against your resources, consider implementing these additional controls:

* **Preventative control example:** Consider restricting [ModifyDocumentPermission](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_ModifyDocumentPermission.html) permissions to administrators only using an SCP. See [data_perimeter_governance_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/data_perimeter_governance_scp.json) for an example policy.
* **Detective control example:** Consider using CloudTrail management events to monitor the [ModifyDocumentPermission](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_ModifyDocumentPermission.html) API calls in your environment (specifically, the [AccountIdsToAdd](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_ModifyDocumentPermission.html#API_ModifyDocumentPermission_RequestParameters) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access so that your identities cannot view resources that were shared with your accounts by untrusted entities, consider implementing this additional control:

* **Detective control example:** Consider using [DescribeDocument](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribeDocument.html) to monitor the SSM documents shared with your accounts (specifically, the [Owner](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DocumentDescription.html#systemsmanager-Type-DocumentDescription-Owner) response parameter). If necessary, remediate with the responsive controls of your choice.


**Additional consideration 11**

Perimeter type applicability: all.
        
StartAutomationExecution allows you to run your code, but it does not currently support resource creation within your Amazon VPC. 

If you want to achieve data perimeter control objectives, consider implementing these additional controls:

* **Preventative control example:** Consider restricting the AWS Systems Manager StartAutomationExecution permissions to administrators only using an SCP. See [restrict_nonvpc_deployment_scp.json](../service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.



**List of service APIs reviewed against data perimeter control objectives**
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
* DescribeAssociationExecutions
* DescribeAssociationExecutionTargets
* DescribeAutomationExecutions
* DescribeAutomationStepExecutions
* DescribeAvailablePatches
* DescribeDocument
* DescribeDocumentPermission
* DescribeEffectiveInstanceAssociations
* DescribeEffectivePatchesForPatchBaseline
* DescribeInstanceAssociationsStatus
* DescribeInstanceInformation
* DescribeInstancePatches
* DescribeInstancePatchStates
* DescribeInstancePatchStatesForPatchGroup
* DescribeInstanceProperties
* DescribeInventoryDeletions
* DescribeMaintenanceWindowExecutions
* DescribeMaintenanceWindowExecutionTaskInvocations
* DescribeMaintenanceWindowExecutionTasks
* DescribeMaintenanceWindows
* DescribeMaintenanceWindowSchedule
* DescribeMaintenanceWindowsForTarget
* DescribeMaintenanceWindowTargets
* DescribeMaintenanceWindowTasks
* DescribeOpsItems
* DescribeParameters
* DescribePatchBaselines
* DescribePatchGroups
* DescribePatchGroupState
* DescribePatchProperties
* DescribeSessions
* DisassociateOpsItemRelatedItem
* GetAutomationExecution
* GetCalendarState
* GetCommandInvocation
* GetConnectionStatus
* GetDefaultPatchBaseline
* GetDocument
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
* ListAssociations
* ListAssociationVersions
* ListCommandInvocations
* ListCommands
* ListComplianceItems
* ListComplianceSummaries
* ListDocumentMetadataHistory
* ListDocuments
* ListDocumentVersions
* ListInventoryEntries
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
* StartSession
* StopAutomationExecution
* TerminateSession
* UnlabelParameterVersion
* UpdateAssociation
* UpdateDocument
* UpdateDocumentDefaultVersion
* UpdateMaintenanceWindow
* UpdateMaintenanceWindowTarget
* UpdateMaintenanceWindowTask
* UpdateOpsMetadata
* UpdatePatchBaseline
* UpdateResourceDataSync
* UpdateServiceSetting