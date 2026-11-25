# Service-specific guidance: Amazon Connect Customer Profiles


This document outlines service-specific guidance for implementing a data perimeter for Amazon Connect Customer Profiles.

Amazon Connect Customer Profiles gives your contact center a unified customer profile. It uses pre-built connectors powered by Amazon AppFlow to combine customer information from third-party applications, such as Salesforce (CRM), ServiceNow (ITSM), and enterprise resource planning (ERP) systems, with contact history from your Amazon Connect contact center. Agents see these profiles in the agent workspace, and you can also use them in self-service experiences, in custom agent applications, or through the Customer Profiles APIs as a standalone service.

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

* AddProfileKey
* BatchGetCalculatedAttributeForProfile
* BatchGetProfile
* BatchPutProfileObject
* CreateCalculatedAttributeDefinition
* CreateDomain
* CreateEventStream
* CreateEventTrigger
* CreateProfile
* CreateSegmentDefinition
* CreateSegmentEstimate
* CreateSegmentSnapshot
* CreateUploadJob
* DeleteCalculatedAttributeDefinition
* DeleteDomain
* DeleteEventStream
* DeleteEventTrigger
* DeleteProfile
* DeleteProfileKey
* DeleteProfileObjectType
* DeleteSegmentDefinition
* DetectProfileObjectType
* GetAutoMergingPreview
* GetCalculatedAttributeDefinition
* GetCalculatedAttributeForProfile
* GetDomain
* GetEventStream
* GetEventTrigger
* GetMatches
* GetProfileHistoryRecord
* GetProfileObjectType
* GetProfileObjectTypeTemplate
* GetSegmentDefinition
* GetSegmentEstimate
* GetSegmentMembership
* GetSegmentSnapshot
* GetUploadJob
* GetUploadJobPath
* ListAccountIntegrations
* ListCalculatedAttributeDefinitions
* ListCalculatedAttributesForProfile
* ListDomainLayouts
* ListDomainObjectTypes
* ListDomains
* ListEventStreams
* ListEventTriggers
* ListIdentityResolutionJobs
* ListIntegrations
* ListObjectTypeAttributes
* ListProfileAttributeValues
* ListProfileHistoryRecords
* ListProfileObjectTypeTemplates
* ListProfileObjectTypes
* ListProfileObjects
* ListRecommenderFilters
* ListRecommenderRecipes
* ListRecommenderSchemas
* ListRecommenders
* ListRuleBasedMatches
* ListSegmentDefinitions
* ListTagsForResource
* ListUploadJobs
* ListWorkflows
* PutProfileObject
* PutProfileObjectType
* SearchProfiles
* StartUploadJob
* TagResource
* UntagResource
* UpdateCalculatedAttributeDefinition
* UpdateDomain
* UpdateEventTrigger
* UpdateProfile
