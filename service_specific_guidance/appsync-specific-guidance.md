# Service-specific guidance: AWS AppSync


This document outlines service-specific guidance for implementing a data perimeter for AWS AppSync.

AWS AppSync is a managed service that enables developers to connect their applications and services to data and events with secure, serverless, and high-performing GraphQL and Pub/Sub APIs. It provides a single GraphQL endpoint over multiple data sources, publishes real-time updates to subscribed clients, and includes built-in security, monitoring, logging, tracing, and optional caching.

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
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [CreateDataSource](https://docs.aws.amazon.com/appsync/latest/APIReference/API_CreateDataSource.html) allows you to specify a Lambda function that does not belong to your organization as the value for the `lambdaConfig` request parameter. Because the subsequent call against the Lambda function is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [LambdaConfig](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-appsync-datasource.html#cfn-appsync-datasource-lambdaconfig) property that does not belong to your organization for the [AWS::AppSync::DataSource](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-appsync-datasource.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateDataSource](https://docs.aws.amazon.com/appsync/latest/APIReference/API_CreateDataSource.html) API calls in your environment (specifically, the [lambdaConfig](https://docs.aws.amazon.com/appsync/latest/APIReference/API_CreateDataSource.html#appsync-CreateDataSource-request-lambdaConfig) request parameter). If necessary, remediate with the responsive controls of your choice.

## UpdateDataSource
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [UpdateDataSource](https://docs.aws.amazon.com/appsync/latest/APIReference/API_UpdateDataSource.html) allows you to specify a Lambda function that does not belong to your organization as the value for the `lambdaConfig` request parameter. Because the subsequent call against the Lambda function is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [LambdaConfig](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-appsync-datasource.html#cfn-appsync-datasource-lambdaconfig) property that does not belong to your organization for the [AWS::AppSync::DataSource](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-appsync-datasource.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateDataSource](https://docs.aws.amazon.com/appsync/latest/APIReference/API_UpdateDataSource.html) API calls in your environment (specifically, the [lambdaConfig](https://docs.aws.amazon.com/appsync/latest/APIReference/API_UpdateDataSource.html#appsync-UpdateDataSource-request-lambdaConfig) request parameter). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

* AssociateMergedGraphqlApi
* AssociateSourceGraphqlApi
* CreateApi
* CreateApiCache
* CreateApiKey
* CreateChannelNamespace
* CreateDataSource
* CreateFunction
* CreateGraphqlApi
* CreateResolver
* CreateType
* DeleteApi
* DeleteApiKey
* DeleteChannelNamespace
* DeleteDataSource
* DeleteFunction
* DeleteGraphqlApi
* DeleteResolver
* DeleteType
* DisassociateMergedGraphqlApi
* DisassociateSourceGraphqlApi
* FlushApiCache
* GetApi
* GetApiAssociation
* GetApiCache
* GetChannelNamespace
* GetDataSource
* GetDataSourceIntrospection
* GetDomainName
* GetFunction
* GetGraphqlApi
* GetGraphqlApiEnvironmentVariables
* GetIntrospectionSchema
* GetResolver
* GetSchemaCreationStatus
* GetSourceApiAssociation
* GetType
* ListApiKeys
* ListApis
* ListChannelNamespaces
* ListDataSources
* ListDomainNames
* ListFunctions
* ListGraphqlApis
* ListResolvers
* ListResolversByFunction
* ListSourceApiAssociations
* ListTagsForResource
* ListTypes
* ListTypesByAssociation
* PutGraphqlApiEnvironmentVariables
* StartDataSourceIntrospection
* StartSchemaCreation
* StartSchemaMerge
* TagResource
* UntagResource
* UpdateApi
* UpdateApiCache
* UpdateApiKey
* UpdateChannelNamespace
* UpdateDataSource
* UpdateDomainName
* UpdateFunction
* UpdateGraphqlApi
* UpdateResolver
* UpdateSourceApiAssociation
* UpdateType
