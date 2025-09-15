# Service-specific guidance: Amazon CloudWatch


This document outlines service-specific guidance for implementing a data perimeter for Amazon CloudWatch. 


Amazon CloudWatch is a monitoring and observability service that provides real-time insights into AWS resources, applications, and services. It collects and tracks metrics, logs, and events, allowing users to set alarms, visualize data with automated dashboards, and take automated actions based on predefined thresholds. CloudWatch enables AWS customers to gain system-wide visibility, optimize resource utilization, and respond quickly to operational issues.


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
* DeleteAlarms
* DeleteAnomalyDetector
* DeleteDashboards
* DeleteInsightRules
* DeleteMetricStream
* DescribeAlarmHistory
* DescribeAlarms
* DescribeAlarmsForMetric
* DescribeAnomalyDetectors
* DescribeInsightRules
* DisableAlarmActions
* DisableInsightRules
* EnableAlarmActions
* EnableInsightRules
* GetDashboard
* GetInsightRuleReport
* GetMetricData
* GetMetricStatistics
* GetMetricStream
* GetMetricWidgetImage
* ListDashboards
* ListManagedInsightRules
* ListMetrics
* ListMetricStreams
* ListTagsForResource
* PutAnomalyDetector
* PutCompositeAlarm
* PutDashboard
* PutInsightRule
* PutManagedInsightRules
* PutMetricAlarm
* PutMetricData
* PutMetricStream
* SetAlarmState
* StartMetricStreams
* StopMetricStreams
* TagResource
* UntagResource
