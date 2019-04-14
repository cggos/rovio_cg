# rovio_cg

Modified version of **[ROVIO](https://github.com/ethz-asl/rovio)** (commit 3389c2a  on Dec 19, 2017).

# Install & Build

1. install **[kindr](https://github.com/ethz-asl/kindr)**

2. download the project and its dependency **lightweight_filtering**
    ```sh
    mkdir -p ws_rovio/src
    cd ws_rovio/src
    git clone https://github.com/cggos/rovio_cg.git
    cd ..
    wstool init ./ rovio_cg/dependencies.rosinstall # lightweight_filtering
    ```
3. build

   without opengl scene
   ```sh
   catkin build rovio --cmake-args -DCMAKE_BUILD_TYPE=Release
   # or
   catkin_make -DCMAKE_BUILD_TYPE=Release -j2
   ```

   with opengl scene
   ```sh
   sudo apt install freeglut3-dev, sudo apt-get install libglew-dev # dependencies

   catkin build rovio --cmake-args -DCMAKE_BUILD_TYPE=Release -DMAKE_SCENE=ON
   # or
   catkin_make -DCMAKE_BUILD_TYPE=Release -j2 -DMAKE_SCENE=ON
   ```

# Run

## with Euroc Datasets

```sh
roslaunch rovio rovio_node.launch [rviz:=true]
rosbag play MH_01_easy.bag
```

# Notes

* Camera matrix and distortion parameters should be provided by a yaml file or loaded through rosparam
* The cfg/rovio.info provides most parameters for rovio. The camera extrinsics qCM (quaternion from IMU to camera frame, Hamilton-convention) and MrMC (Translation between IMU and Camera expressed in the IMU frame) should also be set there. They are being estimated during runtime so only a rough guess should be sufficient.
* Especially for application with little motion fixing the IMU-camera extrinsics can be beneficial. This can be done by setting the parameter doVECalibration to false. Please be carefull that the overall robustness and accuracy can be very sensitive to bad extrinsic calibrations.
