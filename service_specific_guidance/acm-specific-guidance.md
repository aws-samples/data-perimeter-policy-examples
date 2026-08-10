# Service-specific guidance: AWS Certificate Manager


This document outlines service-specific guidance for implementing a data perimeter for AWS Certificate Manager.

AWS Certificate Manager (ACM) handles the complexity of creating, storing, and renewing public and private SSL/TLS X.509 certificates and keys that protect your AWS websites and applications. ACM certificates can be deployed to integrated services such as Elastic Load Balancing, Amazon CloudFront distributions, and API Gateway APIs to enable secure connections.

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

* AddTagsToCertificate
* DeleteCertificate
* DescribeCertificate
* GetAccountConfiguration
* GetCertificate
* ImportCertificate
* ListCertificates
* ListTagsForCertificate
* PutAccountConfiguration
* RemoveTagsFromCertificate
* RequestCertificate
* UpdateCertificateOptions
