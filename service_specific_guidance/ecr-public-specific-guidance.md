# Service-specific guidance: Amazon Elastic Container Registry Public


This document outlines service-specific guidance for implementing a data perimeter for Amazon Elastic Container Registry Public.

Amazon Elastic Container Registry Public (Amazon ECR Public) is a managed AWS container image registry service that is secure, scalable, and reliable, letting developers push and manage Docker images, Open Container Initiative (OCI) images, and OCI compatible artifacts in public repositories whose push access is controlled with IAM resource-based permissions. Images are publicly available to pull, either anonymously or using an Amazon ECR Public authentication token.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | N |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | N |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | N |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | N |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | N |

*Y - Additional considerations apply. N - No additional considerations apply.

## List of service APIs reviewed against data perimeter control objectives

The service APIs covered in this review are based on botocore version [1.43.29](https://github.com/boto/botocore/tree/1.43.29/botocore).

* BatchCheckLayerAvailability
* BatchDeleteImage
* CreateRepository
* DeleteRepository
* DeleteRepositoryPolicy
* DescribeImageTags
* DescribeImages
* DescribeRegistries
* DescribeRepositories
* GetAuthorizationToken
* GetRegistryCatalogData
* GetRepositoryCatalogData
* GetRepositoryPolicy
* InitiateLayerUpload
* ListTagsForResource
* PutRegistryCatalogData
* PutRepositoryCatalogData
* SetRepositoryPolicy
* TagResource
* UntagResource
* UploadLayerPart
