# Service-specific guidance: AWS Cost Explorer Service


This document outlines service-specific guidance for implementing a data perimeter for AWS Cost Explorer Service.

AWS Cost Explorer is a tool that enables you to view and analyze your AWS costs and usage, with up to the last 13 months of data, forecasts of likely spend for the next 18 months, and Reserved Instance purchase recommendations. The Cost Explorer API lets you programmatically query your cost and usage data and manage related resources such as cost categories, cost anomaly monitors, and anomaly subscriptions.

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

## CreateAnomalySubscription
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [CreateAnomalySubscription](https://docs.aws.amazon.com/aws-cost-management/latest/APIReference/API_CreateAnomalySubscription.html) allows you to specify an Amazon SNS topic that does not belong to your organization as the value for the `AnomalySubscription.Subscribers[].Address` request parameter when the subscriber `Type` is `SNS`. Because the subsequent call against the Amazon SNS topic is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Subscribers.Address](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-ce-anomalysubscription-subscriber.html#cfn-ce-anomalysubscription-subscriber-address) property that does not belong to your organization for the [AWS::CE::AnomalySubscription](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-ce-anomalysubscription.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateAnomalySubscription](https://docs.aws.amazon.com/aws-cost-management/latest/APIReference/API_CreateAnomalySubscription.html) API calls in your environment (specifically, the [AnomalySubscription.Subscribers[].Address](https://docs.aws.amazon.com/aws-cost-management/latest/APIReference/API_Subscriber.html#awscostmanagement-Type-Subscriber-Address) request parameter). If necessary, remediate with the responsive controls of your choice.

### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [CreateAnomalySubscription](https://docs.aws.amazon.com/aws-cost-management/latest/APIReference/API_CreateAnomalySubscription.html) allows you to specify an email address as the value for the `AnomalySubscription.Subscribers[].Address` request parameter when the subscriber `Type` is `EMAIL`. Because the subsequent requests against the email address are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Subscribers.Address](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-ce-anomalysubscription-subscriber.html#cfn-ce-anomalysubscription-subscriber-address) property that does not belong to your organization for the [AWS::CE::AnomalySubscription](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-ce-anomalysubscription.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateAnomalySubscription](https://docs.aws.amazon.com/aws-cost-management/latest/APIReference/API_CreateAnomalySubscription.html) API calls in your environment (specifically, the [AnomalySubscription.Subscribers[].Address](https://docs.aws.amazon.com/aws-cost-management/latest/APIReference/API_Subscriber.html#awscostmanagement-Type-Subscriber-Address) request parameter). If necessary, remediate with the responsive controls of your choice.

## UpdateAnomalySubscription
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [UpdateAnomalySubscription](https://docs.aws.amazon.com/aws-cost-management/latest/APIReference/API_UpdateAnomalySubscription.html) allows you to specify an Amazon SNS topic that does not belong to your organization as the value for the `Subscribers[].Address` request parameter when the subscriber `Type` is `SNS`. Because the subsequent call against the Amazon SNS topic is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Subscribers.Address](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-ce-anomalysubscription-subscriber.html#cfn-ce-anomalysubscription-subscriber-address) property that does not belong to your organization for the [AWS::CE::AnomalySubscription](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-ce-anomalysubscription.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateAnomalySubscription](https://docs.aws.amazon.com/aws-cost-management/latest/APIReference/API_UpdateAnomalySubscription.html) API calls in your environment (specifically, the [Subscribers[].Address](https://docs.aws.amazon.com/aws-cost-management/latest/APIReference/API_Subscriber.html#awscostmanagement-Type-Subscriber-Address) request parameter). If necessary, remediate with the responsive controls of your choice.

### Configuration of a non-AWS resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [UpdateAnomalySubscription](https://docs.aws.amazon.com/aws-cost-management/latest/APIReference/API_UpdateAnomalySubscription.html) allows you to specify an email address as the value for the `Subscribers[].Address` request parameter when the subscriber `Type` is `EMAIL`. Because the subsequent requests against the email address are not governed by IAM, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Subscribers.Address](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-ce-anomalysubscription-subscriber.html#cfn-ce-anomalysubscription-subscriber-address) property that does not belong to your organization for the [AWS::CE::AnomalySubscription](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-ce-anomalysubscription.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateAnomalySubscription](https://docs.aws.amazon.com/aws-cost-management/latest/APIReference/API_UpdateAnomalySubscription.html) API calls in your environment (specifically, the [Subscribers[].Address](https://docs.aws.amazon.com/aws-cost-management/latest/APIReference/API_Subscriber.html#awscostmanagement-Type-Subscriber-Address) request parameter). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

The service APIs covered in this review are based on botocore version [1.43.29](https://github.com/boto/botocore/tree/1.43.29/botocore).

* CreateAnomalyMonitor
* CreateAnomalySubscription
* CreateCostCategoryDefinition
* DeleteAnomalyMonitor
* DeleteCostCategoryDefinition
* DescribeCostCategoryDefinition
* GetAnomalies
* GetAnomalyMonitors
* GetAnomalySubscriptions
* GetApproximateUsageRecords
* GetCommitmentPurchaseAnalysis
* GetCostAndUsage
* GetCostAndUsageComparisons
* GetCostCategories
* GetCostComparisonDrivers
* GetCostForecast
* GetDimensionValues
* GetReservationCoverage
* GetReservationPurchaseRecommendation
* GetReservationUtilization
* GetRightsizingRecommendation
* GetSavingsPlansCoverage
* GetSavingsPlansPurchaseRecommendation
* GetSavingsPlansUtilization
* GetSavingsPlansUtilizationDetails
* GetTags
* GetUsageForecast
* ListCommitmentPurchaseAnalyses
* ListCostCategoryDefinitions
* ListSavingsPlansPurchaseRecommendationGeneration
* ListTagsForResource
* StartCommitmentPurchaseAnalysis
* StartSavingsPlansPurchaseRecommendationGeneration
* TagResource
* UntagResource
* UpdateAnomalyMonitor
* UpdateAnomalySubscription
* UpdateCostCategoryDefinition
