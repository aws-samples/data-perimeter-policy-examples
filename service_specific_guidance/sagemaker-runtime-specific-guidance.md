# Service-specific guidance: Amazon SageMaker Runtime


This document outlines service-specific guidance for implementing a data perimeter for Amazon SageMaker Runtime.

Amazon SageMaker AI (formerly Amazon SageMaker) is a fully managed machine learning (ML) service that lets data scientists and developers build, train, and deploy ML models into a production-ready hosted environment. Its runtime API, Amazon SageMaker Runtime (`sagemaker-runtime`), is what client applications call to get inferences from models deployed on SageMaker AI hosting endpoints, either synchronously, asynchronously, or as a response stream.

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

* InvokeEndpoint
* InvokeEndpointAsync
* InvokeEndpointWithResponseStream
