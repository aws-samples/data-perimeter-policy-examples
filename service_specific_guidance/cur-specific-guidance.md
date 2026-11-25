# Service-specific guidance: AWS Cost and Usage Report


This document outlines service-specific guidance for implementing a data perimeter for AWS Cost and Usage Report.

AWS Cost and Usage Report (AWS CUR) contains the most comprehensive set of cost and usage data available, letting you publish your AWS billing reports to an Amazon S3 bucket that you own, broken down by the hour, day, or month, by product or product resource, or by tags that you define. Each report contains line items for each unique combination of AWS products, usage type, and operation that you use in your AWS account.

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

* DeleteReportDefinition
* DescribeReportDefinitions
* ListTagsForResource
* ModifyReportDefinition
* PutReportDefinition
* TagResource
* UntagResource
