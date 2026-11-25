# Service-specific guidance: Amazon Bedrock Data Automation


This document outlines service-specific guidance for implementing a data perimeter for Amazon Bedrock Data Automation.

Amazon Bedrock Data Automation (BDA), a capability of Amazon Bedrock, is a cloud-based service that simplifies the process of extracting valuable insights from unstructured content—such as documents, images, video, and audio. BDA uses generative AI to automate the transformation of multi-modal data into structured formats, enabling developers to build applications and automate complex workflows with greater speed and accuracy.

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

* CopyBlueprintStage
* CreateBlueprint
* CreateBlueprintVersion
* CreateDataAutomationLibrary
* CreateDataAutomationProject
* DeleteBlueprint
* DeleteDataAutomationLibrary
* DeleteDataAutomationProject
* GetBlueprint
* GetBlueprintOptimizationStatus
* GetDataAutomationLibrary
* GetDataAutomationLibraryEntity
* GetDataAutomationLibraryIngestionJob
* GetDataAutomationProject
* InvokeBlueprintOptimizationAsync
* InvokeDataAutomationLibraryIngestionJob
* ListBlueprints
* ListDataAutomationLibraries
* ListDataAutomationLibraryEntities
* ListDataAutomationLibraryIngestionJobs
* ListDataAutomationProjects
* ListTagsForResource
* TagResource
* UntagResource
* UpdateBlueprint
* UpdateDataAutomationLibrary
* UpdateDataAutomationProject
