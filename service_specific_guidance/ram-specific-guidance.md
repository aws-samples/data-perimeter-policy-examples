# Service-specific guidance: AWS Resource Access Manager


This document outlines service-specific guidance for implementing a data perimeter for AWS Resource Access Manager.

AWS Resource Access Manager (AWS RAM) helps you securely share your resources across AWS accounts, within your organization or organizational units (OUs), and with AWS Identity and Access Management (IAM) roles and users for supported resource types. If you have multiple AWS accounts, you can create a resource once and use AWS RAM to make that resource usable by those other accounts.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | Y |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | Y |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | N |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | N |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | N |

*Y - Additional considerations apply. N - No additional considerations apply.

## AssociateResourceShare
### Service sharing mechanism

**Perimeter type applicability**: identity perimeter applied on resource; resource perimeter applied on identity.

**Description**: [AssociateResourceShare](https://docs.aws.amazon.com/ram/latest/APIReference/API_AssociateResourceShare.html) allows you to share a resource share, and the resources it includes, with another account.

**Additional controls**:

If you want to restrict access so that only trusted identities can view information about your resources, consider implementing these additional controls:
* Preventative control example: Consider implementing the `ram:RequestedAllowsExternalPrincipals` condition key in an SCP to help prevent sharing with untrusted identities. See [data_perimeter_governance_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/data_perimeter_governance_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Principal](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-ram-principalassociation.html#cfn-ram-principalassociation-principal) property that grants permissions to untrusted identities for the [AWS::RAM::PrincipalAssociation](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-ram-principalassociation.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [AssociateResourceShare](https://docs.aws.amazon.com/ram/latest/APIReference/API_AssociateResourceShare.html) API calls in your environment (specifically, the [principals](https://docs.aws.amazon.com/ram/latest/APIReference/API_AssociateResourceShare.html#ram-AssociateResourceShare-request-principals) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access so that your identities cannot view resources that were shared with your accounts by untrusted entities, consider implementing this additional control:
* Detective control example: Consider using [GetResourceShares](https://docs.aws.amazon.com/ram/latest/APIReference/API_GetResourceShares.html) with the `resourceOwner` request parameter set to `OTHER-ACCOUNTS` to monitor the resource shares shared with your accounts (specifically, the [resourceShares](https://docs.aws.amazon.com/ram/latest/APIReference/API_GetResourceShares.html#ram-GetResourceShares-response-resourceShares) response parameter). If necessary, remediate with the responsive controls of your choice.

## CreateResourceShare
### Service sharing mechanism

**Perimeter type applicability**: identity perimeter applied on resource; resource perimeter applied on identity.

**Description**: [CreateResourceShare](https://docs.aws.amazon.com/ram/latest/APIReference/API_CreateResourceShare.html) allows you to share a supported resource, such as an Amazon VPC subnet or a transit gateway, with another account.

**Additional controls**:

If you want to restrict access so that only trusted identities can view information about your resources, consider implementing these additional controls:
* Preventative control example: Consider implementing the `ram:RequestedAllowsExternalPrincipals` condition key in an SCP to help prevent sharing with untrusted identities. See [data_perimeter_governance_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/data_perimeter_governance_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [AllowExternalPrincipals](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-ram-resourceshare.html#cfn-ram-resourceshare-allowexternalprincipals) property that grants permissions to untrusted identities for the [AWS::RAM::ResourceShare](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-ram-resourceshare.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateResourceShare](https://docs.aws.amazon.com/ram/latest/APIReference/API_CreateResourceShare.html) API calls in your environment (specifically, the [allowExternalPrincipals](https://docs.aws.amazon.com/ram/latest/APIReference/API_CreateResourceShare.html#ram-CreateResourceShare-request-allowExternalPrincipals) and [principals](https://docs.aws.amazon.com/ram/latest/APIReference/API_CreateResourceShare.html#ram-CreateResourceShare-request-principals) request parameters). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access so that your identities cannot view resources that were shared with your accounts by untrusted entities, consider implementing this additional control:
* Detective control example: Consider using [GetResourceShareInvitations](https://docs.aws.amazon.com/ram/latest/APIReference/API_GetResourceShareInvitations.html) to monitor the resource shares shared with your accounts (specifically, the [resourceShareInvitations](https://docs.aws.amazon.com/ram/latest/APIReference/API_GetResourceShareInvitations.html#ram-GetResourceShareInvitations-response-resourceShareInvitations) response parameter). If necessary, remediate with the responsive controls of your choice.

## PromoteResourceShareCreatedFromPolicy
### Service sharing mechanism

**Perimeter type applicability**: identity perimeter applied on resource; resource perimeter applied on identity.

**Description**: [PromoteResourceShareCreatedFromPolicy](https://docs.aws.amazon.com/ram/latest/APIReference/API_PromoteResourceShareCreatedFromPolicy.html) allows you to share a supported resource, such as an EC2 Image Builder image recipe, with another account.

**Additional controls**:

If you want to restrict access so that only trusted identities can view information about your resources, consider implementing these additional controls:
* Preventative control example: Consider implementing the `ram:AllowsExternalPrincipals` condition key in an SCP to help prevent sharing with untrusted identities. See [data_perimeter_governance_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/data_perimeter_governance_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [PromoteResourceShareCreatedFromPolicy](https://docs.aws.amazon.com/ram/latest/APIReference/API_PromoteResourceShareCreatedFromPolicy.html) API calls in your environment. If necessary, remediate with the responsive controls of your choice.

If you want to restrict access so that your identities cannot view resources that were shared with your accounts by untrusted entities, consider implementing this additional control:
* Detective control example: Consider using [GetResourceShareInvitations](https://docs.aws.amazon.com/ram/latest/APIReference/API_GetResourceShareInvitations.html) to monitor the resource shares shared with your accounts (specifically, the [resourceShareInvitations](https://docs.aws.amazon.com/ram/latest/APIReference/API_GetResourceShareInvitations.html#ram-GetResourceShareInvitations-response-resourceShareInvitations) response parameter). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

The service APIs covered in this review are based on botocore version [1.43.29](https://github.com/boto/botocore/tree/1.43.29/botocore).

* AssociateResourceShare
* AssociateResourceSharePermission
* CreatePermission
* CreatePermissionVersion
* CreateResourceShare
* DeletePermission
* DeletePermissionVersion
* DeleteResourceShare
* DisassociateResourceShare
* DisassociateResourceSharePermission
* GetPermission
* GetResourcePolicies
* GetResourceShareAssociations
* GetResourceShareInvitations
* GetResourceShares
* ListPermissionAssociations
* ListPermissionVersions
* ListPermissions
* ListPrincipals
* ListReplacePermissionAssociationsWork
* ListResourceSharePermissions
* ListResourceTypes
* ListResources
* ListSourceAssociations
* PromoteResourceShareCreatedFromPolicy
* ReplacePermissionAssociations
* SetDefaultPermissionVersion
* TagResource
* UntagResource
* UpdateResourceShare
