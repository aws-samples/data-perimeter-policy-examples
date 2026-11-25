# Service-specific guidance: Amazon Bedrock Runtime


This document outlines service-specific guidance for implementing a data perimeter for Amazon Bedrock Runtime.

Amazon Bedrock is a fully managed service that provides secure, enterprise-grade access to high-performing foundation models from leading AI companies, enabling you to build and scale generative AI applications. Its bedrock-runtime endpoint provides the data plane APIs, such as InvokeModel, Converse, ApplyGuardrail, and their streaming and asynchronous variants, for making inference requests to models hosted in Amazon Bedrock.

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

* ApplyGuardrail
* Converse
* ConverseStream
* CountTokens
* GetAsyncInvoke
* InvokeModel
* InvokeModelWithResponseStream
* ListAsyncInvokes
* StartAsyncInvoke
