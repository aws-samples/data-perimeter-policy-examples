# Service-specific guidance: Amazon Managed Streaming for Kafka Connect


This document outlines service-specific guidance for implementing a data perimeter for Amazon Managed Streaming for Kafka Connect.

MSK Connect is a feature of Amazon Managed Streaming for Apache Kafka (Amazon MSK) that makes it easy for developers to stream data to and from Apache Kafka clusters by deploying fully managed Kafka Connect connectors that move data between Kafka topics and external systems such as databases, search indexes, file systems, Amazon S3, and Amazon OpenSearch Service. It continuously monitors connector health and delivery state, patches and manages the underlying hardware, and autoscales connectors to match changes in throughput.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | N |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | N |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | N |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | Y |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | N |

*Y - Additional considerations apply. N - No additional considerations apply.

## CreateConnector
### Service role calls from expected networks

**Perimeter type applicability**: network perimeter on identity.

**Description**: [CreateConnector](https://docs.aws.amazon.com/MSKC/latest/mskc/API_CreateConnector.html) allows you to specify a service role for your MSK Connect connector operations. The role's credentials are provided to the compute environment, which uses them to call AWS APIs on your behalf.

**Additional controls**:

If you want to restrict access to expected networks, consider implementing this control:
* Preventative control example: Consider using the default network perimeter SCP, [network_perimeter_sourcevpc_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_sourcevpc_scp.json) or [network_perimeter_vpceorgid_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/network_perimeter_vpceorgid_scp.json). These policies demonstrate how to use the `dp:include:network` tag set to `true` to restrict calls made by the connector's service execution role. If the service role assumes another role as part of its operations, consider restricting access to expected networks for the assumed role by following the [network perimeter guidance](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/README.md#sidenforcenetworkperimetervpceorgid).

## List of service APIs reviewed against data perimeter control objectives

The service APIs covered in this review are based on botocore version [1.43.29](https://github.com/boto/botocore/tree/1.43.29/botocore).

* CreateConnector
* CreateCustomPlugin
* CreateWorkerConfiguration
* DeleteConnector
* DeleteCustomPlugin
* DeleteWorkerConfiguration
* DescribeConnector
* DescribeCustomPlugin
* DescribeWorkerConfiguration
* ListConnectorOperations
* ListConnectors
* ListCustomPlugins
* ListTagsForResource
* ListWorkerConfigurations
* TagResource
* UntagResource
