# Service-specific guidance: Amazon Elastic Container Registry


This document outlines service-specific guidance for implementing a data perimeter for Amazon Elastic Container Registry. 


Amazon Elastic Container Registry (ECR) is a fully managed container registry service that makes it easy to store, manage, and deploy container images. It integrates seamlessly with Amazon ECS, Amazon EKS, and other container orchestration platforms, allowing developers to securely store and version their container images. ECR provides encrypted storage, access controls, and scalability to support containerized application development and deployment workflows.


The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type | Security objective | Applied on | Policy type | Additional considerations |
|----------------|-------------------|------------|-------------|------------------------|
| Identity perimeter | Only trusted identities can access my resources | Resource | RCP | N |
| Identity perimeter | Only trusted identities are allowed from my network | Network | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources | Identity | SCP | Y |
| Resource perimeter | Only trusted resources can be accessed from my network | Network | VPC endpoint policy | Y |
| Network perimeter | My identities can access resources only from expected networks | Identity | SCP | Y |
| Network perimeter | My resources can be accessed only from expected networks | Resource | RCP | Y |

*Y – Additional considerations apply. N – No additional considerations apply.
 


**Additional consideration 1**

Perimeter type applicability: resource perimeter.
        
GetDownloadUrlForLayer might require access to service-owned repositories, which are repositories that are managed in service accounts.

See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access. 

If you want to restrict access to trusted resources, see [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json) for information about how the default resource perimeter SCP accounts for such an access pattern. Use the service-specific policy, [ecr.api_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/ecr.api_endpoint_policy.json) to enforce the resource perimeter on your network.


**Additional consideration 2**

Perimeter type applicability: resource perimeter.
        
BatchGetImage might require access to service-owned repositories, which are repositories that are managed in service accounts.

See [Service-owned resources](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_owned_resources.md) for a list of resources that AWS services own and that your identities might access. 

If you want to restrict access to trusted resources, see [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json) for information about how the default resource perimeter SCP accounts for such an access pattern. Use the service-specific policy, [ecr.api_endpoint_policy.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/vpc_endpoint_policies/ecr.api_endpoint_policy.json) to enforce the resource perimeter on your network.


**Additional consideration 3**

Perimeter type applicability: network perimeter.
        
GetDownloadUrlForLayer returns an Amazon S3 presigned URL that users can use to download Amazon Elastic Container Registry (Amazon ECR) private image layers from service-owned buckets. The presigned URL is created with an Amazon ECR service account identity.

If you want to restrict access to expected networks, consider implementing these additional controls:

* **Preventative control example:** Consider restricting [GetDownloadUrlForLayer](https://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_GetDownloadUrlForLayer.html) permissions to administrators or specific applications only using an SCP. See [restrict_presignedUrl_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_presignedURL_scp.json) for an example policy.
* **Detective control example:** Consider using CloudTrail management events to monitor the [GetDownloadUrlForLayer](https://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_GetDownloadUrlForLayer.html) API calls in your environment. If necessary, remediate with the responsive controls of your choice.


**Additional consideration 4**

Perimeter type applicability: resource perimeter applied on identity.
        
PutReplicationConfiguration allows you to specify a private registry that does not belong to your organization as the value for the destinations request parameter. Because the subsequent ReplicateImage call against the registry is performed by the service-linked role (SLR), it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls:

* **Proactive control example:** Consider implementing CloudFormation Hooks to help prevent developers from specifying the [ReplicationConfiguration](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-ecr-replicationconfiguration.html#cfn-ecr-replicationconfiguration-replicationconfiguration) property that does not belong to your organization for the [AWS::ECR::ReplicationConfiguration](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-ecr-replicationconfiguration.html) resource.
* **Detective control example:** Consider using CloudTrail management events to monitor the [PutReplicationConfiguration](https://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_PutReplicationConfiguration.html) API calls in your environment (specifically, the [destinations](https://docs.aws.amazon.com/AmazonECR/latest/APIReference/API_ReplicationRule.html#ECR-Type-ReplicationRule-destinations) request parameter). If necessary, remediate with the responsive controls of your choice.


**List of service APIs reviewed against data perimeter control objectives**
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
* DeleteRepositoryCreationTemplate
* DeleteRepositoryPolicy
* DescribeImageReplicationStatus
* DescribeImages
* DescribeImageScanFindings
* DescribePullThroughCacheRules
* DescribeRegistry
* DescribeRepositories
* DescribeRepositoryCreationTemplates
* GetAccountSetting
* GetAuthorizationToken
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
