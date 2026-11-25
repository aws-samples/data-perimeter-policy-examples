# Service-specific guidance: AWS Security Hub


This document outlines service-specific guidance for implementing a data perimeter for AWS Security Hub.

AWS Security Hub is a unified cloud security solution that prioritizes your critical security issues and helps you respond at scale by automatically correlating and enriching security signals from posture management (AWS Security Hub CSPM), vulnerability management (Amazon Inspector), sensitive data (Amazon Macie), and threat detection (Amazon GuardDuty). Its complementary service, AWS Security Hub CSPM, provides a comprehensive view of your security state in AWS and helps you assess your environment against security industry standards and best practices.

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

## AcceptAdministratorInvitation
### No ResourceOrgID support

**Perimeter type applicability**: resource perimeter on identity.

**Description**: [AcceptAdministratorInvitation](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_AcceptAdministratorInvitation.html) does not currently support the `aws:ResourceOrgID` condition key for the `InvitationId` request parameter.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Preventative control example: Consider restricting [AcceptAdministratorInvitation](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_AcceptAdministratorInvitation.html) permissions to select principals using an SCP. See [data_perimeter_governance_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/data_perimeter_governance_scp.json) for an example policy and ["Sid":"ProtectActionsNotSupportedByPrimaryDPControls"](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies#sidprotectactionsnotsupportedbyprimarydpcontrols) for other APIs that currently don't support the `aws:ResourceOrgID` condition key.
* Detective control example: Consider using CloudTrail management events to monitor the [AcceptAdministratorInvitation](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_AcceptAdministratorInvitation.html) API calls in your environment (specifically, the [InvitationId](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_AcceptAdministratorInvitation.html#securityhub-AcceptAdministratorInvitation-request-InvitationId) request parameter). If necessary, remediate with the responsive controls of your choice.

## CreateMembers
### Service sharing mechanism

**Perimeter type applicability**: identity perimeter applied on resource; resource perimeter applied on identity.

**Description**: [CreateMembers](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_CreateMembers.html) allows you to add an account as a member association in your administrator account.

**Additional controls**:

If you want to restrict access so that only trusted identities can view information about your resources, consider implementing these additional controls:
* Preventative control example: Consider restricting [CreateMembers](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_CreateMembers.html) permissions to select principals using an SCP. See [data_perimeter_governance_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/data_perimeter_governance_scp.json) for an example policy and ["Sid":"PreventExternalResourceShare"](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies#sidpreventexternalresourceshare) for a list of resources that can be granted cross-account access.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateMembers](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_CreateMembers.html) API calls in your environment (specifically, the `AccountId` values in the [AccountDetails](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_AccountDetails.html) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access so that your identities cannot view resources that were shared with your accounts by untrusted entities, consider implementing this additional control:
* Detective control example: Consider using [ListInvitations](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_ListInvitations.html) to monitor the Security Hub CSPM membership invitations created for your accounts (specifically, the [Invitations](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_ListInvitations.html#securityhub-ListInvitations-response-Invitations) response parameter). If necessary, remediate with the responsive controls of your choice.

## InviteMembers
### Service sharing mechanism

**Perimeter type applicability**: identity perimeter applied on resource; resource perimeter applied on identity.

**Description**: [InviteMembers](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_InviteMembers.html) allows you to invite another account to your administrator account.

**Additional controls**:

If you want to restrict access so that only trusted identities can view information about your resources, consider implementing these additional controls:
* Preventative control example: Consider restricting [InviteMembers](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_InviteMembers.html) permissions to select principals using an SCP. See [data_perimeter_governance_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/data_perimeter_governance_scp.json) for an example policy and ["Sid":"PreventExternalResourceShare"](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies#sidpreventexternalresourceshare) for a list of resources that can be granted cross-account access.
* Detective control example: Consider using CloudTrail management events to monitor the [InviteMembers](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_InviteMembers.html) API calls in your environment (specifically, the [AccountIds](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_InviteMembers.html#securityhub-InviteMembers-request-AccountIds) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access so that your identities cannot view resources that were shared with your accounts by untrusted entities, consider implementing this additional control:
* Detective control example: Consider using [ListInvitations](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_ListInvitations.html) to monitor the Security Hub CSPM membership invitations created for your accounts (specifically, the [Invitations](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_ListInvitations.html#securityhub-ListInvitations-response-Invitations) response parameter). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

The service APIs covered in this review are based on botocore version [1.43.29](https://github.com/boto/botocore/tree/1.43.29/botocore).

* AcceptAdministratorInvitation
* BatchDeleteAutomationRules
* BatchDisableStandards
* BatchEnableStandards
* BatchGetAutomationRules
* BatchGetConfigurationPolicyAssociations
* BatchGetSecurityControls
* BatchGetStandardsControlAssociations
* BatchImportFindings
* BatchUpdateAutomationRules
* BatchUpdateFindings
* BatchUpdateFindingsV2
* BatchUpdateStandardsControlAssociations
* CreateActionTarget
* CreateAggregatorV2
* CreateAutomationRule
* CreateAutomationRuleV2
* CreateConfigurationPolicy
* CreateConnectorV2
* CreateFindingAggregator
* CreateInsight
* CreateMembers
* DeleteActionTarget
* DeleteAggregatorV2
* DeleteAutomationRuleV2
* DeleteConnectorV2
* DeleteFindingAggregator
* DeleteInsight
* DeleteInvitations
* DeleteMembers
* DescribeActionTargets
* DescribeHub
* DescribeOrganizationConfiguration
* DescribeProducts
* DescribeProductsV2
* DescribeSecurityHubV2
* DescribeStandards
* DescribeStandardsControls
* DisableImportFindingsForProduct
* DisableSecurityHubV2
* DisassociateMembers
* EnableImportFindingsForProduct
* EnableSecurityHubV2
* GetAdministratorAccount
* GetAggregatorV2
* GetAutomationRuleV2
* GetConnectorV2
* GetEnabledStandards
* GetFindingAggregator
* GetFindingHistory
* GetFindingStatisticsV2
* GetFindings
* GetFindingsTrendsV2
* GetFindingsV2
* GetInsightResults
* GetInsights
* GetInvitationsCount
* GetMasterAccount
* GetMembers
* GetResourcesStatisticsV2
* GetResourcesTrendsV2
* GetResourcesV2
* GetSecurityControlDefinition
* InviteMembers
* ListAggregatorsV2
* ListAutomationRules
* ListAutomationRulesV2
* ListConnectorsV2
* ListEnabledProductsForImport
* ListFindingAggregators
* ListInvitations
* ListMembers
* ListSecurityControlDefinitions
* ListStandardsControlAssociations
* ListTagsForResource
* StartConfigurationPolicyAssociation
* StartConfigurationPolicyDisassociation
* TagResource
* UntagResource
* UpdateActionTarget
* UpdateAggregatorV2
* UpdateAutomationRuleV2
* UpdateConfigurationPolicy
* UpdateConnectorV2
* UpdateFindingAggregator
* UpdateFindings
* UpdateInsight
* UpdateOrganizationConfiguration
* UpdateSecurityControl
* UpdateSecurityHubConfiguration
* UpdateStandardsControl
