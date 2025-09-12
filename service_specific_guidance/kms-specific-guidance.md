
# Service-specific guidance: AWS Key Management Service (KMS)


This document outlines service-specific guidance for implementing a data perimeter for AWS Key Management Service (KMS). 

AWS KMS is a managed service that makes it easy for you to create and control the encryption keys used to encrypt your data. KMS integrates with other AWS services to help you protect the data you store in these services and control access to it. It provides a centralized system for managing keys across a wide range of AWS services and in your applications.


The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type | Security objective | Applied on | Policy type | Additional considerations |
|----------------|-------------------|------------|-------------|------------------------|
| Identity perimeter | Only trusted identities can access my resources | Resource | RCP | Y |
| Identity perimeter | Only trusted identities are allowed from my network | Network | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources | Identity | SCP | Y |
| Resource perimeter | Only trusted resources can be accessed from my network | Network | VPC endpoint policy | N |
| Network perimeter | My identities can access resources only from expected networks | Identity | SCP | N |
| Network perimeter | My resources can be accessed only from expected networks | Resource | RCP | N |

*Y – Additional considerations apply. N – No additional considerations apply.
 



**Additional consideration 1**

Perimeter type applicability: identity perimeter applied on resource; resource perimeter applied on identity.
        
CreateGrant allows you to create a grant for another account.

If you want to restrict access so that only trusted identities can view information about your resources, consider implementing these additional controls:

* **Preventative control example 1:** Consider implementing `aws:PrincipalOrgID` in an RCP to restrict service API calls so that your resources can only be accessed by trusted identities. See [identity_perimeter_rcp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/resource_control_policies/identity_perimeter_rcp.json) for an example policy.
* **Preventative control example 2:** Consider restricting [CreateGrant](https://docs.aws.amazon.com/kms/latest/APIReference/API_CreateGrant.html) permissions to administrators and AWS services only using an SCP. See [data_perimeter_governance_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/data_perimeter_governance_scp.json) for an example policy.
* **Detective control example:** Consider using CloudTrail management events to monitor the [CreateGrant](https://docs.aws.amazon.com/kms/latest/APIReference/API_CreateGrant.html) API calls in your environment (specifically, the [GranteePrincipal](https://docs.aws.amazon.com/kms/latest/APIReference/API_CreateGrant.html#KMS-CreateGrant-request-GranteePrincipal) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access so that your identities cannot view resources that were shared with your accounts by untrusted entities, consider implementing this additional control:

* **Detective control example:** Consider using [ListRetirableGrants](https://docs.aws.amazon.com/kms/latest/APIReference/API_ListRetirableGrants.html) to monitor the grants created for your accounts (specifically, the [KeyId](https://docs.aws.amazon.com/kms/latest/APIReference/API_ListRetirableGrants.html#API_ListRetirableGrants_ResponseSyntax) response parameter). If necessary, remediate with the responsive controls of your choice.





**List of service APIs reviewed against data perimeter control objectives**

* CreateAlias
* CreateGrant
* CreateKey
* Decrypt
* DeleteAlias
* DeleteImportedKeyMaterial
* DeriveSharedSecret
* DescribeCustomKeyStores
* DescribeKey
* DisableKey
* DisableKeyRotation
* EnableKey
* EnableKeyRotation
* Encrypt
* GenerateDataKey
* GenerateDataKeyPair
* GenerateDataKeyPairWithoutPlaintext
* GenerateDataKeyWithoutPlaintext
* GenerateMac
* GetKeyPolicy
* GetKeyRotationStatus
* GetParametersForImport
* GetPublicKey
* ImportKeyMaterial
* ListAliases
* ListGrants
* ListKeyPolicies
* ListKeyRotations
* ListKeys
* ListResourceTags
* ListRetirableGrants
* PutKeyPolicy
* ReEncrypt
* ReplicateKey
* RetireGrant
* RotateKeyOnDemand
* Sign
* TagResource
* UntagResource
* UpdateAlias
* UpdateKeyDescription
* UpdatePrimaryRegion
* Verify
* VerifyMac