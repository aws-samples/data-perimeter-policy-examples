
# Service-specific guidance: Amazon S3 Glacier


This document outlines service-specific guidance for implementing a data perimeter for Amazon S3 Glacier. 

Amazon S3 Glacier is a secure, durable, and low-cost cloud storage service for data archiving and long-term backup. It is designed for infrequently accessed data with flexible retrieval options ranging from minutes to hours. S3 Glacier integrates with Amazon S3 to provide a complete data storage solution for cost-effective data lifecycle management.


The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type | Security objective | Applied on | Policy type | Additional considerations |
|----------------|-------------------|------------|-------------|------------------------|
| Identity perimeter | Only trusted identities can access my resources | Resource | RCP | Y |
| Identity perimeter | Only trusted identities are allowed from my network | Network | VPC endpoint policy | Y |
| Resource perimeter | My identities can access only trusted resources | Identity | SCP | N |
| Resource perimeter | Only trusted resources can be accessed from my network | Network | VPC endpoint policy | Y |
| Network perimeter | My identities can access resources only from expected networks | Identity | SCP | N |
| Network perimeter | My resources can be accessed only from expected networks | Resource | RCP | Y |

*Y – Additional considerations apply. N – No additional considerations apply.
 



**Additional consideration 1**

Perimeter type applicability: identity and network perimeter applied on resource.
        
SetVaultAccessPolicy allows you to apply a resource-based policy to grant access to a vault. The service currently does not support RCPs.


If you want to restrict access to trusted identities and expected networks, consider implementing these additional controls:

* **Preventative control example**: Consider restricting [SetVaultAccessPolicy](https://docs.aws.amazon.com/amazonglacier/latest/dev/api-SetVaultAccessPolicy.html) permissions to administrators only using an SCP. See [restrict_resource_policy_configurations_scp.json](../service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* **Detective control example:** Consider using CloudTrail management events to monitor the [SetVaultAccessPolicy](https://docs.aws.amazon.com/amazonglacier/latest/dev/api-SetVaultAccessPolicy.html) calls in your environment (specifically, the Policy request parameter). If necessary, remediate with the responsive controls of your choice. 


**Additional consideration 2**

Perimeter type applicability: identity and resource perimeter applied on network.
        
The service does not currently support VPC endpoint policies.

If you want to restrict access to your networks to trusted identities and trusted resources, consider implementing these additional controls:

* **Preventative control example 1**: Consider implementing `aws:ResourceOrgID` in an SCP to restrict service API calls so that your identities can only access trusted resources. See [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json) for an example policy.
* **Preventative control example 2**: Consider using your existing security appliances such as outbound proxies to inspect service API calls in your environment for the identities making the calls and resources being accessed, and restrict the calls accordingly. This type of solution might have implications for security, scalability, latency, and reliability that you should evaluate carefully. 






**List of service APIs reviewed against data perimeter control objectives**

* AbortMultipartUpload
* AbortVaultLock
* AddTagsToVault
* CreateVault
* DeleteArchive
* DeleteVault
* DeleteVaultAccessPolicy
* DeleteVaultNotifications
* DescribeJob
* DescribeVault
* GetDataRetrievalPolicy
* GetVaultAccessPolicy
* GetVaultLock
* GetVaultNotifications
* InitiateJob
* InitiateMultipartUpload
* InitiateVaultLock
* ListJobs
* ListMultipartUploads
* ListParts
* ListProvisionedCapacity
* ListTagsForVault
* ListVaults
* RemoveTagsFromVault
* SetDataRetrievalPolicy
* SetVaultAccessPolicy
* SetVaultNotifications
* UploadArchive
* UploadMultipartPart
