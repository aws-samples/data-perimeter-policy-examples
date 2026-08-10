# Service-specific guidance: AWS Directory Service


This document outlines service-specific guidance for implementing a data perimeter for AWS Directory Service.

AWS Directory Service provides multiple ways to use Microsoft Active Directory (AD) with other AWS services. Directories store information about users, groups, and devices, and administrators use them to manage access to information and resources.

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

## AcceptSharedDirectory
### No ResourceOrgID support

**Perimeter type applicability**: resource perimeter on identity.

**Description**: [AcceptSharedDirectory](https://docs.aws.amazon.com/directoryservice/latest/devguide/API_AcceptSharedDirectory.html) does not currently support the `aws:ResourceOrgID` condition key for the `SharedDirectoryId` request parameter.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Preventative control example: Consider restricting [AcceptSharedDirectory](https://docs.aws.amazon.com/directoryservice/latest/devguide/API_AcceptSharedDirectory.html) permissions to select principals using an SCP. See [data_perimeter_governance_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/data_perimeter_governance_scp.json) for an example policy and ["Sid":"ProtectActionsNotSupportedByPrimaryDPControls"](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies#sidprotectactionsnotsupportedbyprimarydpcontrols) for other APIs that currently don't support the `aws:ResourceOrgID` condition key.
* Detective control example: Consider using CloudTrail management events to monitor the [AcceptSharedDirectory](https://docs.aws.amazon.com/directoryservice/latest/devguide/API_AcceptSharedDirectory.html) API calls in your environment (specifically, the [SharedDirectoryId](https://docs.aws.amazon.com/directoryservice/latest/devguide/API_AcceptSharedDirectory.html#DirectoryService-AcceptSharedDirectory-request-SharedDirectoryId) request parameter). If necessary, remediate with the responsive controls of your choice.

## ShareDirectory
### Service sharing mechanism

**Perimeter type applicability**: identity perimeter applied on resource; resource perimeter applied on identity.

**Description**: [ShareDirectory](https://docs.aws.amazon.com/directoryservice/latest/devguide/API_ShareDirectory.html) allows you to share a directory with another account.

**Additional controls**:

If you want to restrict access so that only trusted identities can view information about your resources, consider implementing these additional controls:
* Preventative control example: Consider restricting [ShareDirectory](https://docs.aws.amazon.com/directoryservice/latest/devguide/API_ShareDirectory.html) permissions to select principals using an SCP. See [data_perimeter_governance_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/data_perimeter_governance_scp.json) for an example policy and ["Sid":"PreventExternalResourceShare"](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies#sidpreventexternalresourceshare) for a list of resources that can be granted cross-account access.
* Detective control example: Consider using CloudTrail management events to monitor the [ShareDirectory](https://docs.aws.amazon.com/directoryservice/latest/devguide/API_ShareDirectory.html) API calls in your environment (specifically, the [ShareTarget](https://docs.aws.amazon.com/directoryservice/latest/devguide/API_ShareTarget.html) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access so that your identities cannot view resources that were shared with your accounts by untrusted entities, consider implementing this additional control:
* Detective control example: Consider using [DescribeSharedDirectories](https://docs.aws.amazon.com/directoryservice/latest/devguide/API_DescribeSharedDirectories.html) to monitor the AWS Managed Microsoft AD directories shared with your accounts (specifically, the [SharedDirectories](https://docs.aws.amazon.com/directoryservice/latest/devguide/API_DescribeSharedDirectories.html#API_DescribeSharedDirectories_ResponseSyntax) response parameter). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

* AcceptSharedDirectory
* AddIpRoutes
* AddRegion
* AddTagsToResource
* CancelSchemaExtension
* ConnectDirectory
* CreateAlias
* CreateComputer
* CreateConditionalForwarder
* CreateDirectory
* CreateHybridAD
* CreateLogSubscription
* CreateMicrosoftAD
* CreateSnapshot
* CreateTrust
* DeleteADAssessment
* DeleteConditionalForwarder
* DeleteDirectory
* DeleteLogSubscription
* DeleteSnapshot
* DeleteTrust
* DeregisterCertificate
* DeregisterEventTopic
* DescribeADAssessment
* DescribeCAEnrollmentPolicy
* DescribeCertificate
* DescribeClientAuthenticationSettings
* DescribeConditionalForwarders
* DescribeDirectories
* DescribeDirectoryDataAccess
* DescribeDomainControllers
* DescribeEventTopics
* DescribeHybridADUpdate
* DescribeLDAPSSettings
* DescribeRegions
* DescribeSettings
* DescribeSharedDirectories
* DescribeSnapshots
* DescribeTrusts
* DescribeUpdateDirectory
* DisableCAEnrollmentPolicy
* DisableClientAuthentication
* DisableDirectoryDataAccess
* DisableLDAPS
* DisableRadius
* DisableSso
* EnableCAEnrollmentPolicy
* EnableClientAuthentication
* EnableDirectoryDataAccess
* EnableLDAPS
* EnableRadius
* EnableSso
* GetDirectoryLimits
* GetSnapshotLimits
* ListADAssessments
* ListCertificates
* ListIpRoutes
* ListLogSubscriptions
* ListSchemaExtensions
* ListTagsForResource
* RegisterCertificate
* RegisterEventTopic
* RejectSharedDirectory
* RemoveIpRoutes
* RemoveRegion
* RemoveTagsFromResource
* ResetUserPassword
* RestoreFromSnapshot
* ShareDirectory
* StartADAssessment
* StartSchemaExtension
* UnshareDirectory
* UpdateConditionalForwarder
* UpdateDirectorySetup
* UpdateHybridAD
* UpdateNumberOfDomainControllers
* UpdateRadius
* UpdateSettings
* UpdateTrust
* VerifyTrust
