# Service-specific guidance: Amazon SageMaker Feature Store Runtime


This document outlines service-specific guidance for implementing a data perimeter for Amazon SageMaker Feature Store Runtime.

Amazon SageMaker Feature Store Runtime provides the data plane API operations for Amazon SageMaker Feature Store, which you use to put, delete, and retrieve (get) feature records in feature groups. Feature Store simplifies how you create, store, share, and manage machine learning features, with an online store for low-millisecond-latency real-time lookups of the latest records and an offline store that keeps historical records in your Amazon S3 bucket.

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

* BatchGetRecord
* DeleteRecord
* GetRecord
* PutRecord
