<p align="right" ><b>Author</b>: Chaitanya Khire, Swapnil Udapure</p>

## Index 

1. Introduction
2. Core Evolution
3. AI/NN End-to-end Pipeline
4. Evaluation Matrix
5. Development Process

***


## Introduction
This page introduces the evolution of the AI/NN or AI/ML work from beginning to the current development and explains the development approach, methodologies, coding practices and the tools to be used for this project. This ensures better development & research quality in a short time from a small team with granular updates and complete transparency and avoids filthy code.


***


## Development Timeline (Core Evolution)

**August 2019, To Evaluate Pre-existing AI Models, AI/NN Team Model Evaluation Phase**:  Evaluation of  various AI models for the problem statement of having faster AI Models on CPU with better accuracy. Evaluation results for SSD MobileNet, SSD Inception, FAST RCNN, FASTER RCNN, YOLOv3, TinyYOLO, SSD MobileNetv2 have been shared. The frameworks used for these are TensorFlow, DarkNet and OpenCVDNN.
**AI/NN Team: Swapnil Udapure**

**October 2019, Rules and Events POC, Use Case development Phase**: In the previous phase SSD MobileNet was finalized as Object Detection Algorithm for optimal speed and performance on CPU. Various Rules like Line Crossing, Crowding, Person Moving in an Area, Loitering were implemented and demonstrated in this Phase.
**Frameworks: Python and OpenCV**
**AI/NN Team: Swapnil Udapure**

**November 2019, CPU based Inference Engine Evaluation, Inference Engine Evaluation Phase**: Opencv and TensorFlow as the inference engine were evaluated across official benchmark readings of TensorFlow servings. It was observed that TensorFlow was not  supporting c++ API. Similarly TensorFlow servings were fully containerized and third party models were not supported in it. As per the expert’s references Opencv and OpenVino were of same value thus OpenVino was kept for future evaluation.
**AI/NN Team: Swapnil Udapure**

**December 2019, Tracking Algorithm Evaluation and Implementation, Person Tracking Use case development Phase**: Opencv’s in build Tracking Algorithm, Dlib’s Tracking Algorithm and GoTurn Algorithm were compared with respect to speed and accuracy on CPU. It was observed that all these algorithms were having a lot of lag. Kalman Filter with Tracking as a POC was implemented and integrated with earlier Rules. Kalman Filter was faster than rest Tracking algorithm
**AI/NN Team: Swapnil Udapure and Chaitanya Khire**

**January 2020, Multi-channel Support Implementation and Evaluation, Enhancement of POC for Multiple Channel**: Enhancement of earlier POC to provide support for the multiple channels. Multi-threading and Multiprocessing with various ways of designs were implemented and evaluated.
During in-depth OpencvDNN API analysis it was found that Opencv’s Inference Framework is singleton in nature. Thus the root cause of low Multi-channel was with Opencv’s inference framework implementation. This was reported to Opencv by the team.
**AI/NN Team: Chaitanya khire and Swapnil Udapure, Kishore Parida**

**March 2020, Openvino as new Inference Engine and Research Framework Development**: Implementation of various custom object detection models.
Research Framework design and implementation for having better control and quantification of every module. Research Framework covered the boiler plate and provided a way to re-use the modules so that the framework could be integrated with other UI-Modules using Publisher/Subscriber pattern for communication between the UI Modules and that of the AI/NN Framework.
**AI/NN Team: Swapnil Udapure, Kishore Parida, Chaitanya Khire and Girish Joshi**

**May 2020, Expanding Research framework to include all product features, Alpha Development**: AI/NN Research framework was expanded to make it a product having all the features. Some components were re-design and architecture so that the framework should support Multiple-Rules on single or multiple ROIs on same or multiple channels on CPU. Rules and Event definition were implemented in the context of Product. Communication protocol was designed and implemented as part of integrating alpha with other UI components.In this phase framework got expanded to Multi-process, multi-channel Product Prototype which was released on 21st July providing support till Mid of August 2020. 
**AI/NN Team: Chaitanya Khire and Girish Joshi**

