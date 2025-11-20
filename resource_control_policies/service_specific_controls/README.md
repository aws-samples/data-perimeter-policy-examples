# Examples of service-specific controls

Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved. SPDX-License-Identifier: CC-BY-SA-4.0

## Description

This folder contains examples of resource-based policies that enforce identity and network perimeter controls for services that are currently not supported by resource control policies (RCPs):

* [api_gateway_policy](api_gateway_policy.json) - Enforces identity and network perimeter controls on [Amazon API Gateway](https://aws.amazon.com/api-gateway/) resources.
* [sns_topic_policy](sns_topic_policy.json) - Enforces identity and network perimeter controls on [Amazon Simple Notification Service (Amazon SNS)](https://aws.amazon.com/sns/) resources.

Because developers will be creating resources such as Amazon SNS topics on a regular basis, you might need to implement automation to enforce identity and network perimeter controls when those resources are created or their policies are changed. One option is to use custom [AWS Config rules](https://docs.aws.amazon.com/config/latest/developerguide/evaluate-config_develop-rules.html)_._ Alternatively, you can choose to enforce resource deployment through [AWS Service Catalog](https://aws.amazon.com/servicecatalog/?aws-service-catalog.sort-by=item.additionalFields.createdDate&aws-service-catalog.sort-order=desc) or a CI/CD pipeline. With the AWS Service Catalog approach, you can have identity and network perimeter controls built into the centrally controlled products that are made available to developers to deploy within their accounts. With the CI/CD pipeline approach, the pipeline can have built-in compliance checks that enforce identity and network perimeter controls during the deployment. If you are deploying resources with your CI/CD pipeline by using [AWS CloudFormation](https://aws.amazon.com/cloudformation/), see the blog post [Proactively keep resources secure and compliant with AWS CloudFormation Hooks](https://aws.amazon.com/blogs/mt/proactively-keep-resources-secure-and-compliant-with-aws-cloudformation-hooks/).

Note that the policy examples in this folder do not represent a complete list of valid data access patterns, and they are intended for you to tailor and extend them to suit the needs of your environment. Additionally, these examples do not grant any permissions; they only restrict access by explicitly denying specific data access patterns. You still have to grant appropriate permissions with explicit `Allow` statements in identity-based or resource-based policies.  