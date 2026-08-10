# Service-specific guidance: AWS Systems Manager Incident Manager Contacts


This document outlines service-specific guidance for implementing a data perimeter for AWS Systems Manager Incident Manager Contacts.

AWS Systems Manager Incident Manager Contacts lets you define responders (contacts) that Incident Manager engages during an incident. A contact can have multiple contact channels (such as email, SMS, and voice) and an engagement plan that describes how and when Incident Manager engages the contact.

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

## CreateContactChannel
### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [CreateContactChannel](https://docs.aws.amazon.com/incident-manager/latest/APIReference/API_SSMContacts_CreateContactChannel.html) allows you to specify an email address, SMS phone number, and voice contact channel as the value for the `DeliveryAddress` request parameter. Because the subsequent requests against the contact channels are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [ChannelAddress](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ssmcontacts-contactchannel.html#cfn-ssmcontacts-contactchannel-channeladdress) property that does not belong to your organization for the [AWS::SSMContacts::ContactChannel](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ssmcontacts-contactchannel.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateContactChannel](https://docs.aws.amazon.com/incident-manager/latest/APIReference/API_SSMContacts_CreateContactChannel.html) API calls in your environment (specifically, the [DeliveryAddress](https://docs.aws.amazon.com/incident-manager/latest/APIReference/API_SSMContacts_CreateContactChannel.html#IncidentManager-SSMContacts_CreateContactChannel-request-DeliveryAddress) request parameter). If necessary, remediate with the responsive controls of your choice.

## PutContactPolicy
### No RCP support

**Perimeter type applicability**: identity and network perimeter applied on resource.

**Description**: [PutContactPolicy](https://docs.aws.amazon.com/incident-manager/latest/APIReference/API_SSMContacts_PutContactPolicy.html) allows you to apply a resource-based policy to grant access to a contact or escalation plan. The service currently does not support RCPs.

**Additional controls**:

If you want to restrict access to trusted identities, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutContactPolicy](https://docs.aws.amazon.com/incident-manager/latest/APIReference/API_SSMContacts_PutContactPolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [PutContactPolicy](https://docs.aws.amazon.com/incident-manager/latest/APIReference/API_SSMContacts_PutContactPolicy.html) API calls in your environment (specifically, the [Policy](https://docs.aws.amazon.com/incident-manager/latest/APIReference/API_SSMContacts_PutContactPolicy.html#IncidentManager-SSMContacts_PutContactPolicy-request-Policy) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutContactPolicy](https://docs.aws.amazon.com/incident-manager/latest/APIReference/API_SSMContacts_PutContactPolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [PutContactPolicy](https://docs.aws.amazon.com/incident-manager/latest/APIReference/API_SSMContacts_PutContactPolicy.html) API calls in your environment (specifically, the [Policy](https://docs.aws.amazon.com/incident-manager/latest/APIReference/API_SSMContacts_PutContactPolicy.html#IncidentManager-SSMContacts_PutContactPolicy-request-Policy) request parameter). If necessary, remediate with the responsive controls of your choice.

## UpdateContactChannel
### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [UpdateContactChannel](https://docs.aws.amazon.com/incident-manager/latest/APIReference/API_SSMContacts_UpdateContactChannel.html) allows you to specify an email address, SMS phone number, and voice contact channel as the value for the `DeliveryAddress` request parameter. Because the subsequent requests against the contact channels are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [ChannelAddress](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ssmcontacts-contactchannel.html#cfn-ssmcontacts-contactchannel-channeladdress) property that does not belong to your organization for the [AWS::SSMContacts::ContactChannel](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ssmcontacts-contactchannel.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateContactChannel](https://docs.aws.amazon.com/incident-manager/latest/APIReference/API_SSMContacts_UpdateContactChannel.html) API calls in your environment (specifically, the [DeliveryAddress](https://docs.aws.amazon.com/incident-manager/latest/APIReference/API_SSMContacts_UpdateContactChannel.html#IncidentManager-SSMContacts_UpdateContactChannel-request-DeliveryAddress) request parameter). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

* AcceptPage
* CreateContact
* CreateContactChannel
* CreateRotation
* CreateRotationOverride
* DeactivateContactChannel
* DeleteContact
* DeleteContactChannel
* DeleteRotation
* DeleteRotationOverride
* DescribeEngagement
* DescribePage
* GetContact
* GetContactChannel
* GetContactPolicy
* GetRotation
* GetRotationOverride
* ListContactChannels
* ListContacts
* ListEngagements
* ListPageReceipts
* ListPageResolutions
* ListPagesByContact
* ListPagesByEngagement
* ListPreviewRotationShifts
* ListRotationOverrides
* ListRotationShifts
* ListRotations
* ListTagsForResource
* PutContactPolicy
* SendActivationCode
* StartEngagement
* StopEngagement
* TagResource
* UntagResource
* UpdateContact
* UpdateContactChannel
* UpdateRotation
