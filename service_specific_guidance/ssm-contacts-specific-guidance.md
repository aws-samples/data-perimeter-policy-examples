# Service-specific guidance: AWS Systems Manager Incident Manager Contacts


This document outlines service-specific guidance for implementing a data perimeter for AWS Systems Manager Incident Manager Contacts. 


AWS Systems Manager Incident Manager Contacts is a feature within AWS Systems Manager that helps you manage and organize contact information for individuals and teams involved in incident response. It allows you to store, update, and quickly access contact details, enabling efficient communication and escalation during incidents or emergencies.


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
 






**Additional consideration 1**

Perimeter type applicability: resource perimeter applied on identity.
        
UpdateContactChannel allows you to specify contact channels, such as Email, Short Message Service (SMS), and Voice, as the value for the DeliveryAddress request parameter. Because the subsequent requests are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls:


* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [ChannelAddress](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-ssmcontacts-contactchannel.html#cfn-ssmcontacts-contactchannel-channeladdress) property that does not belong to your organization for the [AWS::SSMContacts::ContactChannel](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-ssmcontacts-contactchannel.html) resource.

* **Detective control example:** Consider using CloudTrail management events to monitor the [UpdateContactChannel](https://docs.aws.amazon.com/incident-manager/latest/APIReference/API_SSMContacts_UpdateContactChannel.html) API calls in your environment (specifically, the DeliveryAddress request parameter). If necessary, remediate with the responsive controls of your choice.


**Additional consideration 2**

Perimeter type applicability: resource perimeter applied on identity.
        
CreateContactChannel allows you to specify contact channels, such as Email, Short Message Service (SMS), and Voice, as the value for the DeliveryAddress request parameter. Because the subsequent requests are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls:


* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [ChannelAddress](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-ssmcontacts-contactchannel.html#cfn-ssmcontacts-contactchannel-channeladdress) property that does not belong to your organization for the [AWS::SSMContacts::ContactChannel](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-ssmcontacts-contactchannel.html) resource.

* **Detective control example:** Consider using CloudTrail management events to monitor the [CreateContactChannel](https://docs.aws.amazon.com/incident-manager/latest/APIReference/API_SSMContacts_CreateContactChannel.html) API calls in your environment (specifically, the DeliveryAddress request parameter). If necessary, remediate with the responsive controls of your choice.





**List of service APIs reviewed against data perimeter control objectives**
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
