# Service-specific guidance: Amazon EventBridge Scheduler


This document outlines service-specific guidance for implementing a data perimeter for Amazon EventBridge Scheduler.

Amazon EventBridge Scheduler is a serverless scheduler that allows you to create, run, and manage tasks from one central, managed service. Using cron, rate, or one-time schedule expressions, it can invoke more than 270 AWS services and over 6,000 API operations, with built-in mechanisms that adjust schedules based on the availability of downstream targets.

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

* CreateSchedule
* CreateScheduleGroup
* DeleteSchedule
* DeleteScheduleGroup
* GetSchedule
* GetScheduleGroup
* ListScheduleGroups
* ListSchedules
* ListTagsForResource
* TagResource
* UntagResource
* UpdateSchedule
