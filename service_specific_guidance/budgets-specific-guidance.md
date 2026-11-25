# Service-specific guidance: AWS Budgets


This document outlines service-specific guidance for implementing a data perimeter for AWS Budgets.

AWS Budgets lets you track and take action on your AWS costs and usage. It alerts you by email or Amazon SNS when actual or forecasted costs or usage exceed your budgeted amount, can run budget actions in response, and also monitors utilization and coverage for your Reserved Instances (RIs) or Savings Plans.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | N |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | N |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | N |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | N |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | N |

*Y - Additional considerations apply. N - No additional considerations apply.

## List of service APIs reviewed against data perimeter control objectives

The service APIs covered in this review are based on botocore version [1.43.29](https://github.com/boto/botocore/tree/1.43.29/botocore).

* CreateBudget
* CreateBudgetAction
* CreateNotification
* CreateSubscriber
* DeleteBudget
* DeleteBudgetAction
* DeleteNotification
* DeleteSubscriber
* DescribeBudget
* DescribeBudgetAction
* DescribeBudgetActionHistories
* DescribeBudgetActionsForAccount
* DescribeBudgetActionsForBudget
* DescribeBudgetNotificationsForAccount
* DescribeBudgetPerformanceHistory
* DescribeBudgets
* DescribeNotificationsForBudget
* DescribeSubscribersForNotification
* ListTagsForResource
* TagResource
* UntagResource
* UpdateBudget
* UpdateBudgetAction
* UpdateNotification
* UpdateSubscriber
