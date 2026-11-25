# Service-specific guidance: Amazon Bedrock Agents


This document outlines service-specific guidance for implementing a data perimeter for Amazon Bedrock Agents.

Amazon Bedrock is a fully managed service that provides secure, enterprise-grade access to high-performing foundation models from leading AI companies, enabling you to build and scale generative AI applications. Its bedrock-agent (Agents for Amazon Bedrock build-time) API is used to create and manage agents, knowledge bases, flows, and prompts that orchestrate interactions between foundation models, data sources, software applications, and user conversations.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | N |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | Y |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | N |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | N |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | N |

*Y - Additional considerations apply. N - No additional considerations apply.

## CreateDataSource
### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [CreateDataSource](https://docs.aws.amazon.com/bedrock/latest/APIReference/API_agent_CreateDataSource.html) allows you to specify endpoints such as Confluence, Microsoft SharePoint, and Salesforce instances as the value for the `dataSourceConfiguration` request parameter. Because the subsequent requests against the endpoints are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Preventative control example: Consider restricting [CreateDataSource](https://docs.aws.amazon.com/bedrock/latest/APIReference/API_agent_CreateDataSource.html) permissions to select principals using an SCP. See [restrict_untrusted_endpoints_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_untrusted_endpoints_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [DataSourceConfiguration](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-bedrock-datasource.html#cfn-bedrock-datasource-datasourceconfiguration) property that does not belong to your organization for the [AWS::Bedrock::DataSource](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-bedrock-datasource.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateDataSource](https://docs.aws.amazon.com/bedrock/latest/APIReference/API_agent_CreateDataSource.html) API calls in your environment (specifically, the [dataSourceConfiguration](https://docs.aws.amazon.com/bedrock/latest/APIReference/API_agent_CreateDataSource.html#bedrock-agent_CreateDataSource-request-dataSourceConfiguration) request parameter). If necessary, remediate with the responsive controls of your choice.

## UpdateDataSource
### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [UpdateDataSource](https://docs.aws.amazon.com/bedrock/latest/APIReference/API_agent_UpdateDataSource.html) allows you to specify endpoints such as Confluence, Microsoft SharePoint, and Salesforce instances as the value for the `dataSourceConfiguration` request parameter. Because the subsequent requests against the endpoints are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Preventative control example: Consider restricting [UpdateDataSource](https://docs.aws.amazon.com/bedrock/latest/APIReference/API_agent_UpdateDataSource.html) permissions to select principals using an SCP. See [restrict_untrusted_endpoints_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_untrusted_endpoints_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [DataSourceConfiguration](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-bedrock-datasource.html#cfn-bedrock-datasource-datasourceconfiguration) property that does not belong to your organization for the [AWS::Bedrock::DataSource](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-bedrock-datasource.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateDataSource](https://docs.aws.amazon.com/bedrock/latest/APIReference/API_agent_UpdateDataSource.html) API calls in your environment (specifically, the [dataSourceConfiguration](https://docs.aws.amazon.com/bedrock/latest/APIReference/API_agent_UpdateDataSource.html#bedrock-agent_UpdateDataSource-request-dataSourceConfiguration) request parameter). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

The service APIs covered in this review are based on botocore version [1.43.29](https://github.com/boto/botocore/tree/1.43.29/botocore).

* AssociateAgentKnowledgeBase
* CreateAgent
* CreateAgentActionGroup
* CreateAgentAlias
* CreateDataSource
* CreateFlow
* CreateFlowAlias
* CreateFlowVersion
* CreateKnowledgeBase
* CreatePrompt
* CreatePromptVersion
* DeleteAgent
* DeleteAgentActionGroup
* DeleteAgentAlias
* DeleteDataSource
* DeleteFlow
* DeleteFlowAlias
* DeleteFlowVersion
* DeleteKnowledgeBase
* DeleteKnowledgeBaseDocuments
* DeletePrompt
* DisassociateAgentKnowledgeBase
* GetAgent
* GetAgentActionGroup
* GetAgentAlias
* GetAgentKnowledgeBase
* GetAgentVersion
* GetDataSource
* GetFlow
* GetFlowAlias
* GetFlowVersion
* GetIngestionJob
* GetKnowledgeBase
* GetKnowledgeBaseDocuments
* GetPrompt
* IngestKnowledgeBaseDocuments
* ListAgentActionGroups
* ListAgentAliases
* ListAgentCollaborators
* ListAgentKnowledgeBases
* ListAgentVersions
* ListAgents
* ListDataSources
* ListFlowAliases
* ListFlowVersions
* ListFlows
* ListIngestionJobs
* ListKnowledgeBaseDocuments
* ListKnowledgeBases
* ListPrompts
* ListTagsForResource
* PrepareAgent
* PrepareFlow
* StartIngestionJob
* TagResource
* UntagResource
* UpdateAgent
* UpdateAgentActionGroup
* UpdateAgentAlias
* UpdateAgentKnowledgeBase
* UpdateDataSource
* UpdateFlow
* UpdateFlowAlias
* UpdateKnowledgeBase
* UpdatePrompt
* ValidateFlowDefinition
