# Service-specific guidance: AWS Glue


This document outlines service-specific guidance for implementing a data perimeter for AWS Glue.

AWS Glue is a serverless data integration service that makes it easy for analytics users to discover, prepare, move, and integrate data from multiple sources for analytics, machine learning, and application development.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | Y |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | Y |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | Y |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | Y |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | Y |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | Y |

*Y - Additional considerations apply. N - No additional considerations apply.

## CreateConnection
### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [CreateConnection](https://docs.aws.amazon.com/glue/latest/webapi/API_CreateConnection.html) allows you to specify endpoints such as `JDBC` and `SERVICENOW` as the value for the `ConnectionType` and `ConnectionProperties` request parameters. Because the subsequent requests against the endpoints are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [ConnectionType](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-glue-connection-connectioninput.html#cfn-glue-connection-connectioninput-connectiontype) and [ConnectionProperties](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-glue-connection-connectioninput.html#cfn-glue-connection-connectioninput-connectionproperties) properties that do not belong to your organization for the [AWS::Glue::Connection](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-glue-connection.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateConnection](https://docs.aws.amazon.com/glue/latest/webapi/API_CreateConnection.html) API calls in your environment (specifically, the [ConnectionType](https://docs.aws.amazon.com/glue/latest/webapi/API_ConnectionInput.html#Glue-Type-ConnectionInput-ConnectionType) request parameter). If necessary, remediate with the responsive controls of your choice.

## CreateJob
### Code execution within a VPC

**Perimeter type applicability**: identity and resource perimeter on network, network perimeter.

