# Service-specific guidance: Amazon Simple Storage Service


This document outlines service-specific guidance for implementing a data perimeter for Amazon Simple Storage Service.

Amazon Simple Storage Service (Amazon S3) is a scalable object storage service that offers industry-leading durability, availability, and performance. It allows you to store and retrieve any amount of data from anywhere on the web, making it ideal for a wide range of use cases such as data lakes, websites, mobile applications, backup and restore, archive, and big data analytics.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | N |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | Y |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | Y |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | N |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | N |

*Y - Additional considerations apply. N - No additional considerations apply.

## GetObject
### Access to service-owned resources

**Perimeter type applicability**: resource perimeter.

**Description**: [GetObject](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetObject.html) might require access to service-owned buckets, which are buckets that are managed in service accounts.

**Additional controls**:

If you want to restrict access to trusted resources for your identities, consider implementing this control:
* Preventative control example: Consider implementing the default resource perimeter SCP, [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json), which lists service-owned buckets in the `NotResource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access.

If you want to restrict access to your networks to trusted resources, consider implementing this control:
* Preventative control example: Consider implementing the service-specific VPC endpoint policy, [s3_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/s3_endpoint_policy.json), which lists service-owned buckets in the `Resource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that might be accessed from your networks.

## GetObjectVersion
### Access to service-owned resources

**Perimeter type applicability**: resource perimeter.

**Description**: [GetObjectVersion](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetObject.html) might require access to service-owned buckets, which are buckets that are managed in service accounts.

**Additional controls**:

If you want to restrict access to trusted resources for your identities, consider implementing this control:
* Preventative control example: Consider implementing the default resource perimeter SCP, [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json), which lists service-owned buckets in the `NotResource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access.

If you want to restrict access to your networks to trusted resources, consider implementing this control:
* Preventative control example: Consider implementing the service-specific VPC endpoint policy, [s3_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/s3_endpoint_policy.json), which lists service-owned buckets in the `Resource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that might be accessed from your networks.

## HeadBucket
### Access to service-owned resources

**Perimeter type applicability**: resource perimeter.

