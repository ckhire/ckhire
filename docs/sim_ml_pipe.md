## Index 
1. Introduction<br/>
2. Data Science Team Responsibilities<br/>
&emsp;2.1 Data collection<br/>
&emsp;2.2 Data validation & filtering<br/>
&emsp;2.3 Data Annotation & Augmentation<br/>
&emsp;2.4 Data Pre-processing Pipeline<br/>
&emsp;2.5 Exploratory Data analysis<br/>
&emsp;2.6 MLops & data management responsibilities<br/>
3. Data Standardization & Workflow pipeline<br/>
&emsp;3.1 Data Workflow management using DVC+Git<br/>
&emsp;3.2 DVC+Git to track all AI training logs & data artifacts<br/>
5. AI/NN End-to-end Pipeline
6. Team Structure
7. Development Process
8. References

***
## Introduction
This page introduces data science & annotation team scope & responsibilities also data Standardization and workflow pipeline management along with type and nature of efforts required to handle the data related jobs and also explains the development approach, methodologies, coding practices and the tools to be used for this project. This ensures better development & research quality in a short time from a small team with granular updates and complete transparency and avoids filthy code and development practices.


***
## Data Science Team Responsibilities
The primary roles and responsibilities of the data science developers are more oriented towards defining managing and implementing following stages as per the defined by company standards design and various types and nature of the datasets.
### Data collection
This stage includes collection of contextual data from different data sources, data collection process also need to make sure the overall integrity and usability of the data from its respective data sources.

### Data validation & filtering
This stage includes the process of fixing or filtering incorrect, corrupted, incorrectly formatted, duplicate, or incomplete data samples within a dataset. When combining multiple data sources, there are many chances for data to be duplicated or mislabeled.the data science developer also need to review existing datasets for the annotation relationship with the corresponding data samples(images), the annotations for the data sample must match the use-case specific requirement that's been mentioned for the give problem statement.


### Data Annotation & Augmentation
This stage mainly involves annotating the data samples in the dataset if not already annotated for most of the cases the datasets currently used for the detection , classification ,segmentation, problems are publicly available and annotations are available for them, but there might be situation where some datasets are not annotated in that case developer has to manually annotate the data samples for the given dataset, the tool that's been used for annotating the dataset images is [remo](https://remo.ai/), the augmentation stage involves augmenting the data samples to handle various external environmental situations which are most obvious in production environments, the data samples need to be augmented for both train test/val datasets respectively.

### Data Pre-processing Pipeline
This stage is based on data analysis stage of the data science pipeline, this stage mainly involves experimentation of various pre-process transforms and DNN, image processing primitives to evaluate and analyze the behavior of the datasets with respect to NN model architect which will eventually depends on use-case specific problem statement and also training pipeline which can potentially produce more robust model in terms of real-time production oriented scenarios.

### Exploratory Data analysis
This stage includes analysis of the collected data for different pre-process transformed dataset by using certain supervised/unsupervised techniques like regression, clustering to appropriately classify the dataset w.r.t given use-case, using this it would be much easier to retrieve and classify feature specific information out of given dataset w.r.t their classes and ground truths. Apart from this standard E.D.A report for every use-case specific dataset also need to be generated which will describe from high level to low level aspects of the dataset.


### MLops & data management responsibilities
This stage involves the basic MLops skill-sets to deal with automated orchestration tools and scenarios like DVC, CML,etc. the data management responsibility mainly involves maintaining and keeping data safe, less data redundancy,and conducting regular dataset backup cycles

