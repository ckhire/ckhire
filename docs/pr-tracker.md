## New Tracking Approach
1. Not to use any prediction algorithm.
2. Assoicate actual previous detection with the current detections.
3. Accuracy verification process will have logs explaining the state on every frame. The complete process of tracking must be readable in logs.
4. Separation of Tracking algorithm and the Line Crossing use case.
5. New Tracking algorithm available as the API for tracking.

## Algorithm
### I.  Tracking
1. Get FrameMetadata.
2. Read the frame and list of Detection from FrameMetadata.
3. Create the DetectedObjectState for BoundingBox of every class belonging to list of Detection.
4. Create the SurveillanceState from all DetectedObjectState.
5. Check 
	if SurveillanceState is empty:
		Then Push the SurveillanceState on SurveillanceStateStack
	else:
		consolidate the SurveillanceState from SurveillanceStateStack and the current SurveillanceState and put the new SurveillanceState on SurveillanceStateStack
6. Add the current SurveillanceState to FrameMetadata.

### II.  Association
It is self explanatory. Here is the actual algorithm solving the problem in maximum O(n*n). In last section latency readings are mentioned

## Advantages of Having Logs
For tracker there are logs. Along with it a test case could be written to store the frame at every instance to cross check with the logs.
So that error of visualization effect is not hidden because of human eye limitations.
Moreover it will be easy for finding the bugs and any calculating the accuracy.

## What is delivered
1. Tracking API is now not using the Kalman
2. Tracking API separate from the Line Crossing
3. Tracking API has new algorithm base on historical association. Thus it does not use any prediction algorithm
4. Tracking API base on feature vector of any length.
5. Tracking API different from the logic of extracting the features and calculating the distance between it.
6. So the current development has Tracking API, Feature extraction, Assoication resolution.
7. As promised there are no false points
8. There are no spikes.
9. No shifting of Ids within the same context.
10. Unique Id over the time.


## Advantages of Current tracking 
1. Python development is done in such a manner that it is directly translated into the Cpp code. (Along with the desings.) To achieve this the use of libs like numpy, cost matrix of scipy is avoided. Instead core algorithms and datastructures are written.
2. Python development explicitly make Tracking API different from its use case
3. Test Case could be added to demonstrate various use of tracking.
4. The overall design is such that the current cpp framework and the new up-comming won't be change. This is just the addon to it.
5. Design allows the introduction of new feature extraction technique, distance technique without modification of rest of the code.
6. Because the current development is done by keeping other points in mind it is better to profile this in cpp. Since the complete development is done to lay the designs of cpp. Otherwise this could be improved a lot by removing many data classes. But the actual aim is in cpp.


## Tracking Metrics
Here the latency of tracking is taken into consideration. This means it indicates the latency of `track` function any of its variants. Tracking is single threaded implementation which uses only CPU. This version doesn't focus on GPU base reading

*	### Average Case
The normal case where-in the there 6-7 person in each frame.

| Average Latency in ms | GPU Memory Consumption | GPU Utilization | RAM Consuption | CPU Utilization |
|:---------------------:|:----------------------:|:---------------:|:--------------:|:---------------:|
|1 ms | 0 MB | 0% | 633 MB | 18%|

In average case the range of time in micro seconds is from 800-1200 microseconds.

*  ### Worst Case
The worst case is when there are 50-100 person in each frame and ~80 person goes out of frame. This too is found after 40th continuous missed frame.

| Average Latency in milli-seconds | GPU Memory Consumption | GPU Utilization | RAM Consuption | CPU Utilization |
|:---------------------:|:----------------------:|:---------------:|:--------------:|:---------------:|
|6 ms | 0 MB | 0% | 642 MB | 18%|

The worst case timing needs to be taken out more concretely. All the above readings are on the frames saved in the disk. This is because the timing is for the tracking alone. Kindly note there is flickering, no wrong assignment and no extra objects. 

# Tracking Algorithm Performance Analysis
   Performance analysis is done on the basis of time taken by complete tracking module execution. In tracking module we get surveillance state of each frame. So to measure the performance of tracking we have considered 3 different approach. for each approach we have given 3 different samples.

