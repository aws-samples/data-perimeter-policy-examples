# Service-specific guidance: Amazon Athena


This document outlines service-specific guidance for implementing a data perimeter for Amazon Athena.

Amazon Athena is an interactive query service that makes it easy to analyze data directly in Amazon Simple Storage Service (Amazon S3) using standard SQL. Athena is serverless, so there is no infrastructure to set up or manage, and you pay only for the queries you run.

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

* BatchGetNamedQuery
* BatchGetQueryExecution
* CancelCapacityReservation
* CreateCapacityReservation
* CreateDataCatalog
* CreateNamedQuery
* CreateNotebook
* CreatePreparedStatement
* CreateWorkGroup
* DeleteCapacityReservation
* DeleteDataCatalog
* DeleteNamedQuery
* DeleteNotebook
* DeletePreparedStatement
* DeleteWorkGroup
* ExportNotebook
* GetCalculationExecution
* GetCalculationExecutionCode
* GetCalculationExecutionStatus
* GetCapacityAssignmentConfiguration
* GetCapacityReservation
* GetDataCatalog
* GetDatabase
* GetNamedQuery
* GetNotebookMetadata
* GetPreparedStatement
* GetQueryExecution
* GetQueryResults
* GetQueryRuntimeStatistics
* GetSession
* GetSessionStatus
* GetTableMetadata
* GetWorkGroup
* ImportNotebook
* ListApplicationDPUSizes
* ListCalculationExecutions
* ListCapacityReservations
* ListDataCatalogs
* ListDatabases
* ListEngineVersions
* ListExecutors
* ListNamedQueries
* ListNotebookMetadata
* ListNotebookSessions
* ListPreparedStatements
* ListQueryExecutions
* ListSessions
* ListTableMetadata
* ListTagsForResource
* ListWorkGroups
* PutCapacityAssignmentConfiguration
* StartCalculationExecution
* StartQueryExecution
* StartSession
* StopCalculationExecution
* StopQueryExecution
* TagResource
* TerminateSession
* UntagResource
* UpdateCapacityReservation
* UpdateDataCatalog
* UpdateNamedQuery
* UpdateNotebook
* UpdateNotebookMetadata
* UpdatePreparedStatement
* UpdateWorkGroup
