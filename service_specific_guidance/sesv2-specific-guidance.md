# Service-specific guidance: Amazon Simple Email Service V2


This document outlines service-specific guidance for implementing a data perimeter for Amazon SES V2.

Amazon Simple Email Service (SES) is an email platform that provides an easy, cost-effective way for you to send and receive email using your own email addresses and domains.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | Y |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | Y |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | N |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | N |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | Y |

*Y - Additional considerations apply. N - No additional considerations apply.

## CreateEmailIdentityPolicy
### No RCP support

**Perimeter type applicability**: identity and network perimeter applied on resource.

**Description**: [CreateEmailIdentityPolicy](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_CreateEmailIdentityPolicy.html) allows you to apply a resource-based policy to grant access to an email identity. The service currently does not support RCPs.

**Additional controls**:

If you want to restrict access to trusted identities, consider implementing these additional controls:
* Preventative control example: Consider restricting [CreateEmailIdentityPolicy](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_CreateEmailIdentityPolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateEmailIdentityPolicy](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_CreateEmailIdentityPolicy.html) API calls in your environment (specifically, the [Policy](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_CreateEmailIdentityPolicy.html#SES-CreateEmailIdentityPolicy-request-Policy) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [CreateEmailIdentityPolicy](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_CreateEmailIdentityPolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateEmailIdentityPolicy](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_CreateEmailIdentityPolicy.html) API calls in your environment (specifically, the [Policy](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_CreateEmailIdentityPolicy.html#SES-CreateEmailIdentityPolicy-request-Policy) request parameter). If necessary, remediate with the responsive controls of your choice.

## SendCustomVerificationEmail
### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [SendCustomVerificationEmail](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_SendCustomVerificationEmail.html) allows you to specify an email address as the value for the `EmailAddress` request parameter. Because the subsequent requests against the email address are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Preventative control example: Consider restricting [SendCustomVerificationEmail](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_SendCustomVerificationEmail.html) permissions to select principals using an SCP to help prevent requests to untrusted resources. See [restrict_untrusted_endpoints_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_untrusted_endpoints_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [SendCustomVerificationEmail](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_SendCustomVerificationEmail.html) API calls in your environment (specifically, the [EmailAddress](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_SendCustomVerificationEmail.html#SES-SendCustomVerificationEmail-request-EmailAddress) request parameter). If necessary, remediate with the responsive controls of your choice.

## SendEmail
### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [SendEmail](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_SendEmail.html) allows you to specify an email address as the value for the `Destination` request parameter. Because the subsequent requests against the email address are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing this additional control:
* Preventative control example: Consider implementing `ses:Recipients` in an SCP to help prevent requests to untrusted resources. See [restrict_untrusted_endpoints_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_untrusted_endpoints_scp.json) for an example policy.

## UpdateEmailIdentityPolicy
### No RCP support

**Perimeter type applicability**: identity and network perimeter applied on resource.

**Description**: [UpdateEmailIdentityPolicy](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_UpdateEmailIdentityPolicy.html) allows you to apply a resource-based policy to grant access to an email identity. The service currently does not support RCPs.

**Additional controls**:

If you want to restrict access to trusted identities, consider implementing these additional controls:
* Preventative control example: Consider restricting [UpdateEmailIdentityPolicy](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_UpdateEmailIdentityPolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateEmailIdentityPolicy](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_UpdateEmailIdentityPolicy.html) API calls in your environment (specifically, the [Policy](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_UpdateEmailIdentityPolicy.html#SES-UpdateEmailIdentityPolicy-request-Policy) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [UpdateEmailIdentityPolicy](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_UpdateEmailIdentityPolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateEmailIdentityPolicy](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_UpdateEmailIdentityPolicy.html) API calls in your environment (specifically, the [Policy](https://docs.aws.amazon.com/ses/latest/APIReference-V2/API_UpdateEmailIdentityPolicy.html#SES-UpdateEmailIdentityPolicy-request-Policy) request parameter). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

* CreateConfigurationSet
* CreateConfigurationSetEventDestination
* CreateContact
* CreateContactList
* CreateCustomVerificationEmailTemplate
* CreateDedicatedIpPool
* CreateEmailIdentity
* CreateEmailIdentityPolicy
* CreateEmailTemplate
* CreateExportJob
* CreateImportJob
* CreateMultiRegionEndpoint
* CreateTenant
* CreateTenantResourceAssociation
* DeleteConfigurationSet
* DeleteConfigurationSetEventDestination
* DeleteContact
* DeleteContactList
* DeleteCustomVerificationEmailTemplate
* DeleteDedicatedIpPool
* DeleteEmailIdentity
* DeleteEmailIdentityPolicy
* DeleteEmailTemplate
* DeleteMultiRegionEndpoint
* DeleteTenant
* DeleteTenantResourceAssociation
* GetAccount
* GetBlacklistReports
* GetConfigurationSet
* GetConfigurationSetEventDestinations
* GetContact
* GetContactList
* GetCustomVerificationEmailTemplate
* GetDedicatedIpPool
* GetDedicatedIps
* GetDeliverabilityDashboardOptions
* GetEmailIdentity
* GetEmailIdentityPolicies
* GetEmailTemplate
* GetExportJob
* GetImportJob
* GetMultiRegionEndpoint
* GetReputationEntity
* GetTenant
* ListConfigurationSets
* ListContactLists
* ListContacts
* ListCustomVerificationEmailTemplates
* ListDedicatedIpPools
* ListDeliverabilityTestReports
* ListEmailIdentities
* ListEmailTemplates
* ListExportJobs
* ListImportJobs
* ListMultiRegionEndpoints
* ListRecommendations
* ListReputationEntities
* ListResourceTenants
* ListSuppressedDestinations
* ListTagsForResource
* ListTenantResources
* ListTenants
* PutAccountDedicatedIpWarmupAttributes
* PutAccountSendingAttributes
* PutAccountSuppressionAttributes
* PutAccountVdmAttributes
* PutConfigurationSetArchivingOptions
* PutConfigurationSetDeliveryOptions
* PutConfigurationSetReputationOptions
* PutConfigurationSetSendingOptions
* PutConfigurationSetSuppressionOptions
* PutConfigurationSetTrackingOptions
* PutConfigurationSetVdmOptions
* PutDedicatedIpPoolScalingAttributes
* PutDeliverabilityDashboardOption
* PutEmailIdentityConfigurationSetAttributes
* PutEmailIdentityDkimAttributes
* PutEmailIdentityFeedbackAttributes
* PutEmailIdentityMailFromAttributes
* SendBulkEmail
* SendCustomVerificationEmail
* SendEmail
* TagResource
* TestRenderEmailTemplate
* UntagResource
* UpdateConfigurationSetEventDestination
* UpdateContact
* UpdateContactList
* UpdateCustomVerificationEmailTemplate
* UpdateEmailIdentityPolicy
* UpdateEmailTemplate
* UpdateReputationEntityCustomerManagedStatus
* UpdateReputationEntityPolicy
