# Service-specific guidance: AWS CloudTrail


This document outlines service-specific guidance for implementing a data perimeter for AWS CloudTrail.

AWS CloudTrail is an AWS service that helps you enable operational and risk auditing, governance, and compliance of your AWS account. Actions taken by a user, role, or an AWS service are recorded as events in CloudTrail, including actions taken in the AWS Management Console, AWS Command Line Interface, and AWS SDKs and APIs.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | Y |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | Y |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | N |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | N |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | Y |

*Y - Additional considerations apply. N - No additional considerations apply.

## CreateTrail
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [CreateTrail](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_CreateTrail.html) allows you to specify a KMS key that does not belong to your organization as the value for the `KmsKeyId` request parameter. Because the subsequent call against the KMS key is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [KMSKeyId](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-cloudtrail-trail.html#cfn-cloudtrail-trail-kmskeyid) property that does not belong to your organization for the [AWS::CloudTrail::Trail](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-cloudtrail-trail.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateTrail](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_CreateTrail.html) API calls in your environment (specifically, the [KmsKeyId](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_CreateTrail.html#awscloudtrail-CreateTrail-request-KmsKeyId) request parameter). If necessary, remediate with the responsive controls of your choice.

### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [CreateTrail](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_CreateTrail.html) allows you to specify an S3 bucket that does not belong to your organization as the value for the `S3BucketName` request parameter. Because the subsequent call against the S3 bucket is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [S3BucketName](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-cloudtrail-trail.html#cfn-cloudtrail-trail-s3bucketname) property that does not belong to your organization for the [AWS::CloudTrail::Trail](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-cloudtrail-trail.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateTrail](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_CreateTrail.html) API calls in your environment (specifically, the [S3BucketName](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_CreateTrail.html#awscloudtrail-CreateTrail-request-S3BucketName) request parameter). If necessary, remediate with the responsive controls of your choice.

### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [CreateTrail](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_CreateTrail.html) allows you to specify an SNS topic that does not belong to your organization as the value for the `SnsTopicName` request parameter. Because the subsequent call against the SNS topic is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [SnsTopicName](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-cloudtrail-trail.html#cfn-cloudtrail-trail-snstopicname) property that does not belong to your organization for the [AWS::CloudTrail::Trail](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-cloudtrail-trail.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateTrail](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_CreateTrail.html) API calls in your environment (specifically, the [SnsTopicName](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_CreateTrail.html#awscloudtrail-CreateTrail-request-SnsTopicName) request parameter). If necessary, remediate with the responsive controls of your choice.

## PutResourcePolicy
### No RCP support

**Perimeter type applicability**: identity and network perimeter applied on resource.

**Description**: [PutResourcePolicy](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_PutResourcePolicy.html) allows you to apply a resource-based policy to grant access to a CloudTrail event data store, dashboard, or channel. The service currently does not support RCPs.

**Additional controls**:

If you want to restrict access to trusted identities, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutResourcePolicy](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_PutResourcePolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [ResourcePolicy](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-cloudtrail-resourcepolicy.html#cfn-cloudtrail-resourcepolicy-resourcepolicy) property for the [AWS::CloudTrail::ResourcePolicy](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-cloudtrail-resourcepolicy.html) resource that grants permissions to untrusted identities.
* Detective control example: Consider using CloudTrail management events to monitor the [PutResourcePolicy](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_PutResourcePolicy.html) API calls in your environment (specifically, the [ResourcePolicy](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_PutResourcePolicy.html#awscloudtrail-PutResourcePolicy-request-ResourcePolicy) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutResourcePolicy](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_PutResourcePolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [ResourcePolicy](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-cloudtrail-resourcepolicy.html#cfn-cloudtrail-resourcepolicy-resourcepolicy) property for the [AWS::CloudTrail::ResourcePolicy](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-cloudtrail-resourcepolicy.html) resource that grants permissions to unexpected networks.
* Detective control example: Consider using CloudTrail management events to monitor the [PutResourcePolicy](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_PutResourcePolicy.html) API calls in your environment (specifically, the [ResourcePolicy](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_PutResourcePolicy.html#awscloudtrail-PutResourcePolicy-request-ResourcePolicy) request parameter). If necessary, remediate with the responsive controls of your choice.

## StartQuery
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [StartQuery](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_StartQuery.html) allows you to specify an S3 bucket that does not belong to your organization as the value for the `DeliveryS3Uri` request parameter. Because the subsequent call against the S3 bucket is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing this additional control:
* Detective control example: Consider using CloudTrail management events to monitor the [StartQuery](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_StartQuery.html) API calls in your environment (specifically, the [DeliveryS3Uri](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_StartQuery.html#awscloudtrail-StartQuery-request-DeliveryS3Uri) request parameter). If necessary, remediate with the responsive controls of your choice.

## UpdateTrail
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [UpdateTrail](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_UpdateTrail.html) allows you to specify a KMS key that does not belong to your organization as the value for the `KmsKeyId` request parameter. Because the subsequent call against the KMS key is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [KMSKeyId](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-cloudtrail-trail.html#cfn-cloudtrail-trail-kmskeyid) property that does not belong to your organization for the [AWS::CloudTrail::Trail](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-cloudtrail-trail.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateTrail](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_UpdateTrail.html) API calls in your environment (specifically, the [KmsKeyId](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_UpdateTrail.html#awscloudtrail-UpdateTrail-request-KmsKeyId) request parameter). If necessary, remediate with the responsive controls of your choice.

### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [UpdateTrail](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_UpdateTrail.html) allows you to specify an S3 bucket that does not belong to your organization as the value for the `S3BucketName` request parameter. Because the subsequent call against the S3 bucket is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [S3BucketName](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-cloudtrail-trail.html#cfn-cloudtrail-trail-s3bucketname) property that does not belong to your organization for the [AWS::CloudTrail::Trail](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-cloudtrail-trail.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateTrail](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_UpdateTrail.html) API calls in your environment (specifically, the [S3BucketName](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_UpdateTrail.html#awscloudtrail-UpdateTrail-request-S3BucketName) request parameter). If necessary, remediate with the responsive controls of your choice.

### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [UpdateTrail](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_UpdateTrail.html) allows you to specify an SNS topic that does not belong to your organization as the value for the `SnsTopicName` request parameter. Because the subsequent call against the SNS topic is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [SnsTopicName](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-cloudtrail-trail.html#cfn-cloudtrail-trail-snstopicname) property that does not belong to your organization for the [AWS::CloudTrail::Trail](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-cloudtrail-trail.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateTrail](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_UpdateTrail.html) API calls in your environment (specifically, the [SnsTopicName](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_UpdateTrail.html#awscloudtrail-UpdateTrail-request-SnsTopicName) request parameter). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

* AddTags
* CreateDashboard
* CreateEventDataStore
* CreateTrail
* DeleteDashboard
* DeleteEventDataStore
* DeleteTrail
* DescribeTrails
* DisableFederation
* EnableFederation
* GenerateQuery
* GetDashboard
* GetEventConfiguration
* GetEventDataStore
* GetEventSelectors
* GetInsightSelectors
* GetTrail
* GetTrailStatus
* ListChannels
* ListDashboards
* ListEventDataStores
* ListImports
* ListInsightsMetricData
* ListPublicKeys
* ListQueries
* ListTags
* ListTrails
* LookupEvents
* PutEventConfiguration
* PutInsightSelectors
* PutResourcePolicy
* RemoveTags
* SearchSampleQueries
* StartDashboardRefresh
* StartEventDataStoreIngestion
* StartLogging
* StartQuery
* StopEventDataStoreIngestion
* StopLogging
* UpdateDashboard
* UpdateEventDataStore
* UpdateTrail
