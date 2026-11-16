# Service-specific guidance: AWS Systems Manager Incident Manager


This document outlines service-specific guidance for implementing a data perimeter for AWS Systems Manager Incident Manager.

Incident Manager, a tool in AWS Systems Manager, is designed to help you mitigate and recover from incidents affecting your applications hosted on AWS. An incident is any unplanned interruption or reduction in the quality of services that can have a significant impact on business operations.

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

* CreateResponsePlan
* CreateTimelineEvent
* DeleteIncidentRecord
* DeleteResourcePolicy
* DeleteResponsePlan
* DeleteTimelineEvent
* GetIncidentRecord
* GetReplicationSet
* GetResourcePolicies
* GetResponsePlan
* GetTimelineEvent
* ListIncidentFindings
* ListIncidentRecords
* ListRelatedItems
* ListReplicationSets
* ListResponsePlans
* ListTagsForResource
* ListTimelineEvents
* PutResourcePolicy
* StartIncident
* TagResource
* UntagResource
* UpdateDeletionProtection
* UpdateIncidentRecord
* UpdateRelatedItems
* UpdateResponsePlan
* UpdateTimelineEvent
