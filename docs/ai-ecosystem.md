## Good Desgin Pattern and good code contribution practices : Unkowingly created  a good Product

### Background (Feb 2020 - Dec 2020 )
In my last organization while we were working on AI-ML base Product, I was responsible for architecting, designing all of our work. Among many responsibilities few of them were mentoring new engineers, documenting all research work, leading the team, developing new algorithm, maintaining github repository and providing solution for any problem occuring withing the team or while creating the solution. In short all of the engineers were pre-occupied with actual task of researching and providing better solution. As we were part of small organization and we had very limited resources responsible for all of the task in AI-ML pipeline: <br>
search problem relevant data--> download raw format dataset --> write program to create proper dataset --> if required convert the dataset for annotation as supported by remo--> annotate the dataset --> review the annotated dataset--> clean the dataset --> re-convert the annotations to custom format (writing program) --> custom dataloader --> Dataset maintainance --> Exploratory Data Analysis of Data --> If passed dataset version release --> Training model on dataset --> Evaluating the model accuracy --> If good result creating model release --> maintaining model release with dataset version <br>

### Issues

- Ad-hoc addition of scripts for any intermediary stages directly with jupyter developed by each developer as per there conveniance and no standard
- No MLOPs setup or proprietary Annotation tool provided
- Annotation team were non-programmers and were very few. No understanding of AI/ML
- No standard dataset format.
- Incompatibility among different dataset sources
- Always requires different conversion scripts for converting to custom format or if any standard format decided then again source format to destination format conversion were inevitable
- Multiple times needs to convert input format as suitable for remo
- Difficult association of dataset version to ml-model
- No authenticity of scipts
- Checking data quality was not possible
- Model failure leads to finding root cause of it and then data quality was tested
- Entire cycle used to take ~3 to 6 months for 1 dataset

### Solution

1. Good Framework Easy Contribution ML-Engineers
(Sorry for long explanations. But this is the journey). I created simple design based on pipeline paradigm with Plugin Oriented Programming as Implicity behavior (I can't reveal). In short this is similar to creating NN Models in pytroch. So my backed boiler plate does some what same what's been done by pytroch. Adding any new scipt doesn't require to know the framework architecture. It just needs to know very small problem for writing the script. Like what is input, what is the output of script or are ther any side-ouputs. Developers were supposed to first figure out these things and then created dummy inputs and expected output or side outputs for it. ML-Developers were just supposed to inherit `Stage` and overwrite `work` function. This was for writing new scripts. For integrating existing scripts they used to call script entry point in `work` functions. Finally to test every developer were supposed to maintain there test case for it in test folder. <br>
This freed the ML developers from knowing Design Patterns and complexity around it and adding script or code was easy. But how it solves everything. Below diagram will explain it all. Note below diagram is just for explanation here. Actually we had lot many more things in this layer.

![Screenshot from 2023-08-06 17-13-10](https://github.com/ckhire/ckhire/assets/60615631/6212857d-2326-4931-a933-bed9c2a65328)

2. Integration with DVC + Github - Versioning
   Follwing same `Stage` base framework we integrated DVC + Github python and rest api with our framework. This forms the complete automated AI-ecosystem. ❔ Did I just said ecosystem 🤔.

```
- As explained stage could be connected to form pipeline
- Adding simple CLI input base pipeline involving stages from downloading -> extraction -> distribtuion -> git+dvc creating pr for each annotator -> pr pulling -> conversion stage -> Data cleaning -> Image Enhancement.
- Another pipeline having EDA -> Pushing report links as comment to github pr
- Pipeline are configurable. Executes every stage. Seems similar to MLOps isn't it?
```

3. ❓What about AI-NN Modelling and Evaluation
   There was another framework developed internally we call it Multi-Faceted RAD. This was used for AI modelling and lighting fast deployment and testing with business cases. Integration of two was easy. I just created new stages and called the proper `execution` function in `NN script`. We already had some custom DataLoader in earlier framework. Rest could be taken from here. But still there are missing dots to connect

4. 😏 Integrating with SQLite
  Yes I integrated the SQLite in our libs. Thus our auditing and logs entry used get stored there. Most bigger advantage was when any model training or testing used to take place it used to record the model version, dataset version and model result. There was one more stage which use to show the model-data association graph with results achieved and other properties. Something like below
![Screenshot from 2023-08-06 17-40-07](https://github.com/ckhire/ckhire/assets/60615631/ab56c2a1-71ce-41bb-9f03-aa68cebbe56e)

5. ❔What about annotation and review

I used tornado 6.1 and followed micro-architectue exposing all libs stages using API's. Adding one layer above to the libs. Finally we have very simple UI in node-js which uses the rest-api. Providing very simple user-interface to our annotation team and allowing them to annotate and review the annotation. 

6. 🎲 Make-Sense Game Changer

~ Jan 2022 I took fork of Make-Sense. I don't have background of react or node. But I understood the desing patterns and I ❤️ `redux`. <br>

*  🐌 Power-off and Auto-Refresh. This slowed us down.
*  ✍️ I added browser base persistent layer storing ImageData and Lables. I added code to read these information at refresh and at begining of the Application. This taught 👨‍🏫 nodejs and react.
*  🎩off to 🤟 Design patterns and 🪄 of react and node
*  ❤️ Asynchronous behavior of react and redux

7. Wrapping it all
  We had 3 layer architecture Enhanced Make-sense UI, micro-services , python ML library. Most importantly we put below roles in our UI and customised it to very larger extent
- Annotator
- Reviewer
- ML Engineer
- ML QA Validation
- Manager/ AI Engineer <br>
  Each of this role has its own profile and login and own set of functionalities

  ## Very soon I will add list of functionalities in our UI and ecosystem
  



   


