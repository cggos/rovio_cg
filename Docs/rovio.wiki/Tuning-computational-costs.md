The following parameters can be adapted to reduce computation costs (some of these parameters must be set before compilation):
* Reduce feature count to 15, minimum 10
* Reduce the number of processed image levels by getting startLevel and endLevel closer to each other, e.g. 2 and 1.
* Eventually disable patch warping if your application does not involve too fast motions
* Reduce the patch size to 6x6 or even 4x4
* Set alignMaxUniSample to 0 such that only 1 sample is evaluated when searching patches
* If no external pose measurements are used nPose should definetely be set to 0