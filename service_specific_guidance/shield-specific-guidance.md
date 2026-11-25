# Service-specific guidance: AWS Shield


This document outlines service-specific guidance for implementing a data perimeter for AWS Shield.

AWS Shield is a managed threat protection service that protects the perimeter of your application, providing protections against Distributed Denial of Service (DDoS) attacks for AWS resources at the network and transport layers (layer 3 and 4) and the application layer (layer 7). Shield Standard is provided automatically and at no extra charge, and customers can subscribe to Shield Advanced for expanded protection that includes automatic application layer DDoS mitigation, advanced event visibility, and dedicated support from the Shield Response Team (SRT).

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

* AssociateDRTRole
* AssociateHealthCheck
* CreateProtection
* CreateProtectionGroup
* CreateSubscription
* DeleteProtection
* DeleteProtectionGroup
* DescribeAttack
* DescribeAttackStatistics
* DescribeDRTAccess
* DescribeEmergencyContactSettings
* DescribeProtection
* DescribeProtectionGroup
* DescribeSubscription
* DisableApplicationLayerAutomaticResponse
* DisableProactiveEngagement
* DisassociateDRTRole
* DisassociateHealthCheck
* EnableApplicationLayerAutomaticResponse
* EnableProactiveEngagement
* GetSubscriptionState
* ListAttacks
* ListProtectionGroups
* ListProtections
* ListResourcesInProtectionGroup
* ListTagsForResource
* TagResource
* UntagResource
* UpdateApplicationLayerAutomaticResponse
* UpdateEmergencyContactSettings
* UpdateProtectionGroup
* UpdateSubscription
