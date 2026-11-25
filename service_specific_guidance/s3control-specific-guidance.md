# Service-specific guidance: Amazon Simple Storage Service Control


This document outlines service-specific guidance for implementing a data perimeter for Amazon Simple Storage Service Control.

Amazon Simple Storage Service (Amazon S3) is an object storage service that offers industry-leading scalability, data availability, security, and performance. The S3 Control API (`s3control`) provides access to Amazon S3 control plane actions, such as managing access points, Multi-Region Access Points, S3 Batch Operations jobs, S3 Storage Lens, S3 Access Grants, and account-level Block Public Access settings.

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

* AssociateAccessGrantsIdentityCenter
* CreateAccessGrant
* CreateAccessGrantsInstance
* CreateAccessGrantsLocation
* CreateAccessPoint
* CreateJob
* CreateStorageLensGroup
* DeleteAccessGrant
* DeleteAccessGrantsInstance
* DeleteAccessGrantsInstanceResourcePolicy
* DeleteAccessGrantsLocation
* DeleteAccessPoint
* DeleteAccessPointPolicy
* DeleteJobTagging
* DeleteStorageLensConfiguration
* DeleteStorageLensConfigurationTagging
* DeleteStorageLensGroup
* DescribeJob
* DissociateAccessGrantsIdentityCenter
* GetAccessGrant
* GetAccessGrantsInstance
* GetAccessGrantsInstanceForPrefix
* GetAccessGrantsInstanceResourcePolicy
* GetAccessGrantsLocation
* GetAccessPoint
* GetAccessPointPolicy
* GetAccessPointPolicyStatus
* GetJobTagging
* GetStorageLensConfiguration
* GetStorageLensConfigurationTagging
* GetStorageLensGroup
* ListAccessGrants
* ListAccessGrantsInstances
* ListAccessGrantsLocations
* ListAccessPoints
* ListAccessPointsForDirectoryBuckets
* ListAccessPointsForObjectLambda
* ListCallerAccessGrants
* ListJobs
* ListStorageLensConfigurations
* ListStorageLensGroups
* ListTagsForResource
* PutAccessGrantsInstanceResourcePolicy
* PutAccessPointPolicy
* PutJobTagging
* PutStorageLensConfiguration
* PutStorageLensConfigurationTagging
* TagResource
* UntagResource
* UpdateAccessGrantsLocation
* UpdateStorageLensGroup
