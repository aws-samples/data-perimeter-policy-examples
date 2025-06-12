
# Service-specific guidance: AWS AppSync


This document outlines service-specific guidance for implementing a data perimeter for AWS AppSync. 
AWS AppSync is a fully managed service that enables developers to create scalable GraphQL APIs. It simplifies the process of building applications by allowing you to easily connect to various data sources, including AWS DynamoDB, Lambda, and HTTP APIs. AppSync handles real-time data synchronization and offline programming models, making it ideal for building responsive and collaborative applications across web and mobile platforms.


The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type | Security objective | Applied on | Policy type | Additional considerations |
|----------------|-------------------|------------|-------------|------------------------|
| Identity perimeter | Only trusted identities can access my resources | Resource | RCP | N |
| Identity perimeter | Only trusted identities are allowed from my network | Network | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources | Identity | SCP | Y |
| Resource perimeter | Only trusted resources can be accessed from my network | Network | VPC endpoint policy | N |
| Network perimeter | My identities can access resources only from expected networks | Identity | SCP | N |
| Network perimeter | My resources can be accessed only from expected networks | Resource | RCP | N |

*Y – Additional considerations apply. N – No additional considerations apply.


**Additional consideration 1**

Perimeter type applicability: resource perimeter applied on identity.
        
UpdateDataSource allows you to specify a Lambda function that does not belong to your organization as the value for the lambdaConfig parameter. Because the subsequent call against the function is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls:

* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [LambdaConfig](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-appsync-datasource.html#cfn-appsync-datasource-lambdaconfig) property that does not belong to your organization for the [AWS::AppSync::DataSource](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-appsync-datasource.html) resource.
* **Detective control example:** Consider using CloudTrail management events to monitor the [UpdateDataSource](https://docs.aws.amazon.com/appsync/latest/APIReference/API_UpdateDataSource.html) API calls in your environment (specifically, the [lambdaConfig](https://docs.aws.amazon.com/appsync/latest/APIReference/API_UpdateDataSource.html#appsync-UpdateDataSource-request-lambdaConfig) request parameter). If necessary, remediate with the responsive controls of your  choice.


**Additional consideration 2**

Perimeter type applicability: resource perimeter applied on identity.
        
CreateDataSource allows you to specify a Lambda function that does not belong to your organization as the value for the lambdaConfig parameter. Because the subsequent call against the function is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls:

* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [LambdaConfig](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-appsync-datasource.html#cfn-appsync-datasource-lambdaconfig) property that does not belong to your organization for the [AWS::AppSync::DataSource](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-appsync-datasource.html) resource.
* **Detective control example:** Consider using CloudTrail management events to monitor the [CreateDataSource](https://docs.aws.amazon.com/appsync/latest/APIReference/API_CreateDataSource.html) API calls in your environment (specifically, the [lambdaConfig](https://docs.aws.amazon.com/appsync/latest/APIReference/API_CreateDataSource.html#appsync-CreateDataSource-request-lambdaConfig) request parameter). If necessary, remediate with the responsive controls of your  choice.





**List of service APIs reviewed against data perimeter control objectives**

* CreateGraphqlApi

* CreateDataSource

* CreateApiCache

* CreateFunction

* UpdateDomainName

* CreateType

* FlushApiCache

* CreateApiKey

* CreateResolver

* PutGraphqlApiEnvironmentVariables

* AssociateMergedGraphqlApi

* AssociateSourceGraphqlApi

* StartDataSourceIntrospection

* StartSchemaCreation

* StartSchemaMerge

* TagResource

* UpdateApiCache

* UpdateApiKey

* UpdateDataSource

* UpdateFunction

* UpdateGraphqlApi

* UpdateResolver

* UpdateSourceApiAssociation

* UpdateType

* ListApiKeys

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

* GetApiAssociation

* GetApiCache

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

* UntagResource

* DisassociateMergedGraphqlApi

* DisassociateSourceGraphqlApi

* DeleteApiKey

* DeleteFunction

* DeleteResolver

* DeleteType

* DeleteDataSource

* DeleteGraphqlApi