**May 2020, Evaluation of GPU based inference engines and development frameworks**: GPU based frameworks evaluated to extend the number of channel support and to optimize per channel infer latency , for this PyTorch framework has been evaluated for research based POC implementations, along with TensorRT framework for AI model deployment support, several object detection algorithms has been tested on pytorch and tensorrt framework to achieve optimal results which were Yolo, SSD, and few custom NN architectures. From July basic dataset collection for object detection and traffic classification has begun along with data analysis , traffic light classification initial architecture has been proposed with test evaluation results. 
**AI/NN Team: Swapnil Udapure and Chaitanya Khire and Kishore Parida**

**September 2020, Formulation of research to production pipeline and Core API Interface for Post Alpha development**: Standardized the research based implementation in order to get deployed using TensorRT framework, proposed and finalized design and architecture for end to end research to production pipeline along with core component design architecture.
For product perspective AI/NN deployment framework was finalized, designed and implemented as a core-library to be used by other modules like Core-Manager. It was finalized that the AI/NN deployment framework (known as Core) will be released incrementally in 45 days cycle
**AI/NN Team: Chaitanya Khire**

***

## Core Releases
* September 2020: 
1. Core v1.1: OpenvinoSSD
1. Core v1.2: Yolov3
Developers: Swapnil Udapure and Chaitanya Khire

* October 2020: Core v1.3: TensorRT and Openvino as backend for Linux Only,
Developer: Chaitanya Khire
* November 2020: Core v1.4: Support of Windows and Traffic Light API,
Developer Chaitanya Khire

<p align="center" width="100%">
    <img src="https://docs.google.com/spreadsheets/d/e/2PACX-1vQbKDv05_0UR5tSSr-lvV5s2Cv9qZ_QOAs_z_D_CuCXeKMx_L1E7VhGXTMq5VXpQj_6eBqoYpXbjtPl/pubchart?oid=221947548&format=image"> 
</p>

***

## AI/NN end to end pipeline<p align="center" >
 
<p align="center" width="100%">
    <img src="https://drive.google.com/uc?id=1JzDd0LNHnhH1k9FUKLYXLx5jv1Qod-X0"> 
</p>

### Data Collection & Analysis
**Data Collection**: This step includes collection of contextual data from different data sources, data collection process also need to make sure the overall integrity and usability of the data from its respective data sources.

**Data Analysis**: This phase includes analysis of the collected data for different pre-process transformed dataset by using certain supervised/unsupervised techniques like regression, clustering to appropriately classify the dataset w.r.t given use-case, using this it would be much easier to retrieve and classify feature specific information out of given dataset w.r.t their classes and ground truths.

**Data cleaning**: This phase includes the process of fixing or removing incorrect, corrupted, incorrectly formatted, duplicate, or incomplete data within a dataset. When combining multiple data sources, there are many opportunities for data to be duplicated or mislabeled.
Dataset Creation: Data Creation means the use of the Data, whether incorporated in a Solution or otherwise, to create new data samples using simulation or augmentation techniques, this process also involves data annotation and manual verification of the data samples.

### AI/NN Research & Development
**NN Model Research**: This phase involves researching and evaluating different NN architectures by referring to research papers or some existing architectures which best suits the problem statement use-case.Different NN framework used in this phase like PyTorch, TensorFlow etc.

**NN Model Training**: This phase includes training of NN models over dataset, and tweaking of hyper-parameters as per the evaluation results to produce best in class NN model, this recursive process involves rigorous tweaking of parameters and other components.

**NN model testing**: Once a model is trained over a given dataset, it needs to be tested on validation as well as real-time data, in order to validate the production capable accuracy of the model, this phase leads to generating evaluation metrics for the different model versions.

**NN model Optimization**: This phase involves optimization of NN architectures using different algorithms/techniques the optimization can occur at different stages from NN modeling to gradient optimizer to loss functions etc. techniques like post training quantization and quantization aware training also incorporated in this phase of development.

### AI/NN Deployment Solution
This phase mainly involves selection of appropriate inference engine framework by considering the deployable production scenarios, like in case of CPU based deployment we can make use of OenVino and for GPU’s TensorRT is the choice. This also includes **Inference Optimization** using which the research oriented models can be optimized using inference engine accelerators.

***

## Evaluation Matrix(readings)

