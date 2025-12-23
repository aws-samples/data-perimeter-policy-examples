# Service-specific guidance: Amazon Simple Email Service


This document outlines service-specific guidance for implementing a data perimeter for Amazon Simple Email Service. 


Amazon Simple Email Service (SES) is a cloud-based email sending service that enables you to send transactional email, marketing messages, or any other type of high-quality content to your customers. SES provides a reliable, cost-effective way to send and receive email using your own email addresses and domains, without the need to maintain your own email servers.


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
 

> Amazon SES has two versions of APIs. See also the service-specific guidance for [the version 2 of the Amazon SES API](../sesv2/sesv2-specific-guidance.md).


**Additional consideration 1**

Perimeter type applicability: resource perimeter applied on identity.
        
SendBulkTemplatedEmail allows you to send an email message to multiple emails. Because the subsequent requests are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing this additional control:

* **Preventative control example:** Consider implementing [ses:Recipients](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonsimpleemailservicev2.html#amazonsimpleemailservicev2-ses_Recipients) in an SCP to help prevent requests to untrusted resources. See [restrict_untrusted_endpoints_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies/service_specific_controls/restrict_untrusted_endpoints_scp.json) for an example policy.

**Additional consideration 2**

Perimeter type applicability: resource perimeter applied on identity.
        
SendEmail allows you to send an email message. Because the subsequent requests are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing this additional control:

* **Preventative control example:** Consider implementing [ses:Recipients](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonsimpleemailservicev2.html#amazonsimpleemailservicev2-ses_Recipients) in an SCP to help prevent requests to untrusted resources. See [restrict_untrusted_endpoints_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies/service_specific_controls/restrict_untrusted_endpoints_scp.json) for an example policy.


**Additional consideration 3**

Perimeter type applicability: resource perimeter applied on identity.
        
SendRawEmail allows you to send an email message. Because the subsequent requests are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing this additional control:

* **Preventative control example:** Consider implementing [ses:Recipients](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonsimpleemailservicev2.html#amazonsimpleemailservicev2-ses_Recipients) in an SCP to help prevent requests to untrusted resources. See [restrict_untrusted_endpoints_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies/service_specific_controls/restrict_untrusted_endpoints_scp.json) for an example policy.


**Additional consideration 4**

Perimeter type applicability: resource perimeter applied on identity.
        
SendTemplatedEmail allows you to send an email message using a template. Because the subsequent requests are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing this additional control:

* **Preventative control example:** Consider implementing [ses:Recipients](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonsimpleemailservicev2.html#amazonsimpleemailservicev2-ses_Recipients) in an SCP to help prevent requests to untrusted resources. See [restrict_untrusted_endpoints_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies/service_specific_controls/restrict_untrusted_endpoints_scp.json) for an example policy.


**Additional consideration 5**

Perimeter type applicability: resource perimeter applied on identity.
        
SendCustomVerificationEmail allows you to send a verification email message using a template. Because the subsequent requests are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing this additional control:

* **Preventative control example:** Consider restricting [SendCustomVerificationEmail](https://docs.aws.amazon.com/ses/latest/APIReference/API_SendCustomVerificationEmail.html) permissions to administrators only using an SCP. See [restrict_untrusted_endpoints_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies/service_specific_controls/restrict_untrusted_endpoints_scp.json) for an example policy.



**List of service APIs reviewed against data perimeter control objectives**
* CloneReceiptRuleSet
* CreateConfigurationSet
* CreateConfigurationSetEventDestination
* CreateConfigurationSetTrackingOptions
* CreateCustomVerificationEmailTemplate
* CreateReceiptFilter
* CreateReceiptRule
* CreateReceiptRuleSet
* CreateTemplate
* DeleteConfigurationSet
* DeleteConfigurationSetEventDestination
* DeleteConfigurationSetTrackingOptions
* DeleteCustomVerificationEmailTemplate
* DeleteIdentity
* DeleteIdentityPolicy
* DeleteReceiptFilter
* DeleteReceiptRule
* DeleteReceiptRuleSet
* DeleteTemplate
* DescribeActiveReceiptRuleSet
* DescribeConfigurationSet
* DescribeReceiptRule
* DescribeReceiptRuleSet
* GetAccountSendingEnabled
* GetCustomVerificationEmailTemplate
* GetIdentityDkimAttributes
* GetIdentityMailFromDomainAttributes
* GetIdentityNotificationAttributes
* GetIdentityPolicies
* GetIdentityVerificationAttributes
* GetSendQuota
* GetSendStatistics
* GetTemplate
* ListConfigurationSets
* ListCustomVerificationEmailTemplates
* ListIdentities
* ListIdentityPolicies
* ListReceiptFilters
* ListReceiptRuleSets
* ListTemplates
* ListVerifiedEmailAddresses
* PutConfigurationSetDeliveryOptions
* PutIdentityPolicy
* ReorderReceiptRuleSet
* SendBulkTemplatedEmail
* SendCustomVerificationEmail
* SendEmail
* SendRawEmail
* SendTemplatedEmail
* SetActiveReceiptRuleSet
* SetIdentityDkimEnabled
* SetIdentityFeedbackForwardingEnabled
* SetIdentityHeadersInNotificationsEnabled
* SetIdentityMailFromDomain
* SetIdentityNotificationTopic
* SetReceiptRulePosition
* TestRenderTemplate
* UpdateConfigurationSetEventDestination
* UpdateConfigurationSetReputationMetricsEnabled
* UpdateConfigurationSetSendingEnabled
* UpdateConfigurationSetTrackingOptions
* UpdateCustomVerificationEmailTemplate
* UpdateReceiptRule
* UpdateTemplate
* VerifyDomainDkim
* VerifyDomainIdentity
* VerifyEmailIdentity
