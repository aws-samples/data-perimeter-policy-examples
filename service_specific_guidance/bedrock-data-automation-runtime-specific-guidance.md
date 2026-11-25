# Service-specific guidance: Amazon Bedrock Data Automation Runtime


This document outlines service-specific guidance for implementing a data perimeter for Amazon Bedrock Data Automation Runtime.

Amazon Bedrock Data Automation (BDA) is a capability of Amazon Bedrock that uses generative AI to extract insights from unstructured content such as documents, images, video, and audio, and to turn that multi-modal data into structured formats for applications and automated workflows. The Bedrock Data Automation runtime API processes files using a data automation project (asynchronously from Amazon S3, or synchronously for images) and reports each invocation's status and the S3 location of its results.

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

* GetDataAutomationStatus
* InvokeDataAutomation
* InvokeDataAutomationAsync
* ListTagsForResource
* TagResource
* UntagResource