Detailed evaluation parameters can be visualized from [here](https://docs.google.com/spreadsheets/d/12VHHlfkBbXrC_3XDJWZsQTxO5ym-SP0QBqJ64WAwZxs/edit?ts=6001a05d#gid=1728827555)
<p align="center" >
    <img src="https://docs.google.com/spreadsheets/d/e/2PACX-1vSySRsoQ5e630hALsryOuFNmqOXIfzvQMCPgiluHDRRl7f_rTCkXTQ7BcCWVy3blWMis4JvKVu6qxE8/pubchart?oid=87850311&format=image"> 
</p>
<table>
<tr>
<p align="center" >
    <img src="https://docs.google.com/spreadsheets/d/e/2PACX-1vQbKDv05_0UR5tSSr-lvV5s2Cv9qZ_QOAs_z_D_CuCXeKMx_L1E7VhGXTMq5VXpQj_6eBqoYpXbjtPl/pubchart?oid=1164707582&format=image"> 
</p>
</tr>
<tr>
<p align="center" >
    <img src="https://docs.google.com/spreadsheets/d/e/2PACX-1vQbKDv05_0UR5tSSr-lvV5s2Cv9qZ_QOAs_z_D_CuCXeKMx_L1E7VhGXTMq5VXpQj_6eBqoYpXbjtPl/pubchart?oid=1264471439&format=image"> 
</p>
</tr>
</table>

## DEVELOPMENT PROCESS/ Code of Conduct

### All developers are supposed to follow below guidelines:

1. Listing the To Do’s discussing them and figuring out the act on To Do’s to be created on the Project board associated with respective teams. Project board will be automated Kanban.
2. Creating the Issues from Kanban To Do board and assigning to respective developers.
3. Developers will brainstorm and add the MoM in the assigned issue discussions.
4. Developers will create the UML design for a feature/module/sub-module and discuss it on the Github issue.
5. Reviewers are supposed to review, give feedback on the issue comments and provide their approvals for coding.
6. Developers will code on their own repository and after completing will create the Pull Request.
7. Developers will ask reviewers to review the Pull Request.
8. After Reviewing and resolving all the comments on the Pull Request. The code will get merged.
9. After closing the issue, the Kanban board automatically moves the card to the Done column. [Here Kanban board is the reflection of what developers are doing and how things are getting done.]
10. After go-ahead for code, the developer has to upload the UML design in the Github Wiki.
11. Quality of UML, coding documentation will be reviewed by QA and other developers.
12. After getting the approval on code from peer developer a QA has to test the feature and after its approval it will merge to master/release-branch.

**Note**: _It is assumed that every developer has gone through the_ [Induction Document](https://docs.google.com/document/d/1i70_3TvKFoM6t53zvVl7QZDkmaQi9wGHcjrYy3ZCqcU/edit).


**GitHub Association with Teamwork as per current process:**

1. Every Issue on GitHub is not a task in teamwork.Only those issues having the label Task will be the task in the teamwork.
2. Every Task/issue on GitHub will have a milestone. This milestone is specified/attached in the Issue itself. Time for a milestone will be maximum of 15 days as discussed earlier. The exact time is specified on the given milestone.Milestone will be attached to other kinds of issues which may not be Task. for eg: A Bug, R&D, uses-cases etc.
3. Project Board Details: The Project Board for regular development will be the "Automated Kanban Board". This board has the following status which reflects the overall status of the month. The status and its description is as follow:

    1] **To-Do**: Issue attached for the current board will appear on this column(Check the label on this to know whether it is Task or any other stuff).

    2] **In Progress**: Any Pull-Request, issues attached for the given month/project-board will appear here. In case of Pull-Request --> That a particular developer has started working on it. By clicking this one can see the code in the pull request. In case of Issues --> There could be certain technical discussions or strategy related comments/documentation work. Similar to code this could also be seen.

    3] **Review in Progress**: Any Pull Request for which the developer has completed the development and request a reviewer to review the code, the respective card will appear on this column.

    4] **Reviewer Approved**: When the Pull Request is approved by the reviewer then the respective card appears here automatically.

    5] **On-Hold**: This card contains tasks/issues which are put on hold due to some reasons, by default this card can't be automated by kanban board, developer need to manually change the status of the issue to move it into this card.

    6] **Done**: When the Pull request is merged then the respective cards appear here. The Issues associated with the Pull request also get closed and moved from To-Do to Done. Same gets applied to other issues which are not Pull requests.

    `board links current month --> milestones of the month --> Issues --> Code --> Comments.`


