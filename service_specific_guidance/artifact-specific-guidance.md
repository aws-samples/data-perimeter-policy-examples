
# Service-specific guidance: AWS Artifact


This document outlines service-specific guidance for implementing a data perimeter for AWS Artifact. 

AWS Artifact is a self-service portal that provides on-demand access to AWS' compliance reports and agreements. It allows customers to download AWS security and compliance documents, such as ISO certifications, PCI reports, and SOC reports, to support their regulatory and compliance requirements. AWS Artifact helps organizations demonstrate AWS infrastructure compliance to auditors and regulators.


The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type | Security objective | Applied on | Policy type | Additional considerations |
|----------------|-------------------|------------|-------------|------------------------|
| Identity perimeter | Only trusted identities can access my resources | Resource | RCP | N |
| Identity perimeter | Only trusted identities are allowed from my network | Network | VPC endpoint policy | Y |
| Resource perimeter | My identities can access only trusted resources | Identity | SCP | N |
| Resource perimeter | Only trusted resources can be accessed from my network | Network | VPC endpoint policy | Y |
| Network perimeter | My identities can access resources only from expected networks | Identity | SCP | N |
| Network perimeter | My resources can be accessed only from expected networks | Resource | RCP | N |

*Y – Additional considerations apply. N – No additional considerations apply.
 



**Additional consideration 1**

Perimeter type applicability: identity and resource perimeter applied on network.
        
The service does not currently support VPC endpoint policies.

If you want to restrict access to your networks to trusted identities and trusted resources, consider implementing these additional controls:

* **Preventative control example 1**: Consider implementing `aws:ResourceOrgID` in an SCP to restrict service calls so that your identities can only access trusted resources. See [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json) for an example policy.
* **Preventative control example 2**: Consider using your existing security appliances such as outbound proxies to inspect service API calls in your environment for the identities making the calls and resources being accessed, and restrict the calls accordingly. This type of solution might have implications for security, scalability, latency, and reliability that you should evaluate carefully.






**List of service APIs reviewed against data perimeter control objectives**

* GetAccountSettings
* GetReport
* GetReportMetadata
* GetTermForReport
* ListReports
* PutAccountSettings