***
## Data Standardization & Workflow pipeline
In order to effectively store and maintain the versions of the dataset apart from normal code base the data science team need to make use of [DVC](https://dvc.org/) which is dataset version control & **Workflow Management Systems** - designed specifically to manage machine learning models and data artifacts. [DVC](https://dvc.org/) is built on top of Git. **Data Version Control** or **DVC** is an **open-source** tool for data science and machine learning projects. **Key features**:
-   Simple command line Git-like experience. Does not require installing and maintaining any databases. **Does not depend on any proprietary online services**.
-    Management and versioning of datasets and machine learning models. Data is saved in **S3**, Google cloud, Azure, Alibaba cloud, **SSH server**, HDFS, or even local **HDD** RAID.
-   Makes projects **reproducible** and **shareable**; helping to answer questions about how a model was built.
-    Helps manage experiments with Git **tags/branches** and **metrics tracking**.
![68747470733a2f2f6476632e6f72672f696d672f666c6f772e676966](https://user-images.githubusercontent.com/57352045/109312498-21df9000-786d-11eb-9f2a-1d5496347a9d.gif)

However dataset review mechanism framework will be independent of this , and will function at top of this, it can be tracked, as per basic understanding of DVC it can be utilized in similar fashion to git/git-LFS.Following is the complete end-to-end flow diagram for DVC work flow management system along with git and GitHub SaaS, diagram also include dataset review framework.
<div align="center">
<img align='center' src='https://user-images.githubusercontent.com/57352045/109822351-e07c2580-7c5c-11eb-873f-cbca823e9170.png'/>
</div>

### Data Workflow management using DVC+Git

versioning of the data/images managed using `dvc` is handled by git itself, `dvc` takes care of the data redundancy with the selected storage service, however creating release tags for specific commits is handled using `git` alone, so you can create N number of version releases using `git` alone, which may or may not share same set of images, the version releases in `git` are just pointers to the data managed by `dvc` over given storage service/drive.
All the dataset's will be managed on new GitHub branch as per there respective use-case specific naming convention, for instance all the detection dataset will be managed on branch with name `detection-dvc`, like wise all dataset for classification will go in the branch for `classification-dvc` branch etc. apart from this the following directory structure should be maintained in order to effectively manage the versions of the dataset,
<div align="center">
<img align='center' src='https://user-images.githubusercontent.com/57352045/111368793-4e443a80-86bc-11eb-8e8b-5903b91d9b7f.png'/>
</div>
Another important point to note is, there is a need to maintain respective use-case specific dataset's in one standardized format, so,

- for the **detection** dataset we have decided to put it in standard `darknet` format , this is the standard required annotation format while training on **Yolo** architectures, however in case of training given dataset over other algorithms like **SSD** which takes annotation data in **Pascal VOC** format in that case it can be converted to respective formats using the stage converters provided in the dataset framework, 
- For the **classification** dataset which will be utilized in normal classification task as well as feature extraction based NN's for asset protection, tracking, need not have to maintain separate annotation files hence will be stored in normal directory structure format as mentioned in the above diagram.
- Dataset annotation format for the **segmentation** algorithms will be nearly same to detection algorithms but change in the annotation file formats, however this can't be standardized currently , when relevant use-case come will decide on this at that time alone.
- **Multilabel** dataset format will also be nearly similar to the one described in the above figure, however there might some change in the directory structure but similar to the classification dataset this will also doesn't need to manage separate annotation files respectively.

**complete workflow management and commit history and how developers will use `dvc` with `git` has mentioned below**,

- the developer will checkout to desired dataset branch, for instance `detection` dataset,

    `git checkout detection-dvc`

    `dvc checkout`

    `dvc fetch`

    `dvc pull`

will add N number of images and annotation files(if any) to respective folder, as in this case it will be Detection folder, developer will put the manually verified or annotated dataset images/annotation files in the respective train/test folders, _if need to add validation folder it can be added to the given directory structure_.

**_developers will commit and push these files to S3 bucket using `dvc`, by following commands,_**

    `dvc add -R folder/`

In above command -R option will recursively add all the sub directories for independent data file, which will endup creating dvc file for every data file. so in case of large number of data samples simply avoid having this as argument, the data transmission(pull/push) will be much faster without -R option.

    `dvc commit`

    `dvc push -r <remote>`

- once all files get successfully pushed to S3 bucket, data science developers must commit all .dvc meta files to the GitHub using normal git commands,

    `git add folder/`

    `git commit -m <message>`

    **here the commit message must be in given naming convention format,**

    `<dataset use-case> <subset-number> <classes> <dataset version> <tag>`

    `git push`

**All the git commits will be pushed to the developers private fork only**, and then the PR to the base repo will be created,one thing to note here, any of the image/annotation file doesn't get pushed into the git repo even the dvc and git is in the same directory structure, _because all the data folders that are tracked by dvc are putted into .gitignore file, hence all the data files are ignored by the git_.

**_The data science developers must push whatever dataset subset they have prepared for the given day on daily basis, so it can be tracked by the T.L's and higher project managers, this approach introduces complete workflow transparency in the data science pipeline._**

- Later respective group of commit revisions can be reviewed and will be published as new dataset version on the GitHub itself. Once the respective dataset is released , it will be pulled on the training machine using following commands,

    `git checkout <release-tag/branch>`

    `dvc checkout`

    `git fetch <remote> <branch>`

    `git pull <remote> <branch>`

    `dvc fetch`

    `dvc pull`

- To remove certain image/annotation files, one has to first make sure they are on right branch,

    `git checkout <release-tag/branch>`

    `dvc checkout`

    Then , sync with the upstrem remote using git fetch and dvc fetch, after that one can manually delete .dvc files along with actual data files, or following dvc command can also be used,

    `dvc remove /path/to/.dvc/file`

    one advantage of removing files using above command is dvc updates and tracks meta(config) files in the current workspace, hence it is recommended to use above command instead of manual deletion of the files.

    `dvc gc -w -c`

    above command will finally removes the deleted files from local cache, as well as from the backend cloud storage, <b>note this process is irreversible hence should be used with caution.</b>

**_The command sequence must be in the above mentioned order, because first the dvc meta files will be pulled from the git repo, and then the actual data will be fetched using the `dvc` commands._**

**NOTE**: _All the dataset related operational scripts like converting dataset annotation files from one format to the other, and other data upload download related scripts have been relocated to `framework.scripts` package, these scripts will be used for temporary purpose till framework stage implementation get's completed, later all these operations will be consumed using framework stages alone._

**Disclaimer**: _In order to create a new branch for completely new dataset use-case , like for example segmentation or multilabel classification or modifying some meta things in the existing DVC pipeline using either git or DVC, **datascience developers must inform this to there T.L's ** before making any of these changes, **even in case of deleting some images or dataset revisions** T.L's must be informed, **in case of any damage/corruption to data stored over cloud or even local workspace cache the developer themselves will be convicted as responsible for the cause of happening**._


### DVC+Git to track all AI training logs & data artifacts
As multiple experimentation's are conducted for a use-case specific implementation it becomes difficult to maintain versions of every model weights/checkpoints, tensorboard logs and its evaluation files on GitHub, due to this reason DVC must be integrated for versioning all training related artifacts along with the existing code base.
Following structure needs to be followed for maintaining the model weights and its respective evaluation files,

![Screenshot from 2021-12-14 15-34-30](https://user-images.githubusercontent.com/72791642/145977410-a59ab110-eaeb-48a6-bcc0-f1c1251bd1f0.png)

Apart from the above dir structure one has to also include dir for `meta_info` inside of every experimentation dir, which will signify following things related to given experiment,
1. Model architecture represented using graph diagram or model summary.
2. Optimizer used in the experiment
3. Loss function used
4. pre-process transform used for train/val data
5. other hyper-parameters like lr_rate, momentum,etc.
6. model has fine-tuned(transfer learning) or trained from scratch.

**NOTE**: For versioning convenience it is good to enclose `dvc push` inside(before) of `git push` , one can follow below execution sequence,


`dvc add`<br/>
`dvc commit`<br/>
`dvc push`<br/>
`git add`<br/>
`git commit`<br/>
`git push <up_stream>`


using this, upstream git commit itself will ensure that `dvc push` has already been executed and successful, in case of any technical issue with `dvc` commit/push the subsequent `git push` won't happen until the issue gets resolved, which eventually signifies the respective `dvc push` command was successful, and no explicit information or notification is required as done.


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
This phase mainly involves selection of appropriate inference engine framework by considering the deployable production scenarios, like in case of CPU based deployment we can make use of OpenVino and for GPU's TensorRT is the choice. This also includes **Inference Optimization** using which the research oriented models can be optimized using inference engine accelerators.


## DEVELOPMENT PROCESS/ Code of Conduct

### All developers are supposed to follow below guidelines:

1. Listing the To Do?s discussing them and figuring out the act on To Do?s to be created on the Project board associated with respective teams. Project board will be automated Kanban.
2. Creating the Issues from Kanban To Do board and assigning to respective developers.
3. Developers will brainstorm and add the MoM in the assigned issue discussions.
4. Developers will create the UML design for a feature/module/sub-module and discuss it on the Github issue.
5. Reviewers are supposed to review, give feedback on the issue comments and provide their approvals for coding.
6. Developers will code on their own repository(private fork) and after completing will create the Pull Request.
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

***

## References
1. [DVC documentation](http://dvc.org/doc)
2. GitHub [discussions](https://github.com/onearmbandit/VAS-Data-Science/discussions) 
