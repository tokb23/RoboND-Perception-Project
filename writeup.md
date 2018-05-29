# **Perception Pick & Place**

[![Udacity - Robotics NanoDegree Program](https://s3-us-west-1.amazonaws.com/udacity-robotics/Extra+Images/RoboND_flag.png)](https://www.udacity.com/robotics)

## Overview

**Steps to complete the project:**  
1. Extract features and train an SVM model on new objects (see `pick_list_*.yaml` in `/pr2_robot/config/` for the list of models you'll be trying to identify).
2. Write a ROS node and subscribe to `/pr2/world/points` topic. This topic contains noisy point cloud data that you must work with.
3. Use filtering and RANSAC plane fitting to isolate the objects of interest from the rest of the scene.
4. Apply Euclidean clustering to create separate clusters for individual items.
5. Perform object recognition on these objects and assign them labels (markers in RViz).
6. Calculate the centroid (average in x, y and z) of the set of points belonging to that each object.
7. Create ROS messages containing the details of each object (name, pick_pose, etc.) and write these messages out to `.yaml` files, one for each of the 3 scenarios (`test1-3.world` in `/pr2_robot/worlds/`).  [See the example `output.yaml` for details on what the output should look like.](https://github.com/udacity/RoboND-Perception-Project/blob/master/pr2_robot/config/output.yaml)  
8. Submit a link to your GitHub repo for the project or the Python code for your perception pipeline and your output `.yaml` files (3 `.yaml` files, one for each test world).  You must have correctly identified 100% of objects from `pick_list_1.yaml` for `test1.world`, 80% of items from `pick_list_2.yaml` for `test2.world` and 75% of items from `pick_list_3.yaml` in `test3.world`.
9. Congratulations!  Your Done!

**Extra Challenges: Complete the Pick & Place**

10. To create a collision map, publish a point cloud to the `/pr2/3d_map/points` topic and make sure you change the `point_cloud_topic` to `/pr2/3d_map/points` in `sensors.yaml` in the `/pr2_robot/config/` directory. This topic is read by Moveit!, which uses this point cloud input to generate a collision map, allowing the robot to plan its trajectory.  Keep in mind that later when you go to pick up an object, you must first remove it from this point cloud so it is removed from the collision map!
11. Rotate the robot to generate collision map of table sides. This can be accomplished by publishing joint angle value(in radians) to `/pr2/world_joint_controller/command`
12. Rotate the robot back to its original state.
13. Create a ROS Client for the “pick_place_routine” rosservice.  In the required steps above, you already created the messages you need to use this service. Checkout the [PickPlace.srv](https://github.com/udacity/RoboND-Perception-Project/tree/master/pr2_robot/srv) file to find out what arguments you must pass to this service.
14. If everything was done correctly, when you pass the appropriate messages to the `pick_place_routine` service, the selected arm will perform pick and place operation and display trajectory in the RViz window
15. Place all the objects from your pick list in their respective dropoff box and you have completed the challenge!
16. Looking for a bigger challenge?  Load up the `challenge.world` scenario and see if you can get your perception pipeline working there!

## Project Details

### Exercise 1, 2 and 3 pipeline implemented

The following steps are performed for object segmentation:

* Statistical Outlier Filtering
* Voxel Grid Downsampling
* PassThrough Filter
* RANSAC Plane Segmentation
* Euclidean Clustering

<div><img src='./images/seg.png', width='500'></div>

And then, the following steps are performed for object recognition:

* Generate a training set of features for the objects in the pick lists
* Train a SVM with the generated training set

<div><img src='./images/recog.png', width='500'></div>

Here is the snapshot of the normalized confusion matrix from the SVM I trained:

<div><img src='./images/confusion_matrix.png', width='400'></div>


### Pick and Place Setup

Here are the screenshots of the output, showing label markers in RViz to demonstrate object recognition success rate in each of the three scenarios:

<div><img src='./images/w1.png', width='400'></div>
<div><img src='./images/w2.png', width='400'></div>
<div><img src='./images/w3.png', width='400'></div>
