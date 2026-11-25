# Service-specific guidance: AWS Private Certificate Authority


This document outlines service-specific guidance for implementing a data perimeter for AWS Private Certificate Authority.

AWS Private Certificate Authority enables creation of private certificate authority (CA) hierarchies, including root and subordinate CAs, without the investment and maintenance costs of operating an on-premises CA. Its private CAs can issue end-entity X.509 certificates for use cases such as encrypting TLS communication channels and authenticating users, computers, API endpoints, and IoT devices.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | N |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | Y |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | N |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | N |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | N |

*Y - Additional considerations apply. N - No additional considerations apply.

## CreateCertificateAuthority
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [CreateCertificateAuthority](https://docs.aws.amazon.com/privateca/latest/APIReference/API_CreateCertificateAuthority.html) allows you to specify an Amazon S3 bucket that does not belong to your organization as the value for the `RevocationConfiguration.CrlConfiguration.S3BucketName` request parameter. Because the subsequent call against the Amazon S3 bucket is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [RevocationConfiguration.CrlConfiguration.S3BucketName](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-acmpca-certificateauthority-crlconfiguration.html#cfn-acmpca-certificateauthority-crlconfiguration-s3bucketname) property that does not belong to your organization for the [AWS::ACMPCA::CertificateAuthority](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-acmpca-certificateauthority.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [CreateCertificateAuthority](https://docs.aws.amazon.com/privateca/latest/APIReference/API_CreateCertificateAuthority.html) API calls in your environment (specifically, the [RevocationConfiguration.CrlConfiguration.S3BucketName](https://docs.aws.amazon.com/privateca/latest/APIReference/API_CrlConfiguration.html#privateca-Type-CrlConfiguration-S3BucketName) request parameter). If necessary, remediate with the responsive controls of your choice.

## CreateCertificateAuthorityAuditReport
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [CreateCertificateAuthorityAuditReport](https://docs.aws.amazon.com/privateca/latest/APIReference/API_CreateCertificateAuthorityAuditReport.html) allows you to specify an S3 bucket that does not belong to your organization as the value for the `S3BucketName` request parameter. Because the subsequent call against the S3 bucket is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing this additional control:
* Detective control example: Consider using CloudTrail management events to monitor the [CreateCertificateAuthorityAuditReport](https://docs.aws.amazon.com/privateca/latest/APIReference/API_CreateCertificateAuthorityAuditReport.html) API calls in your environment (specifically, the [S3BucketName](https://docs.aws.amazon.com/privateca/latest/APIReference/API_CreateCertificateAuthorityAuditReport.html#privateca-CreateCertificateAuthorityAuditReport-request-S3BucketName) request parameter). If necessary, remediate with the responsive controls of your choice.

## UpdateCertificateAuthority
### Configuration of an external resource

**Perimeter type applicability**: resource perimeter applied on identity.

**Description**: [UpdateCertificateAuthority](https://docs.aws.amazon.com/privateca/latest/APIReference/API_UpdateCertificateAuthority.html) allows you to specify an Amazon S3 bucket that does not belong to your organization as the value for the `RevocationConfiguration.CrlConfiguration.S3BucketName` request parameter. Because the subsequent call against the Amazon S3 bucket is performed by the service principal, it is not restricted with `aws:ResourceOrgID` implemented in an SCP.

**Additional controls**:

If you want to restrict access to trusted resources, consider implementing these additional controls:
* Proactive control example: Consider implementing CloudFormation Hooks to help prevent developers from specifying the [RevocationConfiguration.CrlConfiguration.S3BucketName](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-acmpca-certificateauthority-crlconfiguration.html#cfn-acmpca-certificateauthority-crlconfiguration-s3bucketname) property that does not belong to your organization for the [AWS::ACMPCA::CertificateAuthority](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-acmpca-certificateauthority.html) resource.
* Detective control example: Consider using CloudTrail management events to monitor the [UpdateCertificateAuthority](https://docs.aws.amazon.com/privateca/latest/APIReference/API_UpdateCertificateAuthority.html) API calls in your environment (specifically, the [RevocationConfiguration.CrlConfiguration.S3BucketName](https://docs.aws.amazon.com/privateca/latest/APIReference/API_CrlConfiguration.html#privateca-Type-CrlConfiguration-S3BucketName) request parameter). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

The service APIs covered in this review are based on botocore version [1.43.29](https://github.com/boto/botocore/tree/1.43.29/botocore).

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