**Description**: [HeadBucket](https://docs.aws.amazon.com/AmazonS3/latest/API/API_HeadBucket.html) might require access to service-owned buckets, which are buckets that are managed in service accounts.

**Additional controls**:

If you want to restrict access to trusted resources for your identities, consider implementing this control:
* Preventative control example: Consider implementing the default resource perimeter SCP, [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json), which lists service-owned buckets in the `NotResource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access.

If you want to restrict access to your networks to trusted resources, consider implementing this control:
* Preventative control example: Consider implementing the service-specific VPC endpoint policy, [s3_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/s3_endpoint_policy.json), which lists service-owned buckets in the `Resource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that might be accessed from your networks.

## ListBucket
### Access to service-owned resources

**Perimeter type applicability**: resource perimeter.

**Description**: [ListBucket](https://docs.aws.amazon.com/AmazonS3/latest/API/API_ListObjectsV2.html) might require access to service-owned buckets, which are buckets that are managed in service accounts.

**Additional controls**:

If you want to restrict access to trusted resources for your identities, consider implementing this control:
* Preventative control example: Consider implementing the default resource perimeter SCP, [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json), which lists service-owned buckets in the `NotResource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access.

If you want to restrict access to your networks to trusted resources, consider implementing this control:
* Preventative control example: Consider implementing the service-specific VPC endpoint policy, [s3_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/s3_endpoint_policy.json), which lists service-owned buckets in the `Resource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that might be accessed from your networks.

## PutBucketInventoryConfiguration
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [PutBucketInventoryConfiguration](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketInventoryConfiguration.html) allows you to specify an S3 bucket that does not belong to your organization as the value for the `Destination` request parameter. Because the subsequent call against the S3 bucket is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [BucketArn](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-s3-bucket-destination.html#cfn-s3-bucket-destination-bucketarn) property that does not belong to your organization for the [AWS::S3::Bucket](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-s3-bucket.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [PutBucketInventoryConfiguration](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketInventoryConfiguration.html) API calls in your environment (specifically, the [Destination](https://docs.aws.amazon.com/AmazonS3/latest/API/API_InventoryDestination.html) request body parameter). If necessary, remediate with the responsive controls of your choice.

## PutBucketLogging
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [PutBucketLogging](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketLogging.html) allows you to specify an S3 bucket that does not belong to your organization as the value for the `TargetBucket` request parameter. Because the subsequent call against the S3 bucket is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [DestinationBucketName](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-s3-bucket-loggingconfiguration.html#cfn-s3-bucket-loggingconfiguration-destinationbucketname) property that does not belong to your organization for the [AWS::S3::Bucket](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-s3-bucket.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [PutBucketLogging](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketLogging.html) API calls in your environment (specifically, the [TargetBucket](https://docs.aws.amazon.com/AmazonS3/latest/API/API_LoggingEnabled.html) request parameter). If necessary, remediate with the responsive controls of your choice.

## PutBucketNotification
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [PutBucketNotification](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketNotification.html) allows you to specify a resource, such as an SNS topic, an SQS queue, or a Lambda function that does not belong to your organization as the value for the `NotificationConfiguration` request parameter. Because the subsequent call against the resource is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [NotificationConfiguration](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-s3-bucket.html#cfn-s3-bucket-notificationconfiguration) property that does not belong to your organization for the [AWS::S3::Bucket](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-s3-bucket.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [PutBucketNotification](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketNotification.html) API calls in your environment (specifically, the [NotificationConfiguration](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketNotification.html#API_PutBucketNotification_RequestSyntax) request parameter). If necessary, remediate with the responsive controls of your choice.

## PutBucketNotificationConfiguration
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [PutBucketNotificationConfiguration](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketNotificationConfiguration.html) allows you to specify a resource, such as an SNS topic, SQS queue, or Lambda function, that does not belong to your organization as the value for the `NotificationConfiguration` request parameter. Because the subsequent call against the resource is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [NotificationConfiguration](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-s3-bucket.html#cfn-s3-bucket-notificationconfiguration) property that does not belong to your organization for the [AWS::S3::Bucket](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-s3-bucket.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [PutBucketNotificationConfiguration](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketNotificationConfiguration.html) API calls in your environment (specifically, the [NotificationConfiguration](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketNotificationConfiguration.html#API_PutBucketNotificationConfiguration_RequestSyntax) request parameter). If necessary, remediate with the responsive controls of your choice.

## PutObject
### Access to service-owned resources

**Perimeter type applicability**: resource perimeter.

**Description**: [PutObject](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutObject.html) might require access to service-owned buckets, which are buckets that are managed in service accounts.

**Additional controls**:

If you want to restrict access to trusted resources for your identities, consider implementing this control:
* Preventative control example: Consider implementing the default resource perimeter SCP, [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json), which lists service-owned buckets in the `NotResource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access.

If you want to restrict access to your networks to trusted resources, consider implementing this control:
* Preventative control example: Consider implementing the service-specific VPC endpoint policy, [s3_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/s3_endpoint_policy.json), which lists service-owned buckets in the `Resource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that might be accessed from your networks.

## PutObjectAcl
### Access to service-owned resources

**Perimeter type applicability**: resource perimeter.

**Description**: [PutObjectAcl](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutObjectAcl.html) might require access to service-owned buckets, which are buckets that are managed in service accounts.

**Additional controls**:

If you want to restrict access to trusted resources for your identities, consider implementing this control:
* Preventative control example: Consider implementing the default resource perimeter SCP, [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json), which lists service-owned buckets in the `NotResource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access.

If you want to restrict access to your networks to trusted resources, consider implementing this control:
* Preventative control example: Consider implementing the service-specific VPC endpoint policy, [s3_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/s3_endpoint_policy.json), which lists service-owned buckets in the `Resource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that might be accessed from your networks.

## List of service APIs reviewed against data perimeter control objectives

* AbortMultipartUpload
* CompleteMultipartUpload
* CopyObject
* CreateBucket
* CreateBucketMetadataConfiguration
* CreateMultipartUpload
* CreateSession
* DeleteBucketAnalyticsConfiguration
* DeleteBucketCors
* DeleteBucketEncryption
* DeleteBucketIntelligentTieringConfiguration
* DeleteBucketInventoryConfiguration
* DeleteBucketLifecycle
* DeleteBucketMetadataConfiguration
* DeleteBucketMetricsConfiguration
* DeleteBucketOwnershipControls
* DeleteBucketPolicy
* DeleteBucketReplication
* DeleteBucketTagging
* DeleteBucketWebsite
* DeleteObject
* DeleteObjectTagging
* DeleteObjects
* DeletePublicAccessBlock
* GetBucketAccelerateConfiguration
* GetBucketAcl
* GetBucketAnalyticsConfiguration
* GetBucketCors
* GetBucketEncryption
* GetBucketIntelligentTieringConfiguration
* GetBucketInventoryConfiguration
* GetBucketLifecycle
* GetBucketLifecycleConfiguration
* GetBucketLocation
* GetBucketLogging
* GetBucketMetadataConfiguration
* GetBucketMetricsConfiguration
* GetBucketNotification
* GetBucketNotificationConfiguration
* GetBucketOwnershipControls
* GetBucketPolicy
* GetBucketPolicyStatus
* GetBucketReplication
* GetBucketRequestPayment
* GetBucketTagging
* GetBucketVersioning
* GetBucketWebsite
* GetObject
* GetObjectAcl
* GetObjectAttributes
* GetObjectLegalHold
* GetObjectLockConfiguration
* GetObjectRetention
* GetObjectTagging
* GetPublicAccessBlock
* HeadBucket
* HeadObject
* ListBucketAnalyticsConfigurations
* ListBucketIntelligentTieringConfigurations
* ListBucketInventoryConfigurations
* ListBucketMetricsConfigurations
* ListBuckets
* ListDirectoryBuckets
* ListMultipartUploads
* ListObjectVersions
* ListObjects
* ListObjectsV2
* ListParts
* PutBucketAccelerateConfiguration
* PutBucketAcl
* PutBucketAnalyticsConfiguration
* PutBucketCors
* PutBucketEncryption
* PutBucketIntelligentTieringConfiguration
* PutBucketInventoryConfiguration
* PutBucketLifecycle
* PutBucketLifecycleConfiguration
* PutBucketLogging
* PutBucketMetricsConfiguration
* PutBucketNotification
* PutBucketNotificationConfiguration
* PutBucketOwnershipControls
* PutBucketPolicy
* PutBucketReplication
* PutBucketRequestPayment
* PutBucketTagging
* PutBucketVersioning
* PutBucketWebsite
* PutObject
* PutObjectAcl
* PutObjectLegalHold
* PutObjectLockConfiguration
* PutObjectRetention
* PutObjectTagging
* PutPublicAccessBlock
* SelectObjectContent
* UpdateBucketMetadataInventoryTableConfiguration
* UpdateBucketMetadataJournalTableConfiguration
* UploadPart
* UploadPartCopy
