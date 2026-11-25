# Service-specific guidance: Amazon Rekognition


This document outlines service-specific guidance for implementing a data perimeter for Amazon Rekognition.

Amazon Rekognition is a cloud-based image and video analysis service that makes it easy to add advanced computer vision capabilities to your applications. Using its APIs, you can detect objects, text, and unsafe content, analyze images and videos, and detect, analyze, and compare faces, without needing machine learning expertise.

The following table specifies whether additional considerations apply to a specific data perimeter control objective, followed by the list of considerations and recommended controls, if any.

| Perimeter type     | Security objective                                             | Applied on | Policy type         | Additional considerations |
|--------------------|----------------------------------------------------------------|------------|---------------------|---------------------------|
| Identity perimeter | Only trusted identities can access my resources                | Resource   | RCP                 | Y |
| Identity perimeter | Only trusted identities are allowed from my network            | Network    | VPC endpoint policy | N |
| Resource perimeter | My identities can access only trusted resources                | Identity   | SCP                 | N |
| Resource perimeter | Only trusted resources can be accessed from my network         | Network    | VPC endpoint policy | N |
| Network perimeter  | My identities can access resources only from expected networks | Identity   | SCP                 | N |
| Network perimeter  | My resources can be accessed only from expected networks       | Resource   | RCP                 | Y |

*Y - Additional considerations apply. N - No additional considerations apply.

## PutProjectPolicy
### No RCP support

**Perimeter type applicability**: identity and network perimeter applied on resource.

**Description**: [PutProjectPolicy](https://docs.aws.amazon.com/rekognition/latest/APIReference/API_PutProjectPolicy.html) allows you to apply a resource-based policy to grant access to a project. The service currently does not support RCPs.

**Additional controls**:

If you want to restrict access to trusted identities, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutProjectPolicy](https://docs.aws.amazon.com/rekognition/latest/APIReference/API_PutProjectPolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [PutProjectPolicy](https://docs.aws.amazon.com/rekognition/latest/APIReference/API_PutProjectPolicy.html) API calls in your environment (specifically, the [PolicyDocument](https://docs.aws.amazon.com/rekognition/latest/APIReference/API_PutProjectPolicy.html#rekognition-PutProjectPolicy-request-PolicyDocument) request parameter). If necessary, remediate with the responsive controls of your choice.

If you want to restrict access to expected networks, consider implementing these additional controls:
* Preventative control example: Consider restricting [PutProjectPolicy](https://docs.aws.amazon.com/rekognition/latest/APIReference/API_PutProjectPolicy.html) permissions to select principals using an SCP. See [restrict_resource_policy_configurations_scp.json](https://github.com/aws-samples/data-perimeter-policy-examples/blob/main/service_control_policies/service_specific_controls/restrict_resource_policy_configurations_scp.json) for an example policy.
* Detective control example: Consider using CloudTrail management events to monitor the [PutProjectPolicy](https://docs.aws.amazon.com/rekognition/latest/APIReference/API_PutProjectPolicy.html) API calls in your environment (specifically, the [PolicyDocument](https://docs.aws.amazon.com/rekognition/latest/APIReference/API_PutProjectPolicy.html#rekognition-PutProjectPolicy-request-PolicyDocument) request parameter). If necessary, remediate with the responsive controls of your choice.

## List of service APIs reviewed against data perimeter control objectives

The service APIs covered in this review are based on botocore version [1.43.29](https://github.com/boto/botocore/tree/1.43.29/botocore).

* AssociateFaces
* CompareFaces
* CopyProjectVersion
* CreateCollection
* CreateDataset
* CreateFaceLivenessSession
* CreateProject
* CreateProjectVersion
* CreateStreamProcessor
* CreateUser
* DeleteCollection
* DeleteDataset
* DeleteFaces
* DeleteProject
* DeleteProjectPolicy
* DeleteProjectVersion
* DeleteStreamProcessor
* DeleteUser
* DescribeCollection
* DescribeDataset
* DescribeProjectVersions
* DescribeProjects
* DescribeStreamProcessor
* DetectFaces
* DetectLabels
* DetectModerationLabels
* DetectProtectiveEquipment
* DetectText
* DisassociateFaces
* GetCelebrityInfo
* GetCelebrityRecognition
* GetContentModeration
* GetFaceDetection
* GetFaceLivenessSessionResults
* GetFaceSearch
* GetLabelDetection
* GetMediaAnalysisJob
* GetSegmentDetection
* GetTextDetection
* IndexFaces
* ListCollections
* ListDatasetEntries
* ListDatasetLabels
* ListFaces
* ListMediaAnalysisJobs
* ListProjectPolicies
* ListStreamProcessors
* ListTagsForResource
* ListUsers
* PutProjectPolicy
* RecognizeCelebrities
* SearchFaces
* SearchFacesByImage
* SearchUsers
* SearchUsersByImage
* StartCelebrityRecognition
* StartContentModeration
* StartFaceDetection
* StartFaceSearch
* StartLabelDetection
* StartMediaAnalysisJob
* StartProjectVersion
* StartSegmentDetection
* StartStreamProcessor
* StartTextDetection
* TagResource
* UntagResource
* UpdateDatasetEntries
