# Service-specific guidance: Amazon Elastic Container Registry


This document outlines service-specific guidance for implementing a data perimeter for Amazon Elastic Container Registry.

Amazon Elastic Container Registry (Amazon ECR) is an AWS managed container image registry service that is secure, scalable, and reliable. It supports private repositories with resource-based permissions using AWS IAM so that specified users or Amazon EC2 instances can access container repositories and images.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | N |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | Y |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | Y |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | N |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | Y |

*Y - Additional considerations apply. N - No additional considerations apply.

## BatchGetImage
### Access to service-owned resources

**Perimeter type applicability**: resource perimeter.

**Description**: [BatchGetImage](https://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_BatchGetImage.html) might require access to service-owned repositories, which are repositories that are managed in service accounts.

**Additional controls**:

If you want to restrict access to trusted resources for your identities, consider implementing this control:
* Preventative control example: Consider implementing the default resource perimeter SCP, [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json), which lists service-owned repositories in the `NotResource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access.

If you want to restrict access to your networks to trusted resources, consider implementing this control:
* Preventative control example: Consider implementing the service-specific VPC endpoint policy, [ecr.api_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/ecr.api_endpoint_policy.json), which lists service-owned repositories in the `Resource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that might be accessed from your networks.

## GetDownloadUrlForLayer
### S3 presigned URLs created by service principals

**Perimeter type applicability**: network perimeter on resource.

**Description**: [GetDownloadUrlForLayer](https://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_GetDownloadUrlForLayer.html) returns an Amazon S3 presigned URL that users can use to download Amazon ECR private image layers from service-owned buckets. The presigned URL is created with the Amazon ECR service account identity.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [GetDownloadUrlForLayer](https://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_GetDownloadUrlForLayer.html) permissions to select principals using an SCP. See [restrict_presignedURL_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_presignedURL_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [GetDownloadUrlForLayer](https://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_GetDownloadUrlForLayer.html) API calls in your environment. If necessary, remediate with the responsive controls of your choice.

### Access to service-owned resources

**Perimeter type applicability**: resource perimeter.

**Description**: [GetDownloadUrlForLayer](https://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_GetDownloadUrlForLayer.html) might require access to service-owned repositories, which are repositories that are managed in service accounts.

**Additional controls**:

If you want to restrict access to trusted resources for your identities, consider implementing this control:
* Preventative control example: Consider implementing the default resource perimeter SCP, [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json), which lists service-owned repositories in the `NotResource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access.

If you want to restrict access to your networks to trusted resources, consider implementing this control:
* Preventative control example: Consider implementing the service-specific VPC endpoint policy, [ecr.api_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/ecr.api_endpoint_policy.json), which lists service-owned repositories in the `Resource` element. See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that might be accessed from your networks.

## PutReplicationConfiguration
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [PutReplicationConfiguration](https://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_PutReplicationConfiguration.html) allows you to specify a private registry that does not belong to your organization as the value for the `destinations` request parameter. Because the subsequent call against the private registry is performed by the service-linked role (SLR), it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [ReplicationConfiguration](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-ecr-replicationconfiguration.html#cfn-ecr-replicationconfiguration-replicationconfiguration) property that does not belong to your organization for the [AWS::ECR::ReplicationConfiguration](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-ecr-replicationconfiguration.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [PutReplicationConfiguration](https://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_PutReplicationConfiguration.html) API calls in your environment (specifically, the [destinations](https://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_ReplicationRule.html#ECR-Type-ReplicationRule-destinations) request parameter). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

* BatchCheckLayerAvailability
* BatchDeleteImage
* BatchGetImage
* BatchGetRepositoryScanningConfiguration
* CreatePullThroughCacheRule
* CreateRepository
* CreateRepositoryCreationTemplate
* DeleteLifecyclePolicy
* DeletePullThroughCacheRule
* DeleteRegistryPolicy
* DeleteRepository
* DeleteRepositoryPolicy
* DescribeImageReplicationStatus
* DescribeImageScanFindings
* DescribeImages
* DescribePullThroughCacheRules
* DescribeRegistry
* DescribeRepositories
* DescribeRepositoryCreationTemplates
* GetAccountSetting
* GetAuthorizationToken
* GetDownloadUrlForLayer
* GetLifecyclePolicy
* GetLifecyclePolicyPreview
* GetRegistryPolicy
* GetRegistryScanningConfiguration
* GetRepositoryPolicy
* InitiateLayerUpload
* ListImages
* ListTagsForResource
* PutAccountSetting
* PutImageScanningConfiguration
* PutImageTagMutability
* PutLifecyclePolicy
* PutRegistryPolicy
* PutRegistryScanningConfiguration
* PutReplicationConfiguration
* SetRepositoryPolicy
* StartImageScan
* StartLifecyclePolicyPreview
* TagResource
* UntagResource
* UpdateRepositoryCreationTemplate
* UploadLayerPart
* ValidatePullThroughCacheRule
