# Service-specific guidance: AWS CloudTrail


This document outlines service-specific guidance for implementing a data perimeter for AWS CloudTrail. 


AWS CloudTrail is a service that enables governance, compliance, operational auditing, and risk auditing of your AWS account. It records and logs AWS account activity, including actions taken through the AWS Management Console, AWS SDKs, command line tools, and other AWS services. CloudTrail provides a history of AWS API calls for your account, allowing you to track changes to your AWS resources and troubleshoot operational issues.


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
        
UpdateTrail allows you to specify an S3 bucket that does not belong to your organization as the value for the S3BucketName request parameter. Because the subsequent PutObject call against the bucket is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls:

* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [S3BucketName](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-cloudtrail-trail.html#cfn-cloudtrail-trail-s3bucketname) property that does not belong to your organization for the [AWS::CloudTrail::Trail](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-cloudtrail-trail.html) resource.
* **Detective control example:** Consider using CloudTrail management events to monitor the [UpdateTrail](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_UpdateTrail.html) API calls in your environment (specifically, the [S3BucketName](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_UpdateTrail.html#awscloudtrail-UpdateTrail-request-S3BucketName) request parameter). If necessary, remediate with the responsive controls of your choice.


**Additional consideration 2**

Perimeter type applicability: resource perimeter applied on identity.
        
CreateTrail allows you to specify a KMS key that does not belong to your organization as the value for the KmsKeyId request parameter. Because the subsequent calls against the key are performed by the service principal, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls:

* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [KmsKeyId](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-cloudtrail-trail.html#cfn-cloudtrail-trail-kmskeyid) property that does not belong to your organization for the [AWS::CloudTrail::Trail](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-cloudtrail-trail.html) resource.
* **Detective control example:** Consider using CloudTrail management events to monitor the [CreateTrail](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_CreateTrail.html) API calls in your environment (specifically, the [KmsKeyId](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_CreateTrail.html#awscloudtrail-CreateTrail-request-KmsKeyId) request parameter). If necessary, remediate with the responsive controls of your choice.


**Additional consideration 3**

Perimeter type applicability: resource perimeter applied on identity.
        
CreateTrail allows you to specify an S3 bucket that does not belong to your organization as the value for the S3BucketName request parameter. Because the subsequent PutObject call against the bucket is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls:

* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [S3BucketName](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-cloudtrail-trail.html#cfn-cloudtrail-trail-s3bucketname) property that does not belong to your organization for the [AWS::CloudTrail::Trail](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-cloudtrail-trail.html) resource.
* **Detective control example:** Consider using CloudTrail management events to monitor the [CreateTrail](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_CreateTrail.html) API calls in your environment (specifically, the [S3BucketName](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_CreateTrail.html#awscloudtrail-CreateTrail-request-S3BucketName) request parameter). If necessary, remediate with the responsive controls of your choice.


**Additional consideration 4**

Perimeter type applicability: resource perimeter applied on identity.
        
CreateTrail allows you to specify an SNS topic that does not belong to your organization as the value for the SnsTopicName request parameter. Because the subsequent Publish call against the topic is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls:

* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [SnsTopicName](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-cloudtrail-trail.html#cfn-cloudtrail-trail-snstopicname) property that does not belong to your organization for the [AWS::CloudTrail::Trail](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-cloudtrail-trail.html) resource.
* **Detective control example:** Consider using CloudTrail management events to monitor the [CreateTrail](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_CreateTrail.html) API calls in your environment (specifically, the [SnsTopicName](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_CreateTrail.html#awscloudtrail-CreateTrail-request-SnsTopicName) request parameter). If necessary, remediate with the responsive controls of your choice.


**Additional consideration 5**

Perimeter type applicability: resource perimeter applied on identity.
        
UpdateTrail allows you to specify an SNS topic that does not belong to your organization as the value for the SnsTopicName request parameter. Because the subsequent Publish call against the topic is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls:

* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [SnsTopicName](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-cloudtrail-trail.html#cfn-cloudtrail-trail-snstopicname) property that does not belong to your organization for the [AWS::CloudTrail::Trail](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-cloudtrail-trail.html) resource.
* **Detective control example:** Consider using CloudTrail management events to monitor the [UpdateTrail](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_UpdateTrail.html) API calls in your environment (specifically, the [SnsTopicName](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_UpdateTrail.html#awscloudtrail-UpdateTrail-request-SnsTopicName) request parameter). If necessary, remediate with the responsive controls of your choice.


**Additional consideration 6**

Perimeter type applicability: resource perimeter applied on identity.
        
StartQuery allows you to specify an S3 bucket that does not belong to your organization as the value for the DeliveryS3Uri request parameter. Because the subsequent PutObject call against the bucket is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls:

* **Detective control example:** Consider using CloudTrail management events to monitor the [StartQuery](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_StartQuery.html) API calls in your environment (specifically, the [DeliveryS3Uri](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_StartQuery.html#awscloudtrail-StartQuery-request-DeliveryS3Uri) request parameter). If necessary, remediate with the responsive controls of your choice.


**Additional consideration 7**

Perimeter type applicability: resource perimeter applied on identity.
        
UpdateTrail allows you to specify a KMS key that does not belong to your organization as the value for the KmsKeyId request parameter. Because the subsequent calls against the key are performed by the service principal, they are not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls:

* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [KmsKeyId](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-cloudtrail-trail.html#cfn-cloudtrail-trail-kmskeyid) property that does not belong to your organization for the [AWS::CloudTrail::Trail](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-cloudtrail-trail.html) resource.
* **Detective control example:** Consider using CloudTrail management events to monitor the [UpdateTrail](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_UpdateTrail.html) API calls in your environment (specifically, the [KmsKeyId](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_UpdateTrail.html#awscloudtrail-UpdateTrail-request-KmsKeyId) request parameter). If necessary, remediate with the responsive controls of your choice.


**List of service APIs reviewed against data perimeter control objectives**
* AddTags
* CreateEventDataStore
* CreateTrail
* DeleteEventDataStore
* DeleteTrail
* DescribeTrails
* DisableFederation
* EnableFederation
* GetEventDataStore
* GetEventSelectors
* GetInsightSelectors
* GetTrail
* GetTrailStatus
* ListChannels
* ListEventDataStores
* ListImports
* ListInsightsMetricData
* ListPublicKeys
* ListQueries
* ListTags
* ListTrails
* LookupEvents
* PutInsightSelectors
* RemoveTags
* StartEventDataStoreIngestion
* StartLogging
* StopEventDataStoreIngestion
* StopLogging
* UpdateEventDataStore
* UpdateTrail
