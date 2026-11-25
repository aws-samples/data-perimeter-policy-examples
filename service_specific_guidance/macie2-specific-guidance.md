# Service-specific guidance: Amazon Macie


This document outlines service-specific guidance for implementing a data perimeter for Amazon Macie.

Amazon Macie is a data security service that discovers sensitive data by using machine learning and pattern matching, provides visibility into data security risks, and enables automated protection against those risks. Macie keeps an inventory of your Amazon S3 general purpose buckets, automatically evaluates and monitors them for security and access control, and generates findings when it detects sensitive data in S3 objects or potential security or privacy issues with buckets.

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

## AcceptInvitation
### No ResourceOrgID support

**Perimeter type applicability**: resource perimeter on identity.

**Description**: [AcceptInvitation](https://docs.aws.amazon.com/macie/latest/APIReference/invitations-accept.html) does not currently support the `aws:ResourceOrgID` condition key for the `invitationId` request parameter.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Preventative control example: Consider restricting [AcceptInvitation](https://docs.aws.amazon.com/macie/latest/APIReference/invitations-accept.html) permissions to select principals using an SCP. See [data_perimeter_governance_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/data_perimeter_governance_scp.json) for an example policy and ["Sid":"ProtectActionsNotSupportedByPrimaryDPControls"](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies#sidprotectactionsnotsupportedbyprimarydpcontrols) for other APIs that currently don't support the `aws:ResourceOrgID` condition key.
* Detective control example: Consider using CloudTrail management events to monitor the [AcceptInvitation](https://docs.aws.amazon.com/macie/latest/APIReference/invitations-accept.html) API calls in your environment (specifically, the [invitationId](https://docs.aws.amazon.com/macie/latest/APIReference/invitations-accept.html#invitations-accept-model-acceptinvitationrequest) request parameter). If necessary, remediate with the responsive controls of your choice.

## CreateInvitations
### Service sharing mechanism

**Perimeter type applicability**: identity perimeter applied on resource; resource perimeter applied on identity.

**Description**: [CreateInvitations](https://docs.aws.amazon.com/macie/latest/APIReference/invitations.html) allows you to invite another account to your administrator account.

**Additional controls**:

If you want to restrict access so that only trusted identities can view information about your resources, consider implementing these additional controls:
* Preventative control example: Consider restricting [CreateInvitations](https://docs.aws.amazon.com/macie/latest/APIReference/invitations.html) permissions to select principals using an SCP. See [data_perimeter_governance_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/data_perimeter_governance_scp.json) for an example policy and ["Sid":"PreventExternalResourceShare"](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies#sidpreventexternalresourceshare) for a list of resources that can be granted cross-account access.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateInvitations](https://docs.aws.amazon.com/macie/latest/APIReference/invitations.html) API calls in your environment (specifically, the [accountIds](https://docs.aws.amazon.com/macie/latest/APIReference/invitations.html#invitations-model-createinvitationsrequest) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access so that your identities cannot view resources that were shared with your accounts by untrusted entities, consider implementing this additional control:
* Detective control example: Consider using [ListInvitations](https://docs.aws.amazon.com/macie/latest/APIReference/invitations.html#invitationsget) to monitor the Amazon Macie membership invitations created for your accounts (specifically, the [invitations](https://docs.aws.amazon.com/macie/latest/APIReference/invitations.html#invitations-model-invitation) response parameter). If necessary, remediate with the responsive controls of your choice.

## CreateMember
### Service sharing mechanism

**Perimeter type applicability**: identity perimeter applied on resource; resource perimeter applied on identity.

**Description**: [CreateMember](https://docs.aws.amazon.com/macie/latest/APIReference/members.html) allows you to add an account as a member association in your administrator account.

**Additional controls**:

If you want to restrict access so that only trusted identities can view information about your resources, consider implementing these additional controls:
* Preventative control example: Consider restricting [CreateMember](https://docs.aws.amazon.com/macie/latest/APIReference/members.html) permissions to select principals using an SCP. See [data_perimeter_governance_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/data_perimeter_governance_scp.json) for an example policy and ["Sid":"PreventExternalResourceShare"](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies#sidpreventexternalresourceshare) for a list of resources that can be granted cross-account access.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [AccountId](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-macie-member.html#cfn-macie-member-accountid) property that specifies an untrusted account for the [AWS::Macie::Member](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-macie-member.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateMember](https://docs.aws.amazon.com/macie/latest/APIReference/members.html) API calls in your environment (specifically, the [account](https://docs.aws.amazon.com/macie/latest/APIReference/members.html#members-model-accountdetail) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access so that your identities cannot view resources that were shared with your accounts by untrusted entities, consider implementing this additional control:
* Detective control example: Consider using [ListInvitations](https://docs.aws.amazon.com/macie/latest/APIReference/invitations.html#invitationsget) to monitor the Amazon Macie membership invitations created for your accounts (specifically, the [invitations](https://docs.aws.amazon.com/macie/latest/APIReference/invitations.html#invitations-model-invitation) response parameter). If necessary, remediate with the responsive controls of your choice.

## PutClassificationExportConfiguration
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [PutClassificationExportConfiguration](https://docs.aws.amazon.com/macie/latest/APIReference/classification-export-configuration.html#classification-export-configurationput) allows you to specify an S3 bucket that does not belong to your organization as the value for the `configuration.s3Destination.bucketName` request parameter. Because the subsequent call against the S3 bucket is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing this additional control:
* Detective control example: Consider using CloudTrail management events to monitor the [PutClassificationExportConfiguration](https://docs.aws.amazon.com/macie/latest/APIReference/classification-export-configuration.html#classification-export-configurationput) API calls in your environment (specifically, the [configuration.s3Destination.bucketName](https://docs.aws.amazon.com/macie/latest/APIReference/classification-export-configuration.html#classification-export-configuration-model-s3destination) request parameter). If necessary, remediate with the responsive controls of your choice.

### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [PutClassificationExportConfiguration](https://docs.aws.amazon.com/macie/latest/APIReference/classification-export-configuration.html#classification-export-configurationput) allows you to specify a KMS key that does not belong to your organization as the value for the `configuration.s3Destination.kmsKeyArn` request parameter. Because the subsequent call against the KMS key is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing this additional control:
* Detective control example: Consider using CloudTrail management events to monitor the [PutClassificationExportConfiguration](https://docs.aws.amazon.com/macie/latest/APIReference/classification-export-configuration.html#classification-export-configurationput) API calls in your environment (specifically, the [configuration.s3Destination.kmsKeyArn](https://docs.aws.amazon.com/macie/latest/APIReference/classification-export-configuration.html#classification-export-configuration-model-s3destination) request parameter). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

The service APIs covered in this review are based on botocore version [1.43.29](https://github.com/boto/botocore/tree/1.43.29/botocore).

* AcceptInvitation
* BatchGetCustomDataIdentifiers
* BatchUpdateAutomatedDiscoveryAccounts
* CreateAllowList
* CreateClassificationJob
* CreateCustomDataIdentifier
* CreateFindingsFilter
* CreateInvitations
* CreateMember
* CreateSampleFindings
* DeclineInvitations
* DeleteAllowList
* DeleteCustomDataIdentifier
* DeleteFindingsFilter
* DeleteInvitations
* DeleteMember
* DescribeBuckets
* DescribeClassificationJob
* DescribeOrganizationConfiguration
* DisassociateMember
* GetAllowList
* GetAutomatedDiscoveryConfiguration
* GetBucketStatistics
* GetClassificationExportConfiguration
* GetClassificationScope
* GetCustomDataIdentifier
* GetFindingStatistics
* GetFindings
* GetFindingsFilter
* GetFindingsPublicationConfiguration
* GetInvitationsCount
* GetMacieSession
* GetMember
* GetResourceProfile
* GetRevealConfiguration
* GetSensitiveDataOccurrencesAvailability
* GetSensitivityInspectionTemplate
* GetUsageStatistics
* GetUsageTotals
* ListAllowLists
* ListAutomatedDiscoveryAccounts
* ListClassificationJobs
* ListClassificationScopes
* ListCustomDataIdentifiers
* ListFindings
* ListFindingsFilters
* ListInvitations
* ListManagedDataIdentifiers
* ListMembers
* ListResourceProfileArtifacts
* ListResourceProfileDetections
* ListSensitivityInspectionTemplates
* ListTagsForResource
* PutClassificationExportConfiguration
* PutFindingsPublicationConfiguration
* SearchResources
* TagResource
* TestCustomDataIdentifier
* UntagResource
* UpdateAllowList
* UpdateAutomatedDiscoveryConfiguration
* UpdateClassificationJob
* UpdateClassificationScope
* UpdateFindingsFilter
* UpdateMacieSession
* UpdateMemberSession
* UpdateOrganizationConfiguration
* UpdateResourceProfile
* UpdateResourceProfileDetections
* UpdateRevealConfiguration
* UpdateSensitivityInspectionTemplate
