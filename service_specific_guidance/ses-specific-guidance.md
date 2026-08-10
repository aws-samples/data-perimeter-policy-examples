# Service-specific guidance: Amazon Simple Email Service


This document outlines service-specific guidance for implementing a data perimeter for Amazon Simple Email Service.

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

## PutIdentityPolicy
### No RCP support

**Perimeter type applicability**: identity and network perimeter applied on resource.

**Description**: [PutIdentityPolicy](https://docs.aws.amazon.com/ses/latest/APIReference/API_PutIdentityPolicy.html) allows you to apply a resource-based policy to grant access to an identity. The service currently does not support RCPs.

**Additional controls**:

If you want to restrict access to trusted identities, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutIdentityPolicy](https://docs.aws.amazon.com/ses/latest/APIReference/API_PutIdentityPolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [PutIdentityPolicy](https://docs.aws.amazon.com/ses/latest/APIReference/API_PutIdentityPolicy.html) API calls in your environment (specifically, the [Policy](https://docs.aws.amazon.com/ses/latest/APIReference/API_PutIdentityPolicy.html#API_PutIdentityPolicy_RequestParameters) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutIdentityPolicy](https://docs.aws.amazon.com/ses/latest/APIReference/API_PutIdentityPolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [PutIdentityPolicy](https://docs.aws.amazon.com/ses/latest/APIReference/API_PutIdentityPolicy.html) API calls in your environment (specifically, the [Policy](https://docs.aws.amazon.com/ses/latest/APIReference/API_PutIdentityPolicy.html#API_PutIdentityPolicy_RequestParameters) request parameter). If necessary, remediate with the responsive controls of your choice.

## SendBulkTemplatedEmail
### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [SendBulkTemplatedEmail](https://docs.aws.amazon.com/ses/latest/APIReference/API_SendBulkTemplatedEmail.html) allows you to specify an email address as the value for the `Destinations` request parameter. Because the subsequent requests against the email address are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing this additional control:
* Preventative control example: Consider implementing `ses:Recipients` in an SCP to help prevent requests to untrusted resources. See [restrict_untrusted_endpoints_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_untrusted_endpoints_scp.json) for an example policy.

## SendCustomVerificationEmail
### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [SendCustomVerificationEmail](https://docs.aws.amazon.com/ses/latest/APIReference/API_SendCustomVerificationEmail.html) allows you to specify an email address as the value for the `EmailAddress` request parameter. Because the subsequent requests against the email address are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Preventative control example: Consider restricting [SendCustomVerificationEmail](https://docs.aws.amazon.com/ses/latest/APIReference/API_SendCustomVerificationEmail.html) permissions to select principals using an SCP to help prevent requests to untrusted resources. See [restrict_untrusted_endpoints_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_untrusted_endpoints_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [SendCustomVerificationEmail](https://docs.aws.amazon.com/ses/latest/APIReference/API_SendCustomVerificationEmail.html) API calls in your environment (specifically, the [EmailAddress](https://docs.aws.amazon.com/ses/latest/APIReference/API_SendCustomVerificationEmail.html#API_SendCustomVerificationEmail_RequestParameters) request parameter). If necessary, remediate with the responsive controls of your choice.

## SendEmail
### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [SendEmail](https://docs.aws.amazon.com/ses/latest/APIReference/API_SendEmail.html) allows you to specify an email address as the value for the `Destination` request parameter. Because the subsequent requests against the email address are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing this additional control:
* Preventative control example: Consider implementing `ses:Recipients` in an SCP to help prevent requests to untrusted resources. See [restrict_untrusted_endpoints_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_untrusted_endpoints_scp.json) for an example policy.

## SendRawEmail
### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [SendRawEmail](https://docs.aws.amazon.com/ses/latest/APIReference/API_SendRawEmail.html) allows you to specify an email address as the value for the `Destinations` request parameter. Because the subsequent requests against the email address are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing this additional control:
* Preventative control example: Consider implementing `ses:Recipients` in an SCP to help prevent requests to untrusted resources. See [restrict_untrusted_endpoints_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_untrusted_endpoints_scp.json) for an example policy.

## SendTemplatedEmail
### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [SendTemplatedEmail](https://docs.aws.amazon.com/ses/latest/APIReference/API_SendTemplatedEmail.html) allows you to specify an email address as the value for the `Destination` request parameter. Because the subsequent requests against the email address are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing this additional control:
* Preventative control example: Consider implementing `ses:Recipients` in an SCP to help prevent requests to untrusted resources. See [restrict_untrusted_endpoints_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_untrusted_endpoints_scp.json) for an example policy.

## List of service APIs reviewed against data perimeter control objectives

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
