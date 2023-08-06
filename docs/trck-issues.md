## Kalman base Prediction ##
Kalman is used to predict the next position of an object base on the previsous behaviour of an object. Kalman does not store any previous data. It works on state and has state matrix which gets updated. Kalman incurs very less cost as it is simple mathematical calculation. Kalman is use for tracking the missiles, germs, people etc.

## Behavior of Kalman in Tracking ##

In human tracking algorithm, Kalman assumes some initial point in the space. This point initial is the same as the centroid of a person. Gradually from initial point it tries to predict the next position of a person. Post prediction the tracking algorithm applies the association to associate the Kalman's predicted value with the actual value in the current frame. This association is base on the Nearest Neighbour algorithm. After association Kalman rectifies itself to predict next value more accurately.

**Observation** 
1. After 50-60 iteration Kalman instead of predicting it assigns the incomming detection value to itself. Thus after 50-60 iteration Kalman's predicted value is equivalent to the co-ordinates of the detection from the current frame.
2. When new person is introduce after some time tx then the predicted Kalman value for new person is the co-ordinates far away from that person. This creates issue in next frame. Because association now associates that Kalman's value with some other person. This is visualize as the spike dot on the window.
3. When previous person is out from the frame and at same instance some new person enters from the same area as that of person leaving then the list management gets complicated. This again causes some extra dots on the screen and tracking id switches. This results in connecting wrong predicted points with some other person's point and extra points appearing on the screen.

## Conclusion ##
1. We need new tracking approach to reduce the list management.
2. Historical Tracking approach is better instead of any prediction base approach.
3. Viewing the complete problem as the type of re-identification of the features from previous to next is the better way to solve this.
4. Instead of only 2-dimensional feature giving spatial information in 2D space it is better to have a large vector giving information of the person. This vector must be lighter but sufficient to distinguish the two person.
5. For association one need to consider the graph base approach or the bucket approach or NN to solve the nearest vector problem.
6. Current Association is solve using matrix, its complexity goes to O(n*n)*logn. This needs to be reduce. Matrix formation incurs more time
7. Tracking must actually consider only previous state and the current state.
8. Thus a complete re-design of the tracking is needed.

## Comming Soon ##
Deep Sort base tacking analysis
