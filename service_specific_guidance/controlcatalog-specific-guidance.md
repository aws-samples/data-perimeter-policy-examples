# Service-specific guidance: AWS Control Catalog


This document outlines service-specific guidance for implementing a data perimeter for AWS Control Catalog.

AWS Control Catalog, a part of AWS Control Tower, is a consolidated catalog of AWS controls for several AWS services that lets you view controls according to common use cases, including security, cost, durability, and operations, without needing to set up AWS Control Tower. It is available through the console and the Control Catalog API, which provides a programmatic way to identify and filter the common controls and related metadata available to AWS customers.

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

* GetControl
* ListCommonControls
* ListControlMappings
* ListControls
* ListDomains
* ListObjectives
