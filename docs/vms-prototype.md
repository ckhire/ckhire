## Background (March 2020)
This is the prototype of the actual large scale VMS AI Solution. This prototype was develope with multiple properties.
1. To have framework which holds all of the experiments
2. Allows benchmarking the models across multiple inference frameworks
3. Deploying model with same rules and events but as hybrid that is on both CPU, GPU together
4. Benchmarking the performance of frameworks with same models in real-time scenario at client side
5. Calculating the accuracy of the deployed model and its latency
6. Allowing easy expansion of the framework to accomodate more models
7. Extension to write new NN Model and train it in same framework
8. Reduce the gap of model inferencing to deployment in this case it was reduce to 98%. Many times it doesn't take more than  10 mins
9. It executes on windows and ubuntu. tensorrt python was added later.

## Architecture

![Screenshot from 2023-08-07 08-49-24](https://github.com/ckhire/ckhire/assets/60615631/c6c650b1-ba73-4423-82d8-d5882fa982d4)

## Description of every component is self explanatory. I will soon add some high level pipeline code to demonstrate the streaming to inference to sending output and adapting pipeline with new rule or deletion of stream.
