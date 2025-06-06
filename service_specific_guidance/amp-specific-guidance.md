
# Service-specific guidance: Amazon Managed Service for Prometheus


This document outlines service-specific guidance for implementing a data perimeter for Amazon Managed Service for Prometheus. Amazon Managed Service for Prometheus is a fully managed monitoring service that makes it easy to monitor containerized applications and infrastructure at scale. It provides a highly available, secure, and managed environment for Prometheus, an open-source monitoring and alerting tool, allowing users to collect, store, and analyze metrics from their applications and infrastructure without the need to manage the underlying infrastructure.


The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type | Security objective | Applied on | Policy type | Additional considerations |
|----------------|-------------------|------------|-------------|------------------------|
| Identity perimeter | Only trusted identities can access my resources | Resource | RCP | N |
| Identity perimeter | Only trusted identities are allowed from my network | Network | VPC Endpoint Policy | Y |
| Resource perimeter | My identities can access only trusted resources | Identity | SCP | N |
| Resource perimeter | Only trusted resources can be accessed from my network | Network | VPC Endpoint Policy | Y |
| Network perimeter | My identities can access resources only from expected networks | Identity | SCP | N |
| Network perimeter | My resources can be accesses only from expected networks | Resource | RCP | N |

*Y – Additional considerations apply. N – No additional considerations apply.
 



**Additional consideration 1**

Perimeter type applicability: identity and resource perimeter applied on network.
        
The service does not currently support VPC endpoint policies.

If you want to restrict access to your networks to trusted identities and trusted resources, consider implementing these additional controls:

* **Preventative control example**: Consider implementing `aws:ResourceOrgID` in an SCP to restrict service API calls so that your identities can only access trusted resources. See [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json) for an example policy.
* **Preventative control example**: Consider using your existing security appliances such as outbound proxies to inspect service API calls in your environment for the identities making the calls and resources being accessed, and restrict the calls accordingly. This type of solution might have implications for security, scalability, latency, and reliability that you should evaluate carefully.






**List of service APIs reviewed against data perimeter control objectives**


            * ListWorkspaces
            
            * ListScrapers
            
            * CreateWorkspace
            
            * CreateRuleGroupsNamespace
            
            * PutRuleGroupsNamespace
            
            * CreateLoggingConfiguration
            
            * CreateScraper
            
            * TagResource
            
            * UpdateLoggingConfiguration
            
            * UpdateWorkspaceAlias
            
            * ListRuleGroupsNamespaces
            
            * ListTagsForResource
            
            * GetDefaultScraperConfiguration
            
            * DescribeLoggingConfiguration
            
            * DescribeRuleGroupsNamespace
            
            * DescribeScraper
            
            * DescribeWorkspace
            
            * UntagResource
            
            * DeleteLoggingConfiguration
            
            * DeleteRuleGroupsNamespace
            
            * DeleteScraper
            
            * DeleteWorkspace
            

