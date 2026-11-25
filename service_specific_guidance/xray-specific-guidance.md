# Service-specific guidance: AWS X-Ray


This document outlines service-specific guidance for implementing a data perimeter for AWS X-Ray.

AWS X-Ray is a service that collects data about requests that your application serves, and provides tools that you can use to view, filter, and gain insights into that data to identify issues and opportunities for optimization. For any traced request, X-Ray shows detailed information about the request and response, and also about calls your application makes to downstream AWS resources, microservices, databases, and web APIs.

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

* CancelTraceRetrieval
* CreateGroup
* CreateSamplingRule
* DeleteGroup
* DeleteResourcePolicy
* DeleteSamplingRule
* GetEncryptionConfig
* GetGroup
* GetGroups
* GetIndexingRules
* GetInsightSummaries
* GetRetrievedTracesGraph
* GetSamplingRules
* GetSamplingStatisticSummaries
* GetSamplingTargets
* GetServiceGraph
* GetTimeSeriesServiceStatistics
* GetTraceSegmentDestination
* GetTraceSummaries
* ListResourcePolicies
* ListRetrievedTraces
* ListTagsForResource
* PutEncryptionConfig
* PutResourcePolicy
* PutTelemetryRecords
* PutTraceSegments
* StartTraceRetrieval
* TagResource
* UntagResource
* UpdateGroup
* UpdateIndexingRule
* UpdateSamplingRule
