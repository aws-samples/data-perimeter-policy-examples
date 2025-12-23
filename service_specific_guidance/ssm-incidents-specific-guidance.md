# Service-specific guidance: AWS Systems Manager Incident Manager


This document outlines service-specific guidance for implementing a data perimeter for AWS Systems Manager Incident Manager. 


AWS Systems Manager Incident Manager is a service that helps you manage, respond to, and resolve operational incidents in your AWS environment. It provides automated incident response, collaboration tools, and post-incident analysis capabilities to help teams quickly mitigate and learn from operational issues, reducing mean time to resolution (MTTR) and improving overall system reliability.


The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type | Security objective | Applied on | Policy type | Additional considerations |
|----------------|-------------------|------------|-------------|------------------------|
| Identity perimeter | Only trusted identities can access my resources | Resource | RCP | N |
| Identity perimeter | Only trusted identities are allowed from my network | Network | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources | Identity | SCP | N |
| Resource perimeter | Only trusted resources can be accessed from my network | Network | VPC endpoint policy | N |
| Network perimeter | My identities can access resources only from expected networks | Identity | SCP | N |
| Network perimeter | My resources can be accessed only from expected networks | Resource | RCP | N |

*Y – Additional considerations apply. N – No additional considerations apply.
 



**List of service APIs reviewed against data perimeter control objectives**
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
