## Background

After having various experiments on tracking and benchmarking the performance of the Trackers provided in DeepStream. I implemented tacking with yolo kernel. I experimented it and results were really good.

## Challenges
- Solving matching we all know is O(n*n)logn complexity problem
- Thus having cuda kernel for it would waste lot of time
- Because it is CPU base problem. Otherwise we will spend lot of time in cuda thread synchronization

## Approach
- There is an approach to solve this problem in O(n*n) that too without cuda thread synchronization
- Below diagram shows the design
  ![Screenshot from 2023-08-07 08-11-11 (1)](https://github.com/ckhire/ckhire/assets/60615631/e6e5e267-208d-449e-88e3-9d6588562290)

- As we can see we are capturing some x features from yolo kernel. This is simply storing cuda results of the kernel in another variable. It requires bit of modification in the yolo kernel.
- I have written my own `nms-cuda version`. The output from `nms-cuda` is given added with `x-features`. Here is the biggest catch. As we know nms reduces the outputmap. Features we have are from all the candidates. But we don't need to worry about it because it is taken care in `nms output matrix`. Thus combining two matrix on the index basis is sufficient
- Then it calls the Tracker cuda function which just checks the stack and if it is empty it converts the combine matrix to tracking output. If stack has candidate then it would fetch the stack value and the current matrix would be given to `association kernel`. Association kernel finally creates final tracking matrix and put in stack.
- The main changes are done in `nms-cuda`, `association` and `final-matrix-tracking`. I will soon provide the python bindings and live video output of it.
- I will also add benchmark properly. But the latency of tracking was constant no-matter number of candidates appearing in the frame. The overall latency was ~1 milli-seconds post detection to fetching tracking output in cpu.