**Description**: [CreateJob](https://docs.aws.amazon.com/glue/latest/webapi/API_CreateJob.html) allows you to associate jobs with an Amazon VPC to run your code.

**Additional controls**:

If you want to achieve data perimeter control objectives, consider implementing these additional controls:
* Preventative control example: Consider implementing `glue:VpcIds` in an SCP to help restrict creation of resources to a customer managed VPC only. See [restrict_nonvpc_deployment_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help ensure that developers specify the [Connections](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-glue-job.html#cfn-glue-job-connections) property of the [AWS::Glue::Job](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-glue-job.html) resource.
* Detective control example: Consider implementing a custom AWS Config rule to help detect jobs not associated with a VPC or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateJob](https://docs.aws.amazon.com/glue/latest/webapi/API_CreateJob.html) API calls in your environment (specifically, the [Connections](https://docs.aws.amazon.com/glue/latest/webapi/API_CreateJob.html#Glue-CreateJob-request-Connections) request parameter). If necessary, remediate with the responsive controls of your choice.

### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [CreateJob](https://docs.aws.amazon.com/glue/latest/webapi/API_CreateJob.html) allows you to specify a service role for your AWS Glue ETL job operations. The service uses the role to make AWS API calls from your code.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the service-specific SCP, [network_perimeter_glue_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/network_perimeter_glue_scp.json). This policy lists `logs:CreateLogGroup`, `logs:CreateLogStream`, and `logs:PutLogEvents` in the `NotAction` element because the service makes direct calls from its network to Amazon CloudWatch Logs on your behalf. If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

## CreateSession
### Code execution within a VPC

**Perimeter type applicability**: identity and resource perimeter on network, network perimeter.

**Description**: [CreateSession](https://docs.aws.amazon.com/glue/latest/webapi/API_CreateSession.html) allows you to associate an interactive session with an Amazon VPC to run your code.

**Additional controls**:

If you want to achieve data perimeter control objectives, consider implementing these additional controls:
* Preventative control example: Consider implementing `glue:VpcIds` in an SCP to help restrict creation of resources to a customer managed VPC only. See [restrict_nonvpc_deployment_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.
* Detective control example: Consider implementing a custom AWS Config rule to help detect interactive sessions not associated with a VPC or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateSession](https://docs.aws.amazon.com/glue/latest/webapi/API_CreateSession.html) API calls in your environment (specifically, the [Connections](https://docs.aws.amazon.com/glue/latest/webapi/API_CreateSession.html#Glue-CreateSession-request-Connections) request parameter). If necessary, remediate with the responsive controls of your choice.

### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [CreateSession](https://docs.aws.amazon.com/glue/latest/webapi/API_CreateSession.html) allows you to specify a service role for your interactive session operations. The service uses the role to make AWS API calls from your code.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json) or [network_perimeter_vpceorgid_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_vpceorgid_scp.json). These policies demonstrate how to use the `dp:include:network` tag set to `true` to restrict calls made by the session's runtime role. If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

## PutResourcePolicy
### No RCP support

**Perimeter type applicability**: identity and network perimeter applied on resource.

**Description**: [PutResourcePolicy](https://docs.aws.amazon.com/glue/latest/webapi/API_PutResourcePolicy.html) allows you to apply a resource-based policy to grant access to the Data Catalog. The service currently does not support RCPs.

**Additional controls**:

If you want to restrict access to trusted identities, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutResourcePolicy](https://docs.aws.amazon.com/glue/latest/webapi/API_PutResourcePolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [PutResourcePolicy](https://docs.aws.amazon.com/glue/latest/webapi/API_PutResourcePolicy.html) API calls in your environment (specifically, the [PolicyInJson](https://docs.aws.amazon.com/glue/latest/webapi/API_PutResourcePolicy.html#Glue-PutResourcePolicy-request-PolicyInJson) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutResourcePolicy](https://docs.aws.amazon.com/glue/latest/webapi/API_PutResourcePolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [PutResourcePolicy](https://docs.aws.amazon.com/glue/latest/webapi/API_PutResourcePolicy.html) API calls in your environment (specifically, the [PolicyInJson](https://docs.aws.amazon.com/glue/latest/webapi/API_PutResourcePolicy.html#Glue-PutResourcePolicy-request-PolicyInJson) request parameter). If necessary, remediate with the responsive controls of your choice.

## TestConnection
### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [TestConnection](https://docs.aws.amazon.com/glue/latest/webapi/API_TestConnection.html) allows you to specify endpoints such as `JDBC` and `SERVICENOW` as the value for the `ConnectionType` and `ConnectionProperties` request parameters. Because the subsequent requests against the endpoints are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [ConnectionType](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-glue-connection-connectioninput.html#cfn-glue-connection-connectioninput-connectiontype) and [ConnectionProperties](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-glue-connection-connectioninput.html#cfn-glue-connection-connectioninput-connectionproperties) properties that do not belong to your organization for the [AWS::Glue::Connection](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-glue-connection.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [TestConnection](https://docs.aws.amazon.com/glue/latest/webapi/API_TestConnection.html) API calls in your environment (specifically, the [ConnectionType](https://docs.aws.amazon.com/glue/latest/webapi/API_TestConnectionInput.html#glue-Type-TestConnectionInput-ConnectionType) request parameter). If necessary, remediate with the responsive controls of your choice.

## UpdateConnection
### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [UpdateConnection](https://docs.aws.amazon.com/glue/latest/webapi/API_UpdateConnection.html) allows you to specify endpoints such as `JDBC` and `SERVICENOW` as the value for the `ConnectionType` and `ConnectionProperties` request parameters. Because the subsequent requests against the endpoints are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [ConnectionType](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-glue-connection-connectioninput.html#cfn-glue-connection-connectioninput-connectiontype) and [ConnectionProperties](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-glue-connection-connectioninput.html#cfn-glue-connection-connectioninput-connectionproperties) properties that do not belong to your organization for the [AWS::Glue::Connection](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-glue-connection.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateConnection](https://docs.aws.amazon.com/glue/latest/webapi/API_UpdateConnection.html) API calls in your environment (specifically, the [ConnectionType](https://docs.aws.amazon.com/glue/latest/webapi/API_ConnectionInput.html#Glue-Type-ConnectionInput-ConnectionType) request parameter). If necessary, remediate with the responsive controls of your choice.

## UpdateJob
### Code execution within a VPC

**Perimeter type applicability**: identity and resource perimeter on network, network perimeter.

**Description**: [UpdateJob](https://docs.aws.amazon.com/glue/latest/webapi/API_UpdateJob.html) allows you to associate jobs with an Amazon VPC to run your code.

**Additional controls**:

If you want to achieve data perimeter control objectives, consider implementing these additional controls:
* Preventative control example: Consider implementing `glue:VpcIds` in an SCP to help restrict creation of resources to a customer managed VPC only. See [restrict_nonvpc_deployment_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_nonvpc_deployment_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help ensure that developers specify the [Connections](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-glue-job.html#cfn-glue-job-connections) property of the [AWS::Glue::Job](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-glue-job.html) resource.
* Detective control example: Consider implementing a custom AWS Config rule to help detect jobs not associated with a VPC or use [advanced queries](https://docs.aws.amazon.com/config/latest/developerguide/querying-AWS-resources.html) to get a one-time view of incorrectly configured resources. If necessary, remediate with the responsive controls of your choice.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateJob](https://docs.aws.amazon.com/glue/latest/webapi/API_UpdateJob.html) API calls in your environment (specifically, the [Connections](https://docs.aws.amazon.com/glue/latest/webapi/API_ConnectionsList.html) request parameter). If necessary, remediate with the responsive controls of your choice.

### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [UpdateJob](https://docs.aws.amazon.com/glue/latest/webapi/API_UpdateJob.html) allows you to specify a service role for your AWS Glue ETL job operations. The service uses the role to make AWS API calls from your code.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the service-specific SCP, [network_perimeter_glue_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/network_perimeter_glue_scp.json). This policy lists `logs:CreateLogGroup`, `logs:CreateLogStream`, and `logs:PutLogEvents` in the `NotAction` element because the service makes direct calls from its network to Amazon CloudWatch Logs on your behalf. If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

## List of service APIs reviewed against data perimeter control objectives

* BatchCreatePartition
* BatchGetBlueprints
* BatchGetCrawlers
* BatchGetCustomEntityTypes
* BatchGetDevEndpoints
* BatchGetJobs
* BatchGetPartition
* BatchGetTriggers
* BatchGetWorkflows
* BatchPutDataQualityStatisticAnnotation
* BatchStopJobRun
* BatchUpdatePartition
* CancelDataQualityRuleRecommendationRun
* CancelDataQualityRulesetEvaluationRun
* CancelMLTaskRun
* CancelStatement
* CheckSchemaVersionValidity
* CreateBlueprint
* CreateClassifier
* CreateConnection
* CreateCrawler
* CreateCustomEntityType
* CreateDataQualityRuleset
* CreateDatabase
* CreateDevEndpoint
* CreateGlueIdentityCenterConfiguration
* CreateIntegrationResourceProperty
* CreateIntegrationTableProperties
* CreateJob
* CreateMLTransform
* CreatePartition
* CreatePartitionIndex
* CreateRegistry
* CreateSchema
* CreateSecurityConfiguration
* CreateSession
* CreateTable
* CreateTrigger
* CreateUsageProfile
* CreateUserDefinedFunction
* CreateWorkflow
* DeleteBlueprint
* DeleteClassifier
* DeleteColumnStatisticsForPartition
* DeleteColumnStatisticsForTable
* DeleteConnection
* DeleteCrawler
* DeleteCustomEntityType
* DeleteDataQualityRuleset
* DeleteDatabase
* DeleteDevEndpoint
* DeleteGlueIdentityCenterConfiguration
* DeleteIntegrationTableProperties
* DeleteJob
* DeleteMLTransform
* DeletePartition
* DeletePartitionIndex
* DeleteRegistry
* DeleteResourcePolicy
* DeleteSchema
* DeleteSchemaVersions
* DeleteSecurityConfiguration
* DeleteSession
* DeleteTable
* DeleteTableVersion
* DeleteTrigger
* DeleteUsageProfile
* DeleteWorkflow
* DescribeInboundIntegrations
* DescribeIntegrations
* GetBlueprint
* GetBlueprintRun
* GetBlueprintRuns
* GetCatalog
* GetCatalogImportStatus
* GetCatalogs
* GetClassifier
* GetClassifiers
* GetColumnStatisticsForPartition
* GetColumnStatisticsForTable
* GetColumnStatisticsTaskRuns
* GetConnection
* GetConnections
* GetCrawler
* GetCrawlerMetrics
* GetCrawlers
* GetCustomEntityType
* GetDataCatalogEncryptionSettings
* GetDataQualityRuleRecommendationRun
* GetDataQualityRuleset
* GetDataQualityRulesetEvaluationRun
* GetDatabase
* GetDatabases
* GetDevEndpoint
* GetDevEndpoints
* GetGlueIdentityCenterConfiguration
* GetIntegrationResourceProperty
* GetIntegrationTableProperties
* GetJob
* GetJobRuns
* GetJobs
* GetMLTaskRun
* GetMLTaskRuns
* GetMLTransform
* GetMLTransforms
* GetMapping
* GetPartition
* GetPartitionIndexes
* GetPartitions
* GetRegistry
* GetResourcePolicies
* GetResourcePolicy
* GetSchema
* GetSchemaByDefinition
* GetSchemaVersion
* GetSchemaVersionsDiff
* GetSecurityConfiguration
* GetSecurityConfigurations
* GetSession
* GetStatement
* GetTable
* GetTableVersion
* GetTableVersions
* GetTables
* GetTags
* GetTrigger
* GetTriggers
* GetUsageProfile
* GetUserDefinedFunctions
* GetWorkflow
* GetWorkflowRun
* GetWorkflowRunProperties
* GetWorkflowRuns
* ImportCatalogToGlue
* ListBlueprints
* ListColumnStatisticsTaskRuns
* ListConnectionTypes
* ListCrawlers
* ListCrawls
* ListCustomEntityTypes
* ListDataQualityResults
* ListDataQualityRuleRecommendationRuns
* ListDataQualityRulesetEvaluationRuns
* ListDataQualityRulesets
* ListDevEndpoints
* ListJobs
* ListMLTransforms
* ListRegistries
* ListSchemaVersions
* ListSchemas
* ListSessions
* ListStatements
* ListTriggers
* ListUsageProfiles
* ListWorkflows
* PutDataCatalogEncryptionSettings
* PutResourcePolicy
* PutSchemaVersionMetadata
* PutWorkflowRunProperties
* QuerySchemaVersionMetadata
* RegisterSchemaVersion
* RemoveSchemaVersionMetadata
* RunStatement
* SearchTables
* StartBlueprintRun
* StartCrawler
* StartCrawlerSchedule
* StartDataQualityRuleRecommendationRun
* StartDataQualityRulesetEvaluationRun
* StartJobRun
* StartMLLabelingSetGenerationTaskRun
* StartTrigger
* StartWorkflowRun
* StopCrawlerSchedule
* StopSession
* TagResource
* TestConnection
* UntagResource
* UpdateBlueprint
* UpdateClassifier
* UpdateColumnStatisticsForPartition
* UpdateColumnStatisticsForTable
* UpdateConnection
* UpdateCrawler
* UpdateCrawlerSchedule
* UpdateDataQualityRuleset
* UpdateDatabase
* UpdateGlueIdentityCenterConfiguration
* UpdateIntegrationTableProperties
* UpdateJob
* UpdateMLTransform
* UpdatePartition
* UpdateRegistry
* UpdateSchema
* UpdateTable
* UpdateUsageProfile
* UpdateUserDefinedFunction
* UpdateWorkflow
