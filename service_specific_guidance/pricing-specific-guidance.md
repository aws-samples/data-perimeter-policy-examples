# Service-specific guidance: AWS Price List


This document outlines service-specific guidance for implementing a data perimeter for AWS Price List.

AWS Price List provides a catalog of the products and prices for AWS services that you can purchase on AWS. It offers the Price List Query API to programmatically query services, products, and SKU-level pricing information, and the Price List Bulk API to fetch up-to-date price list files in JSON and CSV formats organized by AWS service and AWS Region.

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

* DescribeServices
* GetAttributeValues
* GetPriceListFileUrl
* GetProducts
* ListPriceLists
