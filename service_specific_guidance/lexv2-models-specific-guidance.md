# Service-specific guidance: Amazon Lex V2 Model Building service


This document outlines service-specific guidance for implementing a data perimeter for Amazon Lex V2 Model Building service. 


Amazon Lex V2 Model Building service is an AWS offering that enables developers to create conversational interfaces using voice and text. It provides tools and APIs for building, testing, and deploying chatbots and virtual assistants with advanced natural language understanding capabilities. This service allows you to design conversational flows, define intents and slots, and integrate with various AWS services to create intelligent, interactive applications.


The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type | Security objective | Applied on | Policy type | Additional considerations |
|----------------|-------------------|------------|-------------|------------------------|
| Identity perimeter | Only trusted identities can access my resources | Resource | RCP | Y |
| Identity perimeter | Only trusted identities are allowed from my network | Network | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources | Identity | SCP | N |
| Resource perimeter | Only trusted resources can be accessed from my network | Network | VPC endpoint policy | N |
| Network perimeter | My identities can access resources only from expected networks | Identity | SCP | Y |
| Network perimeter | My resources can be accessed only from expected networks | Resource | RCP | Y |

*Y – Additional considerations apply. N – No additional considerations apply.
 






**Additional consideration 1**

Perimeter type applicability: network perimeter.
        
DescribeExport returns an Amazon S3 presigned URL that users can use to download a bot or bot locale archive from a service-owned bucket. The presigned URL is created with an Amazon Lex service principal.

If you want to restrict access to expected networks, consider implementing these additional controls:

