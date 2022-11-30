# Simple Online and Realtime Tracking with a Deep Association Metric

Authors: Nicolai Wojke, Alex Bewley, Dietrich Paulus

**Note**: Before reading the SORT algorith, make sure you have a clear understanding about Kalman filter. You can read about the Kalman filter here: https://www.bzarg.com/p/how-a-kalman-filter-works-in-pictures/
![DeepSORT overview](https://miro.medium.com/max/1400/0*-S2EkuGhkP9tp9It.JPG)

## I. SORT(Simple Online and Realtime Tracking) Algorithm
There are 4 main steps in sort algorithm:
> Detection -> Estimation Model (Kalman Filter) -> Data Association -> Creation and Deletion of Track Identities

### 1. Object Detection

![Detection Step Image](https://miro.medium.com/max/1400/0*pMpxXL8rYX0JHVmk.JPG)

**To capitalise on the rapid advancement of CNN based detection, we can use  Faster Region CNN (FrRCNN) or You Only Look Once (YOLO) detection framework to use as the detection part of the SORT algorithm**

### 2. Estimation Model
- The representation and the motion model used to propagate a target’s identity into the
next frame
- The state of each target is modelled as:

  <img src="https://latex.codecogs.com/svg.image?x&space;=&space;[&space;u,&space;v,&space;s,&space;r,&space;u\dot{},&space;v\dot{},&space;r\dot{}&space;]^{T}" title="https://latex.codecogs.com/svg.image?x = [ u, v, s, r, u\dot{}, v\dot{}, r\dot{} ]^{T}" />
  
where u and v represent the horizontal and vertical pixel location of the centre of the target, while the scale s and r represent the scale (area) and the aspect ratio of the target’s bounding box respectively

- When a detection is associated to a target, the detected bounding box is used to update the target state where the velocity components are solved optimally via a Kalman filter framework 
                      
### 3. Data Association

![Data Association Step](https://miro.medium.com/max/1400/0*5ucyaaIb7cJ8Nb0d.JPG)

- In assigning detections to existing targets, each target’s bounding box geometry is estimated by predicting its new location in the latest frame. The assignment cost matrix is then computed as the intersection-over-union (IOU) distance between each detection and all predicted bounding boxes from the existing targets.

- The assignment is solved optimally using the Hungarian algorithm. This works particularly well when one target occludes another.

### 4. Creation and Deletion of Track Identities


