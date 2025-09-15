# Service-specific guidance: AWS Cost Optimization Hub


This document outlines service-specific guidance for implementing a data perimeter for AWS Cost Optimization Hub. 


AWS Cost Optimization Hub is a centralized service that helps customers identify and implement cost-saving opportunities across their AWS environments. It provides recommendations, insights, and tools to optimize resource usage, reduce waste, and improve overall cost efficiency. The service analyzes your AWS usage patterns and suggests actionable steps to lower your AWS bill while maintaining performance and reliability.


The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type | Security objective | Applied on | Policy type | Additional considerations |
|----------------|-------------------|------------|-------------|------------------------|
| Identity perimeter | Only trusted identities can access my resources | Resource | RCP | N |
| Identity perimeter | Only trusted identities are allowed from my network | Network | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources | Identity | SCP | N |
| Resource perimeter | Only trusted resources can be accessed from my network | Network | VPC endpoint policy | N |
| Network perimeter | My identities can access resources only from expected networks | Identity | SCP | N |
| Network perimeter | My resources can be accessed only from expected networks | Resource | RCP | N |

*Y – Additional considerations apply. N – No additional considerations apply.
 



**List of service APIs reviewed against data perimeter control objectives**
* GetPreferences
* GetRecommendation
* ListEnrollmentStatuses
* ListRecommendations
* ListRecommendationSummaries
* UpdateEnrollmentStatus
* UpdatePreferences
