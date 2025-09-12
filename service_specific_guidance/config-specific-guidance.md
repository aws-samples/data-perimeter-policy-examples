
# Service-specific guidance: AWS Config


This document outlines service-specific guidance for implementing a data perimeter for AWS Config. 

AWS Config is a service that enables you to assess, audit, and evaluate the configurations of your AWS resources. It provides a detailed view of the configuration of AWS resources in your account, including how they are related to one another and how they have changed over time. This helps you simplify compliance auditing, security analysis, change management, and operational troubleshooting.


The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type | Security objective | Applied on | Policy type | Additional considerations |
|----------------|-------------------|------------|-------------|------------------------|
| Identity perimeter | Only trusted identities can access my resources | Resource | RCP | Y |
| Identity perimeter | Only trusted identities are allowed from my network | Network | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources | Identity | SCP | Y |
| Resource perimeter | Only trusted resources can be accessed from my network | Network | VPC endpoint policy | N |
| Network perimeter | My identities can access resources only from expected networks | Identity | SCP | N |
| Network perimeter | My resources can be accessed only from expected networks | Resource | RCP | N |

*Y – Additional considerations apply. N – No additional considerations apply.
 



**Additional consideration 1**

Perimeter type applicability: identity perimeter applied on resource; resource perimeter applied on identity.
        
PutConfigurationAggregator allows you to add another account to your aggregator.

