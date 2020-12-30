.. OUTDOOR_NAV2 documentation master file, created by
   sphinx-quickstart on Tue Dec 22 16:24:53 2020.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Grid Map
========================================
`botanbot_grid_map` is a pakage that reads a prebuilt map in .pcd format and pubishes grid_map ith several layers.


This package will be auomatically cloned to workspace with the vcs tool. 
Following same fashion with google cartogrpaher, 
we need to record bag files and generate maps with data inside this bags. 
Assuming that we have recorded bag file that contains point cloud and imu(optional) with topic names `velodyne_points`, `imu/data`, we can generate maps with ;

.. code-block:: bash

   ros2 launch botanbot_grid_map botanbot_grid_map.launch.py 


.. image:: /images/grid_map_0.png
   :width: 700px
   :align: center
   :alt: rqt landing screen

.. image:: /images/grid_map_1.png
   :width: 700px
   :align: center
   :alt: rqt landing screen

