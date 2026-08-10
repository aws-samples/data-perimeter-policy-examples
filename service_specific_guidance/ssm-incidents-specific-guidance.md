# Service-specific guidance: AWS Systems Manager Incident Manager


This document outlines service-specific guidance for implementing a data perimeter for AWS Systems Manager Incident Manager.

Incident Manager, a tool in AWS Systems Manager, is designed to help you mitigate and recover from incidents affecting your applications hosted on AWS. An incident is any unplanned interruption or reduction in the quality of services that can have a significant impact on business operations.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | Y |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | N |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | N |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | N |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | Y |

*Y - Additional considerations apply. N - No additional considerations apply.

## PutResourcePolicy
### No RCP support

**Perimeter type applicability**: identity and network perimeter applied on resource.

**Description**: [PutResourcePolicy](https://docs.aws.amazon.com/incident-manager/latest/APIReference/API_PutResourcePolicy.html) allows you to apply a resource-based policy to grant access to a response plan. The service currently does not support RCPs.

**Additional controls**:

If you want to restrict access to trusted identities, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutResourcePolicy](https://docs.aws.amazon.com/incident-manager/latest/APIReference/API_PutResourcePolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [PutResourcePolicy](https://docs.aws.amazon.com/incident-manager/latest/APIReference/API_PutResourcePolicy.html) API calls in your environment (specifically, the [policy](https://docs.aws.amazon.com/incident-manager/latest/APIReference/API_PutResourcePolicy.html#IncidentManager-PutResourcePolicy-request-policy) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutResourcePolicy](https://docs.aws.amazon.com/incident-manager/latest/APIReference/API_PutResourcePolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [PutResourcePolicy](https://docs.aws.amazon.com/incident-manager/latest/APIReference/API_PutResourcePolicy.html) API calls in your environment (specifically, the [policy](https://docs.aws.amazon.com/incident-manager/latest/APIReference/API_PutResourcePolicy.html#IncidentManager-PutResourcePolicy-request-policy) request parameter). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

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