See ["Sid":"PreventExternalResourceShare"](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies#sidpreventexternalresourceshare) for a list of resources that can be granted cross-account access.

If you want to restrict access so that only trusted identities can take actions against your resources, consider implementing these additional controls:

* **Preventative control example:** Consider restricting [PutConfigurationAggregator](https://docs.aws.amazon.com/config/latest/APIReference/API_PutConfigurationAggregator.html) permissions to administrators only using an SCP. See [data_perimeter_governance_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/data_perimeter_governance_scp.json) for an example policy.
* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [AccountAggregationSources](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-config-configurationaggregator.html#cfn-config-configurationaggregator-accountaggregationsources) property that grants permissions to untrusted identities for the [AWS::Config::ConfigurationAggregator](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-config-configurationaggregator.html) resource.
* **Detective control example:** Consider using CloudTrail management events to monitor the [PutConfigurationAggregator](https://docs.aws.amazon.com/config/latest/APIReference/API_PutConfigurationAggregator.html) API calls in your environment (specifically, the [AccountAggregationSources](https://docs.aws.amazon.com/config/latest/APIReference/API_PutConfigurationAggregator.html#config-PutConfigurationAggregator-request-AccountAggregationSources) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access so that your identities cannot view resources that were shared with your accounts by untrusted entities, consider implementing this additional control:

* **Detective control example:** Consider using [DescribeConfigurationAggregators](https://docs.aws.amazon.com/config/latest/APIReference/API_DescribeConfigurationAggregators.html) to monitor the list of source accounts configurated for your aggregators (specifically, the [AccountAggregationSources](https://docs.aws.amazon.com/config/latest/APIReference/API_DescribeConfigurationAggregators.html#API_DescribeConfigurationAggregators_ResponseSyntax) response parameter). If necessary, remediate with the responsive controls of your choice.


**Additional consideration 2**

Perimeter type applicability: resource perimeter applied on identity.
        
PutDeliveryChannel allows you to specify an S3 bucket and SNS topic that do not belong to your organization as the values for the S3BucketName and snsTopicARN request parameters. Because the subsequent call against S3 bucket and SNS topic is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls:

* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [S3BucketName](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-config-deliverychannel.html#cfn-config-deliverychannel-s3bucketname) and [SnsTopicARN](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-config-deliverychannel.html#cfn-config-deliverychannel-snstopicarn) properties that do not belong to your organization for the [AWS::Config::DeliveryChannel](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-config-deliverychannel.html) resource.
* **Detective control example:** Consider using CloudTrail management events to monitor the [PutDeliveryChannel](https://docs.aws.amazon.com/config/latest/APIReference/API_PutDeliveryChannel.html) API calls in your environment (specifically, the [s3BucketName](https://docs.aws.amazon.com/config/latest/APIReference/API_PutDeliveryChannel.html#API_PutDeliveryChannel_RequestSyntax) request parameter). If necessary, remediate with the responsive controls of your  choice.


**Additional consideration 3**

Perimeter type applicability: resource perimeter applied on identity.
        
StartResourceEvaluation allows you to specify an S3 bucket that does not belong to your organization as the value for the ResourceId request parameter. Because the subsequent call against the bucket is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls:

* **Detective control example:** Consider using CloudTrail management events to monitor the [StartResourceEvaluation](https://docs.aws.amazon.com/config/latest/APIReference/API_StartResourceEvaluation.html) API calls in your environment (specifically, the [ResourceId](https://docs.aws.amazon.com/config/latest/APIReference/API_ResourceDetails.html#config-Type-ResourceDetails-ResourceId) request parameter). If necessary, remediate with the responsive controls of your  choice.


**Additional consideration 4**

Perimeter type applicability: resource perimeter applied on identity.
        
PutOrganizationConfigRule allows you to specify a Lambda function that does not belong to your organization as the value for the LambdaFunctionArn request parameter. Because the subsequent Invoke call against the function is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls:

* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [LambdaFunctionArn](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-config-organizationconfigrule-organizationcustomrulemetadata.html#cfn-config-organizationconfigrule-organizationcustomrulemetadata-lambdafunctionarn) property that does not belong to your organization for the [AWS::Config::OrganizationConfigRule](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-config-organizationconfigrule.html) resource.
* **Detective control example:** Consider using CloudTrail management events to monitor the [PutOrganizationConfigRule](https://docs.aws.amazon.com/config/latest/APIReference/API_PutOrganizationConfigRule.html) API calls in your environment (specifically, the [LambdaFunctionArn](https://docs.aws.amazon.com/config/latest/APIReference/API_OrganizationCustomRuleMetadata.html#config-Type-OrganizationCustomRuleMetadata-LambdaFunctionArn) request parameter). If necessary, remediate with the responsive controls of your  choice.


**Additional consideration 5**

Perimeter type applicability: resource perimeter applied on identity.
        
PutConfigRule allows you to specify a Lambda function that does not belong to your organization as the value for the SourceIdentifier request parameter. Because the subsequent Invoke call against the function is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls:

* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [SourceIdentifier](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-config-configrule-source.html#cfn-config-configrule-source-sourceidentifier) property that does not belong to your organization for the [AWS::Config::ConfigRule](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-config-configrule.html) resource.
* **Detective control example:** Consider using CloudTrail management events to monitor the [PutConfigRule](https://docs.aws.amazon.com/config/latest/APIReference/API_PutConfigRule.html) API calls in your environment (specifically, the [SourceIdentifier](https://docs.aws.amazon.com/config/latest/APIReference/API_Source.html#config-Type-Source-SourceIdentifier) request parameter). If necessary, remediate with the responsive controls of your  choice.


**Additional consideration 6**

Perimeter type applicability: identity perimeter applied on resource; resource perimeter applied on identity.
        
PutAggregationAuthorization allows you to authorize another account to collect data from your account.

See ["Sid":"PreventExternalResourceShare"](https://github.com/aws-samples/data-perimeter-policy-examples/tree/main/service_control_policies#sidpreventexternalresourceshare) for a list of resources that can be granted cross-account access.

If you want to restrict access so that only trusted identities can take actions against your resources, consider implementing these additional controls:

* **Preventative control example:** Consider restricting [PutAggregationAuthorization](https://docs.aws.amazon.com/config/latest/APIReference/API_PutAggregationAuthorization.html) permissions to administrators only using an SCP. See [data_perimeter_governance_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/data_perimeter_governance_scp.json) for an example policy.
* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [AuthorizedAccountId](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-config-aggregationauthorization.html#cfn-config-aggregationauthorization-authorizedaccountid) property that grants permissions to untrusted identities for the [AWS::Config::AggregationAuthorization](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-config-aggregationauthorization.html) resource.
* **Detective control example:** Consider using CloudTrail management events to monitor the [PutAggregationAuthorization](https://docs.aws.amazon.com/config/latest/APIReference/API_PutAggregationAuthorization.html) API calls in your environment (specifically, the [AuthorizedAccountId](https://docs.aws.amazon.com/config/latest/APIReference/API_PutAggregationAuthorization.html#config-PutAggregationAuthorization-request-AuthorizedAccountId) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access so that your identities cannot view resources that were shared with your accounts by untrusted entities, consider implementing this additional control:

* **Detective control example:** Consider using [DescribeConfigurationAggregators](https://docs.aws.amazon.com/config/latest/APIReference/API_DescribeConfigurationAggregators.html) to monitor the list of source accounts configurated for your aggregators (specifically, the [AccountAggregationSources](https://docs.aws.amazon.com/config/latest/APIReference/API_DescribeConfigurationAggregators.html#API_DescribeConfigurationAggregators_ResponseSyntax) response parameter). If necessary, remediate with the responsive controls of your choice.

**Additional consideration 7**

Perimeter type applicability: resource perimeter applied on identity.
        
PutConformancePack allows you to specify an S3 bucket that does not belong to your organization as the value for the TemplateS3Uri request parameter. Because the subsequent call against the bucket is performed by the service-linked role (SLR), it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls:

* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [TemplateS3Uri](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-config-conformancepack.html#cfn-config-conformancepack-templates3uri) property that does not belong to your organization for the [AWS::Config::ConformancePack](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-config-conformancepack.html) resource.
* **Detective control example:** Consider using CloudTrail management events to monitor the [PutConformancePack](https://docs.aws.amazon.com/config/latest/APIReference/API_PutConformancePack.html) API calls in your environment (specifically, the [TemplateS3Uri](https://docs.aws.amazon.com/config/latest/APIReference/API_PutConformancePack.html#config-PutConformancePack-request-TemplateS3Uri) request parameter). If necessary, remediate with the responsive controls of your  choice.

**Additional consideration 8**

Perimeter type applicability: resource perimeter applied on identity.
        
PutOrganizationConformancePack allows you to specify an S3 bucket that does not belong to your organization as the value for theTemplateS3Uri request parameter. Because the subsequent call against the bucket is performed by the service-linked role (SLR), it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls:

* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [TemplateS3Uri](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-config-organizationconformancepack.html#cfn-config-organizationconformancepack-templates3uri) property that does not belong to your organization for the [AWS::Config::OrganizationConformancePack](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-config-organizationconformancepack.html) resource.
* **Detective control example:** Consider using CloudTrail management events to monitor the [PutOrganizationConformancePack](https://docs.aws.amazon.com/config/latest/APIReference/API_PutOrganizationConformancePack.html) API calls in your environment (specifically, the [TemplateS3Uri](https://docs.aws.amazon.com/config/latest/APIReference/API_PutOrganizationConformancePack.html#config-PutOrganizationConformancePack-request-TemplateS3Uri) request parameter). If necessary, remediate with the responsive controls of your  choice.


**List of service APIs reviewed against data perimeter control objectives**

* BatchGetAggregateResourceConfig
* BatchGetResourceConfig
* DeleteAggregationAuthorization
* DeleteConfigRule
* DeleteConfigurationAggregator
* DeleteConformancePack
* DeleteEvaluationResults
* DeleteOrganizationConfigRule
* DeleteOrganizationConformancePack
* DeletePendingAggregationRequest
* DeleteRemediationConfiguration
* DeleteRetentionConfiguration
* DeliverConfigSnapshot
* DescribeAggregateComplianceByConfigRules
* DescribeAggregateComplianceByConformancePacks
* DescribeAggregationAuthorizations
* DescribeComplianceByConfigRule
* DescribeComplianceByResource
* DescribeConfigRuleEvaluationStatus
* DescribeConfigRules
* DescribeConfigurationAggregatorSourcesStatus
* DescribeConfigurationAggregators
* DescribeConfigurationRecorderStatus
* DescribeConfigurationRecorders
* DescribeConformancePackCompliance
* DescribeConformancePackStatus
* DescribeConformancePacks
* DescribeDeliveryChannelStatus
* DescribeDeliveryChannels
* DescribeOrganizationConfigRuleStatuses
* DescribeOrganizationConfigRules
* DescribeOrganizationConformancePackStatuses
* DescribeOrganizationConformancePacks
* DescribePendingAggregationRequests
* DescribeRemediationConfigurations
* DescribeRemediationExecutionStatus
* DescribeRetentionConfigurations
* GetAggregateComplianceDetailsByConfigRule
* GetAggregateConfigRuleComplianceSummary
* GetAggregateConformancePackComplianceSummary
* GetAggregateDiscoveredResourceCounts
* GetAggregateResourceConfig
* GetComplianceDetailsByConfigRule
* GetComplianceDetailsByResource
* GetComplianceSummaryByConfigRule
* GetComplianceSummaryByResourceType
* GetConformancePackComplianceDetails
* GetConformancePackComplianceSummary
* GetCustomRulePolicy
* GetDiscoveredResourceCounts
* GetOrganizationConfigRuleDetailedStatus
* GetOrganizationConformancePackDetailedStatus
* GetResourceConfigHistory
* GetResourceEvaluationSummary
* GetStoredQuery
* ListAggregateDiscoveredResources
* ListConformancePackComplianceScores
* ListDiscoveredResources
* ListResourceEvaluations
* ListStoredQueries
* ListTagsForResource
* PutAggregationAuthorization
* PutConfigRule
* PutConfigurationAggregator
* PutConformancePack
* PutOrganizationConfigRule
* PutOrganizationConformancePack
* PutRemediationConfigurations
* PutRetentionConfiguration
* PutStoredQuery
* SelectAggregateResourceConfig
* SelectResourceConfig
* StartConfigRulesEvaluation
* StartConfigurationRecorder
* StartRemediationExecution
* StartResourceEvaluation
* StopConfigurationRecorder
* TagResource
* UntagResource


