# Service-specific guidance: Amazon CloudFront KeyValueStore


This document outlines service-specific guidance for implementing a data perimeter for Amazon CloudFront KeyValueStore.

CloudFront KeyValueStore is a secure, global, low-latency key value datastore that allows read access from within CloudFront Functions, enabling advanced customizable logic at the CloudFront edge locations.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | N |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | Y |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | N |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | Y |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | N |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | N |

*Y - Additional considerations apply. N - No additional considerations apply.

## All API operations
### No VPC endpoint policy support

**Perimeter type applicability**: identity and resource perimeter applied on network.

**Description**: The service does not currently support VPC endpoint policies.

**Additional controls**:

If you want to restrict access to your networks to trusted identities, consider implementing this additional control:
* Preventative control example: Consider using your existing security appliances such as outbound proxies to inspect service API calls in your environment for the identities making the calls, and restrict the calls accordingly. This type of solution might have implications for security, scalability, latency, and reliability that you should evaluate carefully.

If you want to restrict access to your networks to trusted resources, consider implementing these additional controls:
* Preventative control example: Consider implementing `aws:ResourceOrgID` in an SCP to restrict service API calls so that your identities can only access trusted resources. See [resource_perimeter_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/resource_perimeter_scp.json) for an example policy.
* Preventative control example: Consider using your existing security appliances such as outbound proxies to inspect service API calls in your environment for the resources being accessed, and restrict the calls accordingly. This type of solution might have implications for security, scalability, latency, and reliability that you should evaluate carefully.

## List of service APIs reviewed against data perimeter control objectives

* DeleteKey
* DescribeKeyValueStore
* GetKey
* ListKeys
* PutKey
* UpdateKeys
