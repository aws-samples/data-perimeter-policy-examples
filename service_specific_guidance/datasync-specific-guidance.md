# Service-specific guidance: AWS DataSync


This document outlines service-specific guidance for implementing a data perimeter for AWS DataSync.

AWS DataSync is a secure, reliable, high-speed file transfer service that helps you quickly and easily transfer your file or object data to, from, and between AWS storage services. It supports transfers involving on-premises storage systems (NFS, SMB, HDFS, and object storage), AWS storage services such as Amazon S3, Amazon EFS, and Amazon FSx, and storage services in other clouds.

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

* CancelTaskExecution
* CreateLocationEfs
* CreateLocationFsxLustre
* CreateLocationFsxOntap
* CreateLocationFsxOpenZfs
* CreateLocationFsxWindows
* CreateLocationS3
* CreateTask
* DeleteLocation
* DeleteTask
* DescribeLocationEfs
* DescribeLocationFsxLustre
* DescribeLocationFsxOntap
* DescribeLocationFsxOpenZfs
* DescribeLocationFsxWindows
* DescribeLocationS3
* DescribeTask
* DescribeTaskExecution
* ListAgents
* ListLocations
* ListTagsForResource
* ListTaskExecutions
* ListTasks
* StartTaskExecution
* TagResource
* UntagResource
* UpdateLocationEfs
* UpdateLocationFsxLustre
* UpdateLocationFsxOntap
* UpdateLocationFsxOpenZfs
* UpdateLocationFsxWindows
* UpdateLocationS3
* UpdateTask
* UpdateTaskExecution
