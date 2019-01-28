Human Following
===========

This is a desciption of the human_following repository, which extends the existing tas_group_2 package with the leg_tracker and the human_follower packages.


Leg Tracker
===========
Description:
--------------------

Show the git source and the corresponding paper 

Requirements:
--------------------

In order to run the leg_tracker the following packages/installations are required: 

- pykalman: pykalman is a python package that implements the Kalman Filter which is required to track the detected legs associated with a human 
- scipy: scipy is a python packages that 
- munkres: munkres is a python packages that 


Demonstration:
--------------------

After installing all dependencies and compiling the leg_tracker (catkin_make) it be run with the command: roslaunch leg_tracker joint_leg_tracker.launch
To only work on the leg_tracker there exist three demo files in the launch directory of the leg_tracker


Executables:
--------------------

In the src/ directory, the following executables can be found: 


- laser_processor.cpp: This node processes the laser scan data by creating the scan clusters and publishes them on the XY topic 
- cluster_features.cpp: This node subscribes the XY topic and calculates the features of the clusters: see Paper for more informations on the features
- detect_leg_clusters.cpp: This node uses cluster_features.cpp and detects the leg_clusters. The node publishes to /detected_leg_clusters topic
- train_leg_detector.cpp: This node can be used to train the forest tree model if the setup changes and the performance of the leg_detection decreases

Further, in the scripts/ directory, the following executable can be found: 

- joint_leg_tracker.py: This node receives the detected leg clusters and tracks people using a kalman filter and the global nearest neighbor method. It publishes on the /people_tracked topic


Topics:
--------------------

The leg_tracker requires two topics in order to work properly (Subscription): 

- /scan: leg_tracker receives the scan data from this topic

- /tf: leg_tracker receives all transforms from this topic

Publish: 

- /people_tracked: The joint_leg_tracker node publishes the position and orientation of the tracked people on this topic


Msgs:
--------------------

What are the message types?
Give links to the documentation of the message packages 

leg_tracker:
--------------------

include a picture of the running leg_tracker 


Human Follower
===========




