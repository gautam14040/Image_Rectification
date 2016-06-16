# Recovery of affine and metric properties from images

## Affine Rectification:
In this task we find the transformation that affinely rectifies the image and once found, we apply the transformation to warp the required image.
The whole idea is to find the transformation such that the line at infinity I,e, [0 0 1]T is mapped backed to its original position as in the world plane. Once rectified, all the affine properties can be seen in the output image, few such properties are preserving points, straight lines and planes such that all the sets of parallel lines those are parallel in the world plane remain parallel in the image as well. It may not necessarily preserve angles with respect to a pair of lines or distances with respect to a given pair of points but it will preserve ratios of the imaged distances and angles to the world distances and angles.

###Algorithm:
* First we try to find a pair of a parallel lines on the imaged plane in order to find the line at infinity.
* Since the line at infinity is moved from its canonical position to a finite location on the imaged plane, the first step would be to bring back this line to its original canonical position.
* It can be noticed that the affine properties of the first plane shown in the image above can be measured on the third plane since we have preserved the location of l8.
* Now, we find the projective transformation matric that transforms l1 to its canonical position [0 0 1]T
* Once found, we can apply this transformation to every point of the image in order to affinely rectify the entire image.

###Conclusion:
* Although affine rectification may not allow us to implement a certain fundamental viewing operations, the algorithm is very help in correcting geometric distortions and deformations that might be caused due to the poor quality of cameras.
* This technique can also be helpful for ideal measurements in satellite imaging etc.,
* One of the draw back of my algorithm is that it is semi-automatic meaning it requires user input to find the line of infinity and the accuracy depends on how accurately the user can locate the parallel lines on the image. However, computer vision algorithms can be used to automatically find the pair of parallel lines and then input the same to the above code.
* Note results 5 and 13, the output images are not much different than the input images, this is since the input images have very little affine distortion.
* In the result 14, we see that only one of the two real world planes are affinely rectified. This is one of the drawbacks, we can rectify the plane on which we choose the parallel lines and any other planes that may be seen in the image may not be rectified.
* From all of the above comparisons it can be noted that even though the affine properties of the planes in the real world images were preserved, the ratios of distances between points lying on a straight line could not be preserved, this is where the requirement of metric rectification comes in.

###How to use:
Type the following in a MATLAB instance:
affine_rect(*imread of input*,'*desired ouput name*')

## Metric Rectification:
In this case we find the transformation that metricly rectifies the image and once found, we apply the transformation to warp the required image.
The idea is to transform the circular points to their canonical position. The below section elaborates on how to find a projective transformation that can be the imaged circular points to their canonical location on the line of infinity. One found, the transformation between the world plane and the rectified image is then a similarity since it is projective and the circular points are fixed. There are a number of ways to obtain metric rectification, one of which is to use the dual conic C*8 . Here, the projective and affine components can be determined directly from the image of the dual conic C*8. Once the conic C*8 is identified on the projective plane then projective distortion may be rectified up to a similarity.

### Algorithm:
* We try to find the conic from the result of the following equation
lT C*8m = 0
* But since C*8 is 6 vector, we first try to find 5 pair of orthogonal lines on the imaged plane so that they can be stacked up as.
L = [l1 l2 l3 l4 l5]
M = [m1 m2 m3 m4 m5]
* Then, we find the linear constraint on the elements
(l1m1, (l1m2 + l2m1)/2, l2m2, (l1m3 + l3m1)/2, (l2m3 + l3m2)/2, l3m3) c = 0
* Once C*8 is found, a suitable holography is obtained that can retrieve the canonical position of the circular points.
* Once found, we can apply this transformation to every point of the image in order to metricly rectify the entire image.

### Conclusion:
* The same draw back goes for this algorithm also. it is semi-automatic meaning it requires user input. But, the work can be extend using computer vision alogithms to find the location of parallel lines or circles.
* The advantage of this paradigm is that the length and shape measurement on the world plane can be conveniently done from the image plane without explicitly recovering the homography parameters.
* Not in the above images that all the circles and the squares in the real world appear correctly in the rectified images.
* One of the biggest challenges in both the cases is to identify the gaps, cues, circles, lines etc., which form the means to locate the line of infinity (in case of affine rectification) and circular points (in case of metric rectification). It’d be very difficult (in this particular code) to obtain a good rectification if there are no such cues in the image.

###How to use:
Type the following in a MATLAB instance:
metric_rect1(*imread of input*,'*desired ouput name*')