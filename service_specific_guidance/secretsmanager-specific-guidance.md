
# Service-specific guidance: AWS Secrets Manager


This document outlines service-specific guidance for implementing a data perimeter for AWS Secrets Manager. 

AWS Secrets Manager is a service that helps you protect access to your applications, services, and IT resources. It enables you to easily rotate, manage, and retrieve database credentials, API keys, and other secrets throughout their lifecycle. Secrets Manager allows you to replace hardcoded credentials in your code with an API call to Secrets Manager to retrieve the secret programmatically.


The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type | Security objective | Applied on | Policy type | Additional considerations |
|----------------|-------------------|------------|-------------|------------------------|
| Identity perimeter | Only trusted identities can access my resources | Resource | RCP | N |
| Identity perimeter | Only trusted identities are allowed from my network | Network | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources | Identity | SCP | N |
| Resource perimeter | Only trusted resources can be accessed from my network | Network | VPC endpoint policy | N |
| Network perimeter | My identities can access resources only from expected networks | Identity | SCP | N |
| Network perimeter | My resources can be accessed only from expected networks | Resource | RCP | N |

*Y – Additional considerations apply. N – No additional considerations apply.
 


**List of service APIs reviewed against data perimeter control objectives**

* BatchGetSecretValue
* CancelRotateSecret
* CreateSecret
* DeleteResourcePolicy
* DeleteSecret
* DescribeSecret
* GetRandomPassword
* GetResourcePolicy
* GetSecretValue
* ListSecretVersionIds
* ListSecrets
* PutResourcePolicy
* PutSecretValue
* RemoveRegionsFromReplication
* ReplicateSecretToRegions
* RestoreSecret
* RotateSecret
* TagResource
* UntagResource
* UpdateSecret
* ValidateResourcePolicy