## 1. Performance Analysis: First Version of Association Tracking [16 March 2020 ]
### Observation Table

 | Category | video duration | No of person | Average time(Micro sec.)/Frame | Remark |
 | :---: | :---: | :---: | :---: | :---:|
 | Low      |  30 Sec        | 5            |   1024 ms | total number of person in tracking goes to 8 person |
 | Medium   |  16 Sec        | 15           |  3224 ms  | total number of person in tracking goes to 17 person |
 | High    |  13 sec        | 30           |  6957 ms | - |

*  In the above table we have taken this readings on previous algorithm where it is map based and here we have taken the three aspect low, medium and high for analysis. low represent 5 objects in both frame and it requires 1024 micro sec. time to get association completed. As the number of person increases it takes time as respective. This algorithm doesn't have any scope to take it to parallelism.

# Optimization Strategies

We have implemented Two different algorithm where: 

* **First tracking algorithm** 🚀 In this algorithm the approach we are following is First Come First In for our complete association. The time complexity is too low as compare to all other algorithms. We have taken this module on tuple base only for association part, this tuple base algorithm does not have any scope to port the association on parallelism. This approach is also taken on multi threading but the time taken by the thread creation, thread join is 200% of actual execution. due to this time taken by the threads is not considerable. If the same approach is taken on cuda, that could save time.

* **Second tracking algorithm** 🚀 We have increased the complexity of code i.e. we have made our code more lengthy and divided and sorted it on the basis of cost. the cost base sorting and association is taking little bit more time. There is a scope for optimization in this algorithm.

* **Third Tracking Algorithm** 🚀  This algorithm actually increases the run-time complexity as compare to earlier algorithm version. The main idea behind this algorithm is to increase the complexity and to bring the possibility of parallelism/concurrency in solving the association problem.

## First Tracking Algorithm 🚀 [March, 2020]

### 1. Faster Tracking Association Algorithm :


 | Category | video duration | No of person | Average time(Micro sec.)/Frame | Remark |
 | :---: | :---: | :---: | :---: | :---:|
 | Low      |  30 Sec        | 5            |   764 ms | - |
 | Medium   |  16 Sec        | 15           |  2342 ms  | - |
 | High    |  13 sec        | 30           |  5267 ms | - |

This tracking algorithm actually follows First Come First In approach and thus the time complexity is very low as compare to others.

### 2. Faster Tracking Association Algorithm Multithreaded [Jun, 2021]:
In This approach the main objective was to decrease the time taken by cost finding and association of each object. here we have taken it on multi-threading to decrease the time taken by association.

 | Category | video duration | No of person | Average time(Micro sec.)/Frame | Thread Creation   Time | Thread join Time | Actual Thread execution Time|Remark |
 | :---: | :---: | :---: | :---: | :---:| :---:|:---:|:---:|
 | Low      |  30 Sec        | 5            |            2342 ms             |      300(Approx) ms  |       1048 ms     |     1300 ms  |      - |
 | Medium   |  16 Sec        | 15           |            5568 ms             |      750(Approx) ms  |       1782 ms     |     2844 ms  |      - |
 | High    |  13 sec        | 30            |            9216 ms             |      1800(Approx) ms |       2540 ms     |     4479 ms  |      - |


**_Conclusion:_** As we can see from the above table, having multithreaded is not beneficial. The main problem is thread over-head. **Thread overhead:** Thread creation, context switching, scheduling and internal OS base thread managing actually takes the above time.

As seen from the Actual Thread execution time, our algorithm should take 1/4 of total tracking execution. It means that as compare to earlier approaches this approach is actually taking 20% of less time. But drawback is that thread overhead is taking too much time to get the executed. actually it is taking 200% higher than earlier.

**_Remark:_** As per above conclusion if we could have a system where-in thread overhead is less and threads could be schedule parallel. That is if we write CUDA kernel equivalent to this then we should definitely get very less time.



## Second Tracking Algorithm 🚀 [December, 2021]

This Algorithm is tuple base and does have scope to put it on CUDA for Parallelism. this will resolve the problem...

 | Category | video duration | No of person | Average time(Micro sec.)/Frame | Remark |
 | :---: | :---: | :---: | :---: | :---:|
 | Low      |  30 Sec        |      5       |   800 ms  | - |
 | Medium   |  16 Sec        |      15      |  2648 ms  | - |
 | High     |  13 sec        |      30      |  5987 ms  | - |

## Comming soon the comparative tracking analysis of these algorithm with state of the art algorithm ##
