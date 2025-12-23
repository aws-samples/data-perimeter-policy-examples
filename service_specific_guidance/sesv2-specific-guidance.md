# Service-specific guidance: Amazon Simple Email Service V2


This document outlines service-specific guidance for implementing a data perimeter for Amazon Simple Email Service V2. 


Amazon Simple Email Service (SES) V2 is a cloud-based email sending service that enables you to send marketing, notification, and transactional emails. It provides a reliable, scalable, and cost-effective way for businesses and developers to send and receive emails using their own email addresses and domains. SES V2 offers advanced features like improved deliverability, detailed sending statistics, and enhanced security controls.


The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type | Security objective | Applied on | Policy type | Additional considerations |
|----------------|-------------------|------------|-------------|------------------------|
| Identity perimeter | Only trusted identities can access my resources | Resource | RCP | N |
| Identity perimeter | Only trusted identities are allowed from my network | Network | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources | Identity | SCP | Y |
| Resource perimeter | Only trusted resources can be accessed from my network | Network | VPC endpoint policy | N |
| Network perimeter | My identities can access resources only from expected networks | Identity | SCP | N |
| Network perimeter | My resources can be accessed only from expected networks | Resource | RCP | N |

*Y – Additional considerations apply. N – No additional considerations apply.
 

> Amazon SES has two versions of APIs. See also the service-specific guidance for [the version 1 of the Amazon SES API](../ses/ses-specific-guidance.md).


**Additional consideration 1**

Perimeter type applicability: resource perimeter applied on identity.
        
SendEmail allows you to send an email message. Because the subsequent requests are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing this additional control:

* **Preventative control example:** Consider implementing [ses:Recipients](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonsimpleemailservicev2.html#amazonsimpleemailservicev2-ses_Recipients) in an SCP to help prevent requests to untrusted resources. See [restrict_untrusted_endpoints_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies/service_specific_controls/restrict_untrusted_endpoints_scp.json) for an example policy.


**Additional consideration 2**

Perimeter type applicability: resource perimeter applied on identity.
        
SendCustomVerificationEmail allows you to send a verification email message using a template. Because the subsequent requests are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing this additional control:

* **Preventative control example:** Consider restricting [SendCustomVerificationEmail](https://docs.aws.amazon.com/ses/latest/APIReference/API_SendCustomVerificationEmail.html) permissions to administrators only using an SCP. See [restrict_untrusted_endpoints_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies/service_specific_controls/restrict_untrusted_endpoints_scp.json) for an example policy.




**List of service APIs reviewed against data perimeter control objectives**
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
* DeleteConfigurationSet
* DeleteConfigurationSetEventDestination
* DeleteContact
* DeleteContactList
* DeleteCustomVerificationEmailTemplate
* DeleteDedicatedIpPool
* DeleteEmailIdentity
* DeleteEmailIdentityPolicy
* DeleteEmailTemplate
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
* ListRecommendations
* ListSuppressedDestinations
* ListTagsForResource
* PutAccountDedicatedIpWarmupAttributes
* PutAccountSendingAttributes
* PutAccountSuppressionAttributes
* PutAccountVdmAttributes
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
