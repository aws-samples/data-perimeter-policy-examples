
# Service-specific guidance: AWS CodePipeline


This document outlines service-specific guidance for implementing a data perimeter for AWS CodePipeline. 

AWS CodePipeline is a fully managed continuous delivery service that helps you automate your software release processes. It enables you to model, visualize, and automate the steps required to release your software, allowing you to rapidly and reliably deliver features and updates. CodePipeline integrates with various AWS services and third-party tools, making it easy to build, test, and deploy your code every time there's a change.


The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type | Security objective | Applied on | Policy type | Additional considerations |
|----------------|-------------------|------------|-------------|------------------------|
| Identity perimeter | Only trusted identities can access my resources | Resource | RCP | N |
| Identity perimeter | Only trusted identities are allowed from my network | Network | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources | Identity | SCP | N |
| Resource perimeter | Only trusted resources can be accessed from my network | Network | VPC endpoint policy | N |
| Network perimeter | My identities can access resources only from expected networks | Identity | SCP | N |
| Network perimeter | My resources can be accessed only from expected networks | Resource | RCP | N |

*Y – Additional considerations apply. N – No additional considerations apply.
 


**List of service APIs reviewed against data perimeter control objectives**

* CreatePipeline

* GetPipelineState

* GetPipelineExecution

* RetryStageExecution

* StopPipelineExecution

* PutActionRevision

* EnableStageTransition

* StartPipelineExecution

* TagResource

* UpdatePipeline

* ListActionExecutions

* ListActionTypes

* ListPipelineExecutions

* ListPipelines

* ListRuleExecutions

* ListRuleTypes

* ListTagsForResource

* ListWebhooks

* GetPipeline

* UntagResource

* DisableStageTransition

* DeleteCustomActionType

* DeletePipeline


