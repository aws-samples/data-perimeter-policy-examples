# Service-specific guidance: Amazon CloudWatch Application Insights


This document outlines service-specific guidance for implementing a data perimeter for Amazon CloudWatch Application Insights.

Amazon CloudWatch Application Insights helps you monitor applications that use Amazon EC2 instances along with other application resources by identifying and setting up key metrics, logs, and alarms across your application resources and technology stack, such as SQL Server databases, IIS web and application servers, operating systems, load balancers, and queues. It continuously monitors these metrics and logs to detect and correlate anomalies and errors, and surfaces detected problems through CloudWatch Events, automated troubleshooting dashboards, and AWS Systems Manager OpsCenter OpsItems.

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

* AddWorkload
* CreateApplication
* CreateComponent
* CreateLogPattern
* DeleteApplication
* DeleteComponent
* DeleteLogPattern
* DescribeApplication
* DescribeComponent
* DescribeComponentConfiguration
* DescribeComponentConfigurationRecommendation
* DescribeLogPattern
* DescribeObservation
* DescribeProblem
* DescribeProblemObservations
* DescribeWorkload
* ListApplications
* ListComponents
* ListConfigurationHistory
* ListLogPatternSets
* ListLogPatterns
* ListProblems
* ListTagsForResource
* ListWorkloads
* RemoveWorkload
* TagResource
* UntagResource
* UpdateApplication
* UpdateComponent
* UpdateComponentConfiguration
* UpdateLogPattern
* UpdateProblem
* UpdateWorkload
