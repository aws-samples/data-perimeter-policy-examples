
# Service-specific guidance: Amazon Simple Storage Service


This document outlines service-specific guidance for implementing a data perimeter for Amazon Simple Storage Service. 
Amazon Simple Storage Service (Amazon S3) is a scalable object storage service that offers industry-leading durability, availability, and performance. It allows you to store and retrieve any amount of data from anywhere on the web, making it ideal for a wide range of use cases such as data lakes, websites, mobile applications, backup and restore, archive, and big data analytics.


The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type | Security objective | Applied on | Policy type | Additional considerations |
|----------------|-------------------|------------|-------------|------------------------|
| Identity perimeter | Only trusted identities can access my resources | Resource | RCP | N |
| Identity perimeter | Only trusted identities are allowed from my network | Network | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources | Identity | SCP | Y |
| Resource perimeter | Only trusted resources can be accessed from my network | Network | VPC endpoint policy | Y |
| Network perimeter | My identities can access resources only from expected networks | Identity | SCP | N |
| Network perimeter | My resources can be accesses only from expected networks | Resource | RCP | N |

*Y – Additional considerations apply. N – No additional considerations apply.
 



**Additional consideration 1**

Perimeter type applicability: resource perimeter.
        
HeadBucket might require access to service-owned buckets, which are buckets that are managed in service accounts.

See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access. 
If you want to restrict access to trusted resources, see [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json) for information about how the default resource perimeter SCP accounts for such an access pattern. Use the service-specific policy, [s3_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/s3_endpoint_policy.json) to enforce the resource perimeter on your network.


**Additional consideration 2**

Perimeter type applicability: resource perimeter.
        
GetObject might require access to service-owned buckets, which are buckets that are managed in service accounts.

See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access. 
If you want to restrict access to trusted resources, see [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json) for information about how the default resource perimeter SCP accounts for such an access pattern. Use the service-specific policy, [s3_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/s3_endpoint_policy.json) to enforce the resource perimeter on your network.


**Additional consideration 3**

Perimeter type applicability: resource perimeter.
        
PutObjectAcl might require access to service-owned buckets, which are buckets that are managed in service accounts.

See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access. 
If you want to restrict access to trusted resources, see [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json) for information about how the default resource perimeter SCP accounts for such an access pattern. Use the service-specific policy, [s3_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/s3_endpoint_policy.json) to enforce the resource perimeter on your network.


**Additional consideration 4**

Perimeter type applicability: resource perimeter applied on identity.
        
PutBucketNotificationConfiguration allows you to specify a resource, such as SNS topic, SQS queue, and Lambda function, that does not belong to your organization as the value for the NotificationConfiguration parameter. Because the subsequent call against the resource is performed by the s3 service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls

* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [NotificationConfiguration](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-s3-bucket-notificationconfig.html) property that does not belong to your organization for the [AWS::S3::Bucket](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-s3-bucket.html) resource.
* **Detective control example:** Consider using CloudTrail management events to monitor the [PutBucketNotificationConfiguration](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketNotificationConfiguration.html) API calls in your environment (specifically, the [NotificationConfiguration](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketNotificationConfiguration.html#API_PutBucketNotificationConfiguration_RequestSyntax) request parameter). If necessary, remediate with the responsive controls of your  choice.


**Additional consideration 5**

Perimeter type applicability: resource perimeter applied on identity.
        
PutBucketInventoryConfiguration allows you to specify an S3 bucket that does not belong to your organization as the value for the Destination parameter. Because the subsequent PutObject call against the S3 bucket is performed by the s3 service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls

* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [Destination](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-s3-bucket-inventoryconfiguration.html#cfn-s3-bucket-inventoryconfiguration-destination) property that does not belong to your organization for the [AWS::S3::Bucket](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-s3-bucket.html) resource.
* **Detective control example:** Consider using CloudTrail management events to monitor the [PutBucketInventoryConfiguration](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketInventoryConfiguration.html) API calls in your environment (specifically, the [Destination](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketInventoryConfiguration.html#AmazonS3-PutBucketInventoryConfiguration-request-Destination) request parameter). If necessary, remediate with the responsive controls of your  choice.


**Additional consideration 6**

Perimeter type applicability: resource perimeter.
        
PutObject might require access to service-owned buckets, which are buckets that are managed in service accounts.


See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access. 
If you want to restrict access to trusted resources, see [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json) for information about how the default resource perimeter SCP accounts for such an access pattern. Use the service-specific policy, [s3_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/s3_endpoint_policy.json)



**Additional consideration 7**

Perimeter type applicability: resource perimeter applied on identity.
        
PutBucketNotification allows you to specify a resource, such as SNS topic, SQS queue, and Lambda function, that does not belong to your organization as the value for the NotificationConfiguration parameter. Because the subsequent call against the resource is performed by the S3 service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls

* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [NotificationConfiguration](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-s3-bucket-notificationconfig.html) property that does not belong to your organization for the [AWS::S3::Bucket](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-s3-bucket.html) resource.
* **Detective control example:** Consider using CloudTrail management events to monitor the [PutBucketNotification](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketNotification.html) API calls in your environment (specifically, the [NotificationConfiguration](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketNotification.html#API_PutBucketNotification_RequestSyntax) request parameter). If necessary, remediate with the responsive controls of your  choice.


**Additional consideration 8**

Perimeter type applicability: resource perimeter applied on identity.
        
PutBucketLogging allows you to specify an S3 bucket that does not belong to your organization as the value for the TargetBucket parameter. Because the subsequent PutObject call against the S3 bucket is performed by the s3 service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls

* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [DestinationBucketName](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-s3-bucket-loggingconfiguration.html#cfn-s3-bucket-loggingconfiguration-destinationbucketname) property that does not belong to your organization for the [AWS::S3::Bucket](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-s3-bucket.html) resource.
* **Detective control example:** Consider using CloudTrail management events to monitor the [PutBucketLogging](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketLogging.html) API calls in your environment (specifically, the [TargetBucket](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketLogging.html#API_PutBucketLogging_RequestSyntax) request parameter). If necessary, remediate with the responsive controls of your  choice.





**List of service APIs reviewed against data perimeter control objectives**


            * CreateBucket
            
            * CreateMultipartUpload
            
            * PutObject
            
            * PutObjectLockConfiguration
            
            * PutObjectLegalHold
            
            * GetObjectLegalHold
            
            * PutObjectRetention
            
            * PutObjectTagging
            
            * DeleteObjectTagging
            
            * UploadPart
            
            * UploadPartCopy
            
            * ListParts
            
            * CompleteMultipartUpload
            
            * CreateSession
            
            * PutBucketAccelerateConfiguration
            
            * PutBucketAcl
            
            * PutBucketAnalyticsConfiguration
            
            * PutBucketCors
            
            * PutBucketEncryption
            
            * PutBucketIntelligentTieringConfiguration
            
            * PutBucketInventoryConfiguration
            
            * PutBucketLifecycleConfiguration
            
            * PutBucketLogging
            
            * PutBucketMetricsConfiguration
            
            * PutBucketNotificationConfiguration
            
            * PutBucketOwnershipControls
            
            * PutBucketPolicy
            
            * PutBucketReplication
            
            * PutBucketRequestPayment
            
            * PutBucketTagging
            
            * PutBucketVersioning
            
            * PutBucketWebsite
            
            * PutObjectAcl
            
            * PutPublicAccessBlock
            
            * CopyObject
            
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
            
            * GetBucketAccelerateConfiguration
            
            * GetBucketAcl
            
            * GetBucketAnalyticsConfiguration
            
            * GetBucketCors
            
            * GetBucketEncryption
            
            * GetBucketIntelligentTieringConfiguration
            
            * GetBucketInventoryConfiguration
            
            * GetBucketLifecycleConfiguration
            
            * GetBucketLocation
            
            * GetBucketLogging
            
            * GetBucketMetricsConfiguration
            
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
            
            * GetObjectLockConfiguration
            
            * GetObjectRetention
            
            * GetObjectTagging
            
            * GetPublicAccessBlock
            
            * SelectObjectContent
            
            * HeadBucket
            
            * HeadObject
            
            * AbortMultipartUpload
            
            * DeleteBucketAnalyticsConfiguration
            
            * DeleteBucketCors
            
            * DeleteBucketEncryption
            
            * DeleteBucketIntelligentTieringConfiguration
            
            * DeleteBucketInventoryConfiguration
            
            * DeleteBucketLifecycle
            
            * DeleteBucketMetricsConfiguration
            
            * DeleteBucketOwnershipControls
            
            * DeleteBucketPolicy
            
            * DeleteBucketReplication
            
            * DeleteBucketTagging
            
            * DeleteBucketWebsite
            
            * DeleteObject
            
            * DeleteObjects
            
            * DeletePublicAccessBlock
            

