.. OUTDOOR_NAV2 documentation master file, created by
   sphinx-quickstart on Tue Dec 22 16:24:53 2020.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

SLAM
========================================

We are exploring the opetantials of various SLAM algorithms in context of agri robotics.
So far 3 approaches has been experimented.
 The first two experiments were based on SLAM packages that consumed LIDAR.

1. Related to Google Cartographer 3D SLAM

Unfourtunately Cartographer has not been ported to ROS2 completely ATM. 
We can still create maps and visualize results, the `botanbot_cartogrpaher` is correctly configured and tested. 
But the node named `cartographer_asset_writter` has not been ported,this node is used to 
recieve actual 3D map,therefore we cannot exploit created map now. Hopefully soon enough this will be available. For exploting the actual maps created see the next SLAM option LIDAR ROS2 3D below this section.
We have added configuration package(`botanbot_cartogrpaher`) in order to build 3D maps 
using google cartogrpaher.
The main motivation behind this is; we have thoughts about switching envoirnment representationn from 2D to 3D, in that case a global prebuilt map of envoirnment might be necesarry(e.g a `.pcd` file to load instead of `.pgm`). 

In order to use the provided configuration package, 
one will ned to create ros2 bag files with required topics. 
For now we use 1 LIDAR and 1 IMU(Can be changed to 2 LIDAR in future). 
In order to record a bag file this the required topics, do the following; 

.. code-block:: bash
   
   ros2 bag record /velodyne_points /imu/data

A bag file will be created. Cartogrpaher expects that you define the rigid body 
transforms between sensor links and robot body frame(base_link). 
This transforms are defined in `botanbot_cartographer/urdf`. 
You might need to modify translation and roation between velodyne sensor and imu sensor 
for different setup. A strict calibration might not be necesarry between IMU and LIDAR. 

Also see the `cartogrpaher.launch.py` file and make sure the data topics are remapped correctly. 
After we have the bag file and configuration ready, we do the following to build the 3D map. 

.. code-block:: bash

   ros2 launch botanbot_cartographer cartographer.launch.py use_sim_time:=true bag_file:=${HOME}/rosbag2_2020_12_18-10_25_37/rosbag2_2020_12_18-10_25_37_0.db3

Wait for cartogrpaher to finish and do optimizations on the map. 

2. Related to LIDAR SLAM ROS2 3D PACKAGE

As second(but primary for now) SLAM package we have [lidarslam_ros2](https://github.com/jediofgever/lidarslam_ros2) package. 
The package is able to generate nice maps(in city like envoirnments) based on GICP/NDT. 

This package will be auomatically cloned to workspace with the vcs tool. 
Following same fashion with google cartogrpaher, 
we need to record bag files and generate maps with data inside this bags. 
Assuming that we have recorded bag file that contains point cloud and imu(optional) with topic names `velodyne_points`, `imu/data`, we can generate maps with ;

.. code-block:: bash

   ros2 bag play -r 0.5 rosbag2_2020_12_18-10_25_37/
   ros2 launch lidarslam lidarslam.launch.py
   rviz2 -d src/lidarslam_ros2/lidarslam/rviz/mapping.rviz

Note; if the sensor topic names are different then you need to recorrect them in tha launch file `lidarslam.launch.py`
execute all commands in seperate terminals. 
Here are a few example maps crreated when botanbot was taking a tour to gas station and retruning back. 

.. image:: /images/slam_0.png
   :width: 700px
   :align: center
   :alt: rqt landing screen

.. image:: /images/slam_1.png
   :width: 700px
   :align: center
   :alt: rqt landing screen

.. image:: /images/slam_2.png
   :width: 700px
   :align: center
   :alt: rqt landing screen

3. Related to OpenVSLAM
