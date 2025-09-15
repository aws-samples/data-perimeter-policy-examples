# Service-specific guidance: AWS Security Token Service


This document outlines service-specific guidance for implementing a data perimeter for AWS Security Token Service (AWS STS). 


AWS STS is a web service that enables you to request temporary, limited-privilege credentials for users. These temporary security credentials can be used to access AWS services and resources securely. STS is useful for scenarios that require temporary access, such as identity federation, cross-account access, and applications running on EC2 instances that need to access AWS resources.


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
* AssumeRole
* GetAccessKeyInfo
* GetCallerIdentity
