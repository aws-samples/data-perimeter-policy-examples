# Service-specific guidance: Amazon Connect Outbound Campaigns


This document outlines service-specific guidance for implementing a data perimeter for Amazon Connect Outbound Campaigns.

Amazon Connect Outbound Campaigns is the outbound campaigns feature of Amazon Connect (Connect Customer). It lets you create high-volume outbound campaigns that reach customers proactively over voice, SMS, email, and WhatsApp, for uses such as appointment reminders, telemarketing, subscription renewals, and debt collection.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | N |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | N |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | N |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | N |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | N |

*Y - Additional considerations apply. N - No additional considerations apply.

## List of service APIs reviewed against data perimeter control objectives

The service APIs covered in this review are based on botocore version [1.43.29](https://github.com/boto/botocore/tree/1.43.29/botocore).

* CreateCampaign
* DeleteCampaign
* DeleteConnectInstanceConfig
* DescribeCampaign
* GetCampaignState
* GetCampaignStateBatch
* GetConnectInstanceConfig
* GetInstanceOnboardingJobStatus
* ListCampaigns
* ListTagsForResource
* PauseCampaign
* PutDialRequestBatch
* ResumeCampaign
* StartCampaign
* StartInstanceOnboardingJob
* StopCampaign
* TagResource
* UntagResource
* UpdateCampaignDialerConfig
* UpdateCampaignName
* UpdateCampaignOutboundCallConfig
