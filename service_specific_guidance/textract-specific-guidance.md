# Service-specific guidance: Amazon Textract


This document outlines service-specific guidance for implementing a data perimeter for Amazon Textract.

Amazon Textract helps you add document text detection and analysis to your applications. It uses machine learning to detect typed and handwritten text in a variety of documents, and to extract text, forms, and tables from documents with structured data.

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

* AnalyzeDocument
* AnalyzeExpense
* AnalyzeID
* CreateAdapter
* CreateAdapterVersion
* DeleteAdapter
* DeleteAdapterVersion
* DetectDocumentText
* GetAdapter
* GetAdapterVersion
* GetDocumentAnalysis
* GetDocumentTextDetection
* GetExpenseAnalysis
* GetLendingAnalysis
* GetLendingAnalysisSummary
* ListAdapterVersions
* ListAdapters
* ListTagsForResource
* StartDocumentAnalysis
* StartDocumentTextDetection
* StartExpenseAnalysis
* StartLendingAnalysis
* TagResource
* UntagResource
* UpdateAdapter
