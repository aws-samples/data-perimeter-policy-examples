# Service-specific guidance: AWS CloudWatch RUM


This document outlines service-specific guidance for implementing a data perimeter for AWS CloudWatch RUM.

AWS CloudWatch RUM performs real user monitoring, collecting client-side performance data such as page load times, client-side errors, crashes, and user behavior from actual user sessions of web and mobile applications in near real time. Customers create an app monitor whose RUM client captures telemetry from a configurable percentage of user sessions, which they can use to quickly identify and debug client-side performance issues.

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

* BatchCreateRumMetricDefinitions
* BatchDeleteRumMetricDefinitions
* BatchGetRumMetricDefinitions
* CreateAppMonitor
* DeleteAppMonitor
* DeleteResourcePolicy
* DeleteRumMetricsDestination
* GetAppMonitor
* GetAppMonitorData
* GetResourcePolicy
* ListAppMonitors
* ListRumMetricsDestinations
* ListTagsForResource
* PutResourcePolicy
* PutRumEvents
* PutRumMetricsDestination
* TagResource
* UntagResource
* UpdateAppMonitor
* UpdateRumMetricDefinition
