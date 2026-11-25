# Service-specific guidance: AWS CodeDeploy


This document outlines service-specific guidance for implementing a data perimeter for AWS CodeDeploy.

AWS CodeDeploy is a deployment service that automates application deployments to Amazon EC2 instances, on-premises instances, serverless Lambda functions, or Amazon ECS services. It can deploy application content stored in Amazon S3 buckets, GitHub repositories, or Bitbucket repositories, and scales with your infrastructure from one instance to thousands.

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

* BatchGetApplicationRevisions
* BatchGetApplications
* BatchGetDeploymentGroups
* BatchGetDeploymentInstances
* BatchGetDeploymentTargets
* BatchGetDeployments
* BatchGetOnPremisesInstances
* CreateApplication
* CreateDeployment
* CreateDeploymentConfig
* CreateDeploymentGroup
* DeleteApplication
* DeleteDeploymentConfig
* DeleteDeploymentGroup
* DeregisterOnPremisesInstance
* GetApplication
* GetApplicationRevision
* GetDeployment
* GetDeploymentConfig
* GetDeploymentGroup
* GetDeploymentInstance
* GetDeploymentTarget
* ListApplicationRevisions
* ListApplications
* ListDeploymentConfigs
* ListDeploymentGroups
* ListDeploymentInstances
* ListDeploymentTargets
* ListDeployments
* ListGitHubAccountTokenNames
* ListOnPremisesInstances
* ListTagsForResource
* RegisterApplicationRevision
* StopDeployment
* TagResource
* UntagResource
* UpdateApplication
* UpdateDeploymentGroup
