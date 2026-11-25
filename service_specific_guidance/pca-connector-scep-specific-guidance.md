# Service-specific guidance: AWS Private CA Connector for SCEP


This document outlines service-specific guidance for implementing a data perimeter for AWS Private CA Connector for SCEP.

AWS Private CA Connector for SCEP links AWS Private Certificate Authority to your SCEP-enabled mobile devices and networking equipment, so you can use AWS Private CA to issue certificates and enroll devices using the Simple Certificate Enrollment Protocol (SCEP). It works with popular mobile device management (MDM) systems, such as Microsoft Intune and Jamf Pro, and with other clients or endpoints that support SCEP.

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

* CreateChallenge
* CreateConnector
* DeleteChallenge
* DeleteConnector
* GetChallengeMetadata
* GetChallengePassword
* GetConnector
* ListChallengeMetadata
* ListConnectors
* ListTagsForResource
* TagResource
* UntagResource
