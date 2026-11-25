# Service-specific guidance: Amazon FSx


This document outlines service-specific guidance for implementing a data perimeter for Amazon FSx.

Amazon FSx makes it easy and cost effective to launch, run, and scale feature-rich, high-performance file systems in the cloud. As a fully managed service, it lets you choose between four widely-used file systems (NetApp ONTAP, OpenZFS, Windows File Server, and Lustre) while it handles hardware provisioning, patching, and backups.

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

* AssociateFileSystemAliases
* CopyBackup
* CreateAndAttachS3AccessPoint
* CreateBackup
* CreateDataRepositoryAssociation
* CreateDataRepositoryTask
* CreateFileCache
* CreateFileSystem
* CreateFileSystemFromBackup
* CreateSnapshot
* CreateStorageVirtualMachine
* CreateVolume
* CreateVolumeFromBackup
* DeleteBackup
* DeleteDataRepositoryAssociation
* DeleteFileCache
* DeleteFileSystem
* DeleteSnapshot
* DeleteStorageVirtualMachine
* DeleteVolume
* DescribeBackups
* DescribeDataRepositoryAssociations
* DescribeDataRepositoryTasks
* DescribeFileCaches
* DescribeFileSystemAliases
* DescribeFileSystems
* DescribeS3AccessPointAttachments
* DescribeSharedVpcConfiguration
* DescribeSnapshots
* DescribeStorageVirtualMachines
* DescribeVolumes
* DetachAndDeleteS3AccessPoint
* DisassociateFileSystemAliases
* ListTagsForResource
* ReleaseFileSystemNfsV3Locks
* TagResource
* UntagResource
* UpdateDataRepositoryAssociation
* UpdateFileCache
* UpdateFileSystem
* UpdateSharedVpcConfiguration
* UpdateSnapshot
* UpdateStorageVirtualMachine
* UpdateVolume
