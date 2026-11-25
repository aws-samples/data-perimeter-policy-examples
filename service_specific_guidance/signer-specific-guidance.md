# Service-specific guidance: AWS Signer


This document outlines service-specific guidance for implementing a data perimeter for AWS Signer.

AWS Signer is a fully managed code-signing service that helps ensure the trust and integrity of your code, such as AWS Lambda deployment packages, container images, and IoT device firmware. It gives security administrators a single place to define the signing environment, including which IAM roles can sign code and in which Regions, and enables central management of the code-signing lifecycle.

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

* AddProfilePermission
* CancelSigningProfile
* DescribeSigningJob
* GetSigningPlatform
* GetSigningProfile
* ListProfilePermissions
* ListSigningJobs
* ListSigningPlatforms
* ListSigningProfiles
* ListTagsForResource
* PutSigningProfile
* RemoveProfilePermission
* RevokeSignature
* RevokeSigningProfile
* SignPayload
* StartSigningJob
* TagResource
* UntagResource
