# Service-specific guidance

Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved. SPDX-License-Identifier: CC-BY-SA-4.0

## Description

This folder contains service-specific documents with additional considerations that you might want to review and consider when implementing a data perimeter for a service. Each service-specific document contains a list of service APIs reviewed against data perimeter control objectives to assess whether additional considerations apply to a service within the scope of current analysis.

For each consideration, we provide prescriptive guidance about controls you might want to implement in addition to the [general data perimeter guidance and default policies](../#General-data-perimeter-guidance). 

The following are the types of additional controls that you might want to consider:
* **Preventative controls**: Security controls designed to prevent actions that lead to deviations from your data perimeter baseline. These controls are implemented by using [service control policies (SCPs)](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html) or [resource control policies (RCPs)](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_rcps.html).
* **Proactive controls**: Security controls designed to prevent resource configurations that lead to deviations from your data perimeter baseline. These controls are implemented through automated checks within deployment pipelines, such as those supported with [AWS CloudFormation Hooks](https://docs.aws.amazon.com/cloudformation-cli/latest/hooks-userguide/what-is-cloudformation-hooks.html). Though we primarily use CloudFormation hooks in the prescriptive guidance, you can implement policy-as-code checks by using your preferred infrastructure as code (IaC) tooling.
* **Detective controls**: Security controls designed to detect actions or resource configurations that lead to deviations from your data perimeter baseline. These controls can be implemented by using [AWS CloudTrail](https://aws.amazon.com/cloudtrail/) ([management events](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-events.html#cloudtrail-management-events), [data events](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-events.html#cloudtrail-data-events), [network activity events](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-events.html#cloudtrail-network-events)), [AWS Config](https://aws.amazon.com/config/), and your preferred log analysis tools. If necessary, you can remediate detected deviations with the responsive controls of your choice.

Based on your risk-mitigation strategy, determine which of these control types to apply for additional considerations outlined in each service-specific document. 

When AWS services make calls to other services on your behalf, you might need to review service-specific guidance for all services in use to implement appropriate controls. For example, when using services that store data using your Amazon S3 buckets, consider implementing data perimeter controls for Amazon S3 by consulting S3-specific guidance for comprehensive control coverage.
