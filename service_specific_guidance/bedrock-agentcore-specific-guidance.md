# Service-specific guidance: Amazon Bedrock AgentCore


This document outlines service-specific guidance for implementing a data perimeter for Amazon Bedrock AgentCore.

Amazon Bedrock AgentCore is an agentic platform for building, deploying, and operating highly effective agents securely at scale using any framework and foundation model. AgentCore services work together or independently with any open-source framework such as CrewAI, LangGraph, LlamaIndex, and Strands Agents and with any foundation model.

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

* BatchCreateMemoryRecords
* BatchDeleteMemoryRecords
* BatchUpdateMemoryRecords
* CreateEvent
* DeleteEvent
* DeleteMemoryRecord
* GetAgentCard
* GetBrowserSession
* GetCodeInterpreterSession
* GetEvent
* GetMemoryRecord
* GetWorkloadAccessToken
* GetWorkloadAccessTokenForUserId
* InvokeAgentRuntime
* InvokeCodeInterpreter
* ListActors
* ListBrowserSessions
* ListCodeInterpreterSessions
* ListEvents
* ListMemoryRecords
* ListSessions
* RetrieveMemoryRecords
* StartBrowserSession
* StartCodeInterpreterSession
* StopBrowserSession
* StopCodeInterpreterSession
* StopRuntimeSession
* UpdateBrowserStream
