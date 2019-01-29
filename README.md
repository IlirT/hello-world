# TAS WS1819
====

branch: human_following

authors: Hao Wang and Ilir Tahiraj

contact: hw19@live.de and ilir.tahiraj@tum.de

====

Human Following
====
This is a desciption of the human_following repository, which extends the existing tas_group_2 package with the leg_tracker and the human_follower packages.

## Description

This leg tracker package is used to detect human legs and track them in case it is classified as a human. Based on geometric features and using Random Forest Trees as a classification method, leg clusters, obtained from the scan data, are classified as human or non-human. The random forest classifier is trained on 1700 human and 4500 non-human examples. Autonomous tracking of multiple people is performed by a multi-target Kalman Filter. Further, the global nearest neighbor method is used for the data association. 

In the human following context, the leg_tracker provides the position and orientation of a human, walking in sight of the robot. With that information, the robot continuously updates the navigation goal associated with the human position. 

The leg_tracker package can be found on the following github repository: https://github.com/angusleigh/leg_tracker

Additionaly, a conference paper can be found in the leg_tracker package. For the sake of completion, we stated the paper on this page again: 

[1] Leigh, Angus. Person Tracking and Following with 2D Laser Scanners. IEEE International Conference on Robotics and Automation. Seattle, 2015.


## Requirements

In order to run the leg_tracker the following packages/installations are required: 

#### pykalman
	pykalman is a python package that implements the Kalman Filter which is required to track the detected legs associated with a human 
	Clone from the github repository: https://github.com/pykalman/pykalman
	go into the package directory and type: $ sudo python setup.py install
#### scipy 
	scipy is a python packages for scientific methods and implementations
	$ sudo apt-get install python-scipy
#### munkres
	munkres is a python packages that implements the assignment method for the global nearest neighbor method
	$ sudo apt-get install python-munkres

## Demonstration

After installing all dependencies and compiling the leg_tracker (catkin_make) it can be run with the command: roslaunch leg_tracker joint_leg_tracker.launch.
To only work on the leg_tracker there exist three demo files in the launch directory of the leg_tracker

## Executables

In the src/ directory, the following executables can be found: 


#### laser_processor.cpp: 
	This node processes the laser scan data by creating the scan clusters 
#### cluster_features.cpp: 
	This node calculates the features of the clusters: see [1] for more informations on the features
#### detect_leg_clusters.cpp: 
	This node uses cluster_features.cpp and detects the leg_clusters. The node publishes to /detected_leg_clusters topic
#### train_leg_detector.cpp: 
	This node can be used to train the forest tree model if the setup changes and the performance of the leg_detection decreases

Further, in the scripts/ directory, the following executable can be found: 

#### joint_leg_tracker.py: 
	This node receives the detected leg clusters and tracks people using a kalman filter and the global nearest neighbor method. It publishes on the /people_tracked topic


## Topics

The leg_tracker requires two topics in order to work properly (Subscription): 

#### /scan: 
	leg_tracker receives the scan data from this topic
#### /tf: 
	leg_tracker receives all transforms from this topic

Publish: 

#### /people_tracked: 
	The joint_leg_tracker node publishes the position and orientation of the tracked people on this topic

Human Follower
====

## Description Human Follower

This human follower package is used to receive the position of a detected person (see leg detection package above) and pass it as a goal point to the path planner. Additionally, a P-controller is added to to control the distance range between the car and the tracked person. 

The function to compute the human-following velocity is based on the evaluation relationship results presented in the following paper:

A. Tomoya et al., A mobile robot for following, watching and detecting falls for elderly care. Procedia Comput. Sci. 112, 1994–2003 (2017)
(http://www.sciencedirect.com/science/article/pii/S1877050917314825)


## Requirements

In order to run the human follower package, a successful setup of the leg tracker package is required due to dependencies on classes from that package.

## Demonstration

After installing all dependencies and compiling the human_follower (catkin_make) it can be run with the commands: 
roslaunch human_follower human_goal_node
roslaunch human_follower human_follower_node

## Executables

In the src/ directory, the following executables can be found: 

### human_goal node
#### goalcontrol/goalcontrol.h
	
#### goalcontrol/goalcontrol.cpp

#### human_goal_node_controller.cpp: 
	This node receives the pose of a tracked person, transforms the coordinates from "base_link" to "map" frame  and publishes them to move_base as new goal.


#### human_follower node
#### control/control.h

#### control/control.cpp

#### human_follower_node_controller.cpp: 
	This node receives the pose of a tracked person and the calculated cmd_vel from move_base, and adapts the velocity commands based on the distance of the car to the tracked person with the goal of smooth and safe following.


## Topics

The human_follower requires three topics in order to work properly (Subscription): 

#### /people_tracked: 
	human_follower receives the person tracked data from this topic
#### /tf: 
	human_follower receives all transforms from this topic
#### /cmd_vel: 
	human_follower receives calculated velocity commands from this topic

Publish: 

#### /move_base/goal: 
	The human_goal_node_control node publishes the the human pose as a goal on this topic

#### /move_base_simple/goal: 
	The human_goal_node_control node publishes the the human pose as a goal on this topic

#### /cmd_vel2: 
	The human_follower_node_control node publishes the adapted velocity commands on this topic




	

