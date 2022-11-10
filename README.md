# Udacity_sensorfusion_project
This project is part of the sensor fusion module in the Udacity Self driving car engineer Nano Degree program

# SDCND : Sensor Fusion and Tracking
This is the project for the second course in the  [Udacity Self-Driving Car Engineer Nanodegree Program](https://www.udacity.com/course/c-plus-plus-nanodegree--nd213) : Sensor Fusion and Tracking. 
 **Object tracking** : In this part, an extended Kalman filter is used to track vehicles over time, based on the lidar detections fused with camera detections. Data association and track management are implemented as well.
 The following diagram contains an outline of the data flow and of the individual steps that make up the algorithm. 
![img_title_2_new](https://user-images.githubusercontent.com/32779283/201003926-ed26f909-3254-45e2-9dda-26187266397d.png)

In the tracking project we follow the following steps to implement a object detection and tracking algorithm 
Step 1: Kalman filter implementation.
In  thi project I have implemented an extended kalman filter EKF. An EKF has 2 important steps Prediction step and Update step. We implemen the code in filter.py 
we test the filter on a few frames and plot the RMSE root mean square error for the tracking performance as below.
![step_1_secene](https://user-images.githubusercontent.com/32779283/201005219-dc3e1b11-2ceb-49a0-953d-e76622e36b5f.png)

![step_1rmse](https://user-images.githubusercontent.com/32779283/201005225-902cab7d-59ee-4c2f-9269-9190d6846534.png)

Step 2: Track Management
Under trackmanagement.py we implement fnction handle_updated_track and manage_track. When a new track is introduces the track score and state are initialized. with subsequent scans the track score and state are update.
The update is done if the track is still within sensor view. A track is deleted if the score decreases below a certain threshold or the max covariance of the coordinates are exceeded.

![step_2_rmse](https://user-images.githubusercontent.com/32779283/201005660-3e04cc22-1c7a-45f6-93a2-730b82cf34ea.png)

step 3: Simple nearest neighbor SNN Data association
The code is implemented in association.py. we use the MHD mahalanobis distance along with gating method to associate a track with the measurement list.
![step_3_rmse](https://user-images.githubusercontent.com/32779283/201005821-606b63d1-b2ea-4ed5-915a-0c11c7e48183.png)
![step_3_confirmtrack](https://user-images.githubusercontent.com/32779283/201005836-becd057e-1c96-4d92-a945-38cc67406f86.png)

Step 4 : NonLinear Camera measurement model and Camera lidar fusion model
In this step we implement the code in the measurement.py file. We transform the coordinates from vehicle to sensor and use the camera intrinsic calibration parameters to calculate Hx matrix

![step4_scene](https://user-images.githubusercontent.com/32779283/201006410-9e225187-437d-499e-9e18-9ef5b2bbbf66.png)

* corrected with updated trackmanagement.py. RMSE is below 0.2.
![step_4_rmse](https://user-images.githubusercontent.com/32779283/201186739-3cbf309f-703f-4959-8efb-009965b73dcb.png)



final result
![my_tracking_results](https://user-images.githubusercontent.com/32779283/201007045-764b6b92-b7b6-4c9b-b9a2-95c1f05bfa96.gif)

Do you see any benefits in camera-lidar fusion compared to lidar-only tracking (in theory and in your concrete results)?
In theory, additional snesors benefit in real world scenarios. Both lidars and cameras have thier own weakness and strengths. Lidar is very precise with calculating distance and speed and Cameras are very good at object classification and detecting changes like heading, measurements like height width etc. in the above project we dont notice significant difference by adding additional sensor with limited data runs. Multiple sensors decrease uncertainity andmake the systemm more robuts.

Which challenges will a sensor fusion system face in real-life scenarios? Did you see any of these challenges in the project?
Challenges for sensor fusion may include special edge case scenarios where a particular sensor measurement may need to be down selected over other. For example a slow moving target vehicle in front may not be give greate results for heading and velocity to a camera sensor but a lidar can act better in such situations. hence in such cases advance rules may be needed to down select objects based on the source of sensors. More sensors add redundancy and improve system robustness.
Maintaining sensor health overtime can become a scalability challenge. High onboard compute power needed 

Can you think of ways to improve your tracking results in the future?
Including more camera and possibly a radar may improve results. Introduction of other algorithms such as GNN or  Joint Probabilistic Data Association may improve some results.
