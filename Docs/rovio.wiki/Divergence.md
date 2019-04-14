# Rovio Diverges - Why?
There are many reason why rovio may diverge.
Most often it is caused by a bad setup (calibration, timing) - we will come back to this later.
Rovio can also diverge, even if everything has been setup correctly, but this should never happen right at the start.
It is often due to the lack of motion (or very large scene depth).
In these cases providing information from further sensors may be helpful (rovio supports modular external pose measurements (e.g. GPS) or velocity measurements (e.g. from wheel odometry).
As a side note: once rovio diverges the chance are small that it will catch itself again. However, it can simply be re-initialized on the fly.
If you compiled the visual scene just press 'r' to reset, otherwise you can call the corresponding ros node service.


### Test 1 - No Motion
One of the first tests that should be made is to record a dataset without any motion.
If rovio diverges in this case this means that something is terribly wrong.
Errors in the calibration or sensor timing cannot corrupt the output as there is no motion.
Causes should be search on the programming side: are all files formatted correctly? are the measurements within a reasonable range? no nan? are the timestamps within a reasonable range?


### Test 2 - Little Motion
If rovio passes this first test, but diverges quickly once the sensor is moved, then something is still pretty wrong.
The two most common issues are bad calibration and bad timestamps.
Bad calibration can concern the IMU-camera extrinsics or the camera intrinsics.
The rotational component of the extrinsics can typically be assessed by looking at the virtual horizon in the top left corner of the tracking frame.
If this horizon does not match the real horizon than something is wrong with rotational part of the camera-imu extrinsics.
The linear part of the camera-imu extrinsics is less critical: if just set to zero rovio will still often work but exhibit lower accuracy or be less robust.


Bad timestamps is a very commonly encountered issue.
This means that the REFERENCE time of the timestamps in the different measurements topics is not the same, e.g., your IMU and your camera use different clocks for generating the timestamps of the messages.
The magnitude of the effect depends on how different your references are.
If it is only a couple of milli-seconds than rovio will still often work but with decreased performance.
If it is above 10ms then this will quickly corrupt the filter state and can lead to fast divergences.