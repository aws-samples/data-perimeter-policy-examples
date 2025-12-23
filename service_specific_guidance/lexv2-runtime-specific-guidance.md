# Service-specific guidance: Amazon Lex V2 Runtime Service


This document outlines service-specific guidance for implementing a data perimeter for Amazon Lex V2 Runtime Service. 


Amazon Lex V2 Runtime Service is a fully managed artificial intelligence (AI) service that enables you to build conversational interfaces into applications using voice and text. It provides the runtime API for Amazon Lex V2, allowing developers to integrate natural language processing capabilities into their applications, chatbots, and interactive voice response systems.


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
* DeleteSession
* GetSession
* PutSession
* RecognizeText
* RecognizeUtterance