* **Preventative control example:** Consider restricting [DescribeExport](https://docs.aws.amazon.com/lexv2/latest/APIReference/API_DescribeExport.html) permissions to administrators or specific applications only using an SCP. See [restrict_presignedUrl_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_presignedURL_scp.json) for an example policy.

* **Detective control example:** Consider using CloudTrail management events to monitor the [DescribeExport](https://docs.aws.amazon.com/lexv2/latest/APIReference/API_DescribeExport.html) API calls in your environment. If necessary, remediate with the responsive controls of your choice.


**Additional consideration 2**

Perimeter type applicability: network perimeter.
        
CreateUploadUrl returns an Amazon S3 presigned URL that users can use to upload the zip archive to a service-owned bucket when importing a bot or a bot locale. The presigned URL is created with an Amazon Lex service principal.

If you want to restrict access to expected networks, consider implementing these additional controls:

* **Preventative control example:** Consider restricting [CreateUploadUrl](https://docs.aws.amazon.com/lexv2/latest/APIReference/API_CreateUploadUrl.html) permissions to administrators or specific applications only using an SCP. See [restrict_presignedURL_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_presignedURL_scp.json) for an example policy.

* **Detective control example:** Consider using CloudTrail management events to monitor the [CreateUploadUrl](https://docs.aws.amazon.com/lexv2/latest/APIReference/API_CreateUploadUrl.html) API calls in your environment. If necessary, remediate with the responsive controls of your choice.


**Additional consideration 3**

Perimeter type applicability: identity and network perimeter applied on resource.
        
CreateResourcePolicy allows you to apply a resource-based policy to grant access to a Lex resource such as a bot or a bot alias. The service currently does not support RCPs.

If you want to restrict access to trusted identities and expected networks, consider implementing these additional controls:

* **Preventative control example**: Consider restricting [CreateResourcePolicy](https://docs.aws.amazon.com/lexv2/latest/APIReference/API_CreateResourcePolicy.html) permissions to administrators only using an SCP. See [restrict_resource_policy_configurations_scp.json](../service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Policy](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-lex-resourcepolicy.html#cfn-lex-resourcepolicy-policy) property for the [AWS::Lex::ResourcePolicy](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-lex-resourcepolicy.html) resource that grants permissions to untrusted identities or unexpected networks. 
* **Detective control example:** Consider using CloudTrail management events to monitor the [CreateResourcePolicy](https://docs.aws.amazon.com/lexv2/latest/APIReference/API_CreateResourcePolicy.html) API calls in your environment (specifically, the [policy](https://docs.aws.amazon.com/lexv2/latest/APIReference/API_CreateResourcePolicy.html#lexv2-CreateResourcePolicy-request-policy) request parameter). If necessary, remediate with the responsive controls of your choice. 


**Additional consideration 4**

Perimeter type applicability: identity and network perimeter applied on resource.
        
UpdateResourcePolicy allows you to apply a resource-based policy to grant access to a Lex resource such as a bot or a bot alias. The service currently does not support RCPs.

If you want to restrict access to trusted identities and expected networks, consider implementing these additional controls:

* **Preventative control example**: Consider restricting [UpdateResourcePolicy](https://docs.aws.amazon.com/lexv2/latest/APIReference/API_UpdateResourcePolicy.html) permissions to administrators only using an SCP. See [restrict_resource_policy_configurations_scp.json](../service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Policy](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-lex-resourcepolicy.html#cfn-lex-resourcepolicy-policy) property for the [AWS::Lex::ResourcePolicy](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-lex-resourcepolicy.html) resource that grants permissions to untrusted identities or unexpected networks. 
* **Detective control example:** Consider using CloudTrail management events to monitor the [UpdateResourcePolicy](https://docs.aws.amazon.com/lexv2/latest/APIReference/API_UpdateResourcePolicy.html) API calls in your environment (specifically, the [policy](https://docs.aws.amazon.com/lexv2/latest/APIReference/API_UpdateResourcePolicy.html#lexv2-UpdateResourcePolicy-request-policy) request parameter). If necessary, remediate with the responsive controls of your choice. 





**List of service APIs reviewed against data perimeter control objectives**
* BatchCreateCustomVocabularyItem
* BatchDeleteCustomVocabularyItem
* BatchUpdateCustomVocabularyItem
* BuildBotLocale
* CreateBot
* CreateBotAlias
* CreateBotLocale
* CreateBotReplica
* CreateBotVersion
* CreateExport
* CreateIntent
* CreateResourcePolicy
* CreateResourcePolicyStatement
* CreateSlotType
* CreateTestSetDiscrepancyReport
* CreateUploadUrl
* DeleteBot
* DeleteBotAlias
* DeleteBotLocale
* DeleteBotReplica
* DeleteBotVersion
* DeleteCustomVocabulary
* DeleteExport
* DeleteImport
* DeleteIntent
* DeleteResourcePolicy
* DeleteResourcePolicyStatement
* DeleteSlotType
* DeleteUtterances
* DescribeBot
* DescribeBotAlias
* DescribeBotLocale
* DescribeBotRecommendation
* DescribeBotReplica
* DescribeBotVersion
* DescribeCustomVocabularyMetadata
* DescribeExport
* DescribeImport
* DescribeIntent
* DescribeResourcePolicy
* DescribeSlotType
* DescribeTestExecution
* DescribeTestSet
* DescribeTestSetDiscrepancyReport
* DescribeTestSetGeneration
* GetTestExecutionArtifactsUrl
* ListAggregatedUtterances
* ListBotAliasReplicas
* ListBotAliases
* ListBotLocales
* ListBotRecommendations
* ListBotReplicas
* ListBotResourceGenerations
* ListBotVersionReplicas
* ListBotVersions
* ListBots
* ListBuiltInIntents
* ListBuiltInSlotTypes
* ListCustomVocabularyItems
* ListExports
* ListImports
* ListIntentMetrics
* ListIntentPaths
* ListIntentStageMetrics
* ListIntents
* ListRecommendedIntents
* ListSessionAnalyticsData
* ListSessionMetrics
* ListSlotTypes
* ListSlots
* ListTagsForResource
* ListTestExecutionResultItems
* ListTestExecutions
* ListTestSetRecords
* ListTestSets
* ListUtteranceAnalyticsData
* ListUtteranceMetrics
* SearchAssociatedTranscripts
* StartBotRecommendation
* StartImport
* StartTestExecution
* StartTestSetGeneration
* StopBotRecommendation
* TagResource
* UntagResource
* UpdateBot
* UpdateBotAlias
* UpdateBotLocale
* UpdateExport
* UpdateIntent
* UpdateResourcePolicy
* UpdateSlotType
* UpdateTestSet
