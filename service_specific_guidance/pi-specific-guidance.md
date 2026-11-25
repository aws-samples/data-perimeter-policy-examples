# Service-specific guidance: AWS Performance Insights


This document outlines service-specific guidance for implementing a data perimeter for AWS Performance Insights.

AWS Performance Insights (Amazon RDS Performance Insights) enables you to monitor and explore different dimensions of database load based on data captured from a running DB instance, across Amazon RDS, Amazon Aurora, and Amazon DocumentDB. Its API returns DB load, measured as average active sessions, as a time-series dataset that can be broken down by dimensions such as SQL, wait event, user, and host, and also supports creating performance analysis reports.

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

* DeletePerformanceAnalysisReport
* DescribeDimensionKeys
* GetPerformanceAnalysisReport
* GetResourceMetadata
* GetResourceMetrics
* ListAvailableResourceDimensions
* ListAvailableResourceMetrics
* ListPerformanceAnalysisReports
* ListTagsForResource
* TagResource
* UntagResource
