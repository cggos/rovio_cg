## Build Configuration
There are different build variables that can be set when compiling rovio:
* ROVIO_NMAXFEATURE: Maximal number of features for rovio
* ROVIO_NCAM: Number of enabled cameras
* ROVIO_NLEVELS: Number of image levels for the features
* ROVIO_PATCHSIZE: Size of patch (edge length in pixel, lowest pyramid level)
* ROVIO_NPOSE: Calibration mode for handling additional external pose measurements. 0: no additional calibration, 1: the transformation between the rovio inertial frame (W) and the reference inertial frame (I) is co-estimated, 2: the transformation between the inertial frames (like 1) and between the two body frames (IMU M and reference body V) are co-estimated.

For instance you can configure rovio to use two cameras by using -DROVIO_NCAM=2. If you change these configurations you have to rebuild rovio (if using catkin build you might have to enforce a rebuild).

## Camera Intrinsics
The camera intrinsics have to be provided by a yaml-file for each employed camera. The path of the yaml-file can be set in _rovio.info_ (Camera0 - CalibrationFile) or alternatively be loaded through rosparam (e.g. \<param name="camera0_config" value="$(find rovio)/cfg/euroc_cam0.yaml"/\>). There is an example yaml-file in the _/cfg_ folder.

See https://github.com/ethz-asl/kalibr for a possible calibration tool.

## Camera Extrinsics
The IMU-camera extrinsics are calibrated online (recommended). However reasonable initial guesses have to be provided in order for ROVIO to converge and not to drift away. Also, if your initial guesses is not very accurate be sure to sufficiently excite the sensor setup (especially rotational motion).

The initial guess can be set in the _rovio.info_ file. You have to provide qCM (quaternion representing the rotation from IMU to camera in Hamilton-convention) and MrMC (position of camera w.r.t. the IMU coordinate frame). MrMC is not very critical and a (0,0,0) initial guess is often good enough. qCM is more critical as it can strongly vary between sensor setups. If you do not know the relative orientation between your IMU and camera you can try the different 90 degrees rotation that exists in 3D (e.g. (0,0,0,1), (0.707,0,0,0.707), (1,0,0,0), (-0.707,0,0,0.707), ..., there are 24 different possibilities). One initial guess should end up in a stable estimation and you can read the calibrated qCM and MrMC by enabling the verbose in _rovio.info_.

## Rovio Parameters
You can set many parameters within _rovio.info_. A short description is provided for every parameter. Here are the mose important parameters:
* Common - doVECalibration: should the IMU-Camera extrinsics be co-estimated online.
* Common - vebose: enables the verbose
* CameraX - CalibrationFile: path to the calibration file of camera X
* CameraX - qCM: quaternion representing the orientation of camera X with respect to the IMU coordinate frame
* CameraX - MrMC: position of camera X with respect to the IMU coordinate frame
* Init - State - ***: initial values for the rovio filter state
* Init - Covariance - ***: covariance of the initial values of the rovio filter state
* ImgUpdate - useDirectMethod: should the EKF use the photometric error (true) or the repojection error (false)
* ImgUpdate - startLevel: highest pyramid level employed for the photometric error, must be smaller than ROVIO_NLEVELS
* ImgUpdate - endLevel: lowest pyramid level employed for the photometric error
* ImgUpdate - UpdateNoise - nor: covariance used for the repojection error (if not using the photometric error)
* ImgUpdate - UpdateNoise - int: covariance used for the photometric error
* ImgUpdate - initCovFeature_0: initial covariance of the distance parameter
* ImgUpdate - initDepth: initial distance parameter guess of a feature
* ImgUpdate - penaltyDistance: increase to 100 for outdoor environment to avoid clustering of features on horizon
* Prediction - PredictionNoise - ***: prediction noise employed by EKF, should be adapted to the IMU specification
* PoseUpdate - ***: parameters used if additional external pose measurements are provided, e.g. can be used for fusing GPS measurements

Note: if for some reason the info file is not correct rovio will generate a new info file and output a warning. For instance if you employ more than two camera the standard _rovio.info_ will not work anymore and you will have to add the proper number of cameras to it.