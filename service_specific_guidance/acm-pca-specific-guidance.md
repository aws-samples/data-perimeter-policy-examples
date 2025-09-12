
# Service-specific guidance: AWS Private Certificate Authority


This document outlines service-specific guidance for implementing a data perimeter for AWS Private Certificate Authority (AWS Private CA). 

AWS Private CA is a managed service that allows you to create and manage private certificate authorities (CAs) within your organization. It enables you to issue and manage private certificates for your internal applications, services, and devices, providing secure communication and authentication within your AWS environment and on-premises infrastructure.


The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type | Security objective | Applied on | Policy type | Additional considerations |
|----------------|-------------------|------------|-------------|------------------------|
| Identity perimeter | Only trusted identities can access my resources | Resource | RCP | N |
| Identity perimeter | Only trusted identities are allowed from my network | Network | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources | Identity | SCP | Y |
| Resource perimeter | Only trusted resources can be accessed from my network | Network | VPC endpoint policy | N |
| Network perimeter | My identities can access resources only from expected networks | Identity | SCP | N |
| Network perimeter | My resources can be accessed only from expected networks | Resource | RCP | N |

*Y – Additional considerations apply. N – No additional considerations apply.
 


**Additional consideration 1**

Perimeter type applicability: resource perimeter applied on identity.

CreateCertificateAuthorityAuditReport allows you to specify an S3 bucket that does not belong to your organization as the value for the S3BucketName request parameter. Because the subsequent PutObject call against the S3 bucket is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

If you want to restrict access to trusted resources, consider implementing these additional controls:

* **Detective control example:** Consider using CloudTrail management events to monitor the [CreateCertificateAuthorityAuditReport](https://docs.aws.amazon.com/privateca/latest/APIReference/API_CreateCertificateAuthorityAuditReport.html) API calls in your environment (specifically, the [S3BucketName](https://docs.aws.amazon.com/privateca/latest/APIReference/API_CreateCertificateAuthorityAuditReport.html#privateca-CreateCertificateAuthorityAuditReport-request-S3BucketName) request parameter). If necessary, remediate with the responsive controls of your  choice.





**List of service APIs reviewed against data perimeter control objectives**

* CreateCertificateAuthority
* CreateCertificateAuthorityAuditReport
* CreatePermission
* DeleteCertificateAuthority
* DeletePermission
* DeletePolicy
* DescribeCertificateAuthority
* DescribeCertificateAuthorityAuditReport
* GetCertificate
* GetCertificateAuthorityCertificate
* GetCertificateAuthorityCsr
* GetPolicy
* IssueCertificate
* ListCertificateAuthorities
* ListPermissions
* ListTags
* PutPolicy
* RestoreCertificateAuthority
* TagCertificateAuthority
* UntagCertificateAuthority
* UpdateCertificateAuthority
