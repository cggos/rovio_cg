## Coordinate Frames
Rovio makes use of various coordinate frames:
* W: the inertial coordinate frame employed by the EKF.
* M: the coordinate frame attached to the IMU
* C: the camera coordinate frame (optic center, z is along camera axis, x/y span image plane, x right, y down)
* V: Reference body coordinate frame if external pose measurements are employed
* I: Reference inertial coordinate frame if external pose measurements are employed

An overview is given in the following drawing (courtesy of Bharat Tak)
![](https://cloud.githubusercontent.com/assets/3105122/11783183/051546d6-a276-11e5-9819-bfea8ca3ca58.png)

## Notation
* qAB: represents the quaternion rotating a vector from coordinate frame B to A, Hamilton-convention
* ArAB: represents the coordinate of a vector from A to B expressed in coordinate frame A