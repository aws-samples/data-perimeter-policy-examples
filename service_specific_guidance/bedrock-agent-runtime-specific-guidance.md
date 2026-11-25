# Service-specific guidance: Amazon Bedrock Agents Runtime


This document outlines service-specific guidance for implementing a data perimeter for Amazon Bedrock Agents Runtime.

Amazon Bedrock is a fully managed service that provides secure, enterprise-grade access to high-performing foundation models from leading AI companies, enabling you to build and scale generative AI applications. Its bedrock-agent-runtime API (Agents for Amazon Bedrock Runtime) provides the runtime operations for invoking agents and prompt flows, querying knowledge bases (for example, Retrieve and RetrieveAndGenerate), and managing conversation sessions.

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

* CreateInvocation
* CreateSession
* DeleteAgentMemory
* DeleteSession
* EndSession
* GetAgentMemory
* GetExecutionFlowSnapshot
* GetFlowExecution
* GetInvocationStep
* GetSession
* InvokeAgent
* InvokeFlow
* InvokeInlineAgent
* ListFlowExecutionEvents
* ListFlowExecutions
* ListInvocationSteps
* ListInvocations
* ListSessions
* ListTagsForResource
* OptimizePrompt
* PutInvocationStep
* Rerank
* Retrieve
* RetrieveAndGenerate
* RetrieveAndGenerateStream
* StartFlowExecution
* TagResource
* UntagResource
* UpdateSession
