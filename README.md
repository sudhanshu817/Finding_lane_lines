# **Finding Lane Lines on the Road** 
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

<img src="examples/laneLines_thirdPass.jpg" width="480" alt="Combined Image" />

Overview
---

When we drive, we use our eyes to decide where to go.  The lines on the road that show us where the lanes are act as our constant reference for where to steer the vehicle.  Naturally, one of the first things we would like to do in developing a self-driving car is to automatically detect lane lines using an algorithm. This project repo detects lanes from images.





[//]: # (Image References)

[image1]: ./test_images_output/solidWhiteCurve.jpg

[video1]: ./test_videos_output/solidWhiteRight.mp4

---
### 1. The project pipeline

The pipeline for detecting lane lines includes 5 steps:
* Convert the image to grayscale
* Apply gaussian blur filter to the image inorder to smooth it
* Do canny edge detection on the smooth image
* Mask the canny edges output to only get the edges in the region of interest
* Apply hough transform to the masked image
* Get two lines for each lane from the hough transform results

The output from hough transform contained multiple lines. After getting the edges from the hough transform, the goal was to have two single lines for each lanes. To do this I modified the draw_lines() funcction in the following way:
* Discard all the lines with a slope between (0.2, -0.2) as these are the lines which are perpendicular to the lanes. Also discard lines with infinite slope
* Divide the lines into two parts, one with positive slope and the other with negative slope
* For every line in both the group, calculate the slope and the intercept of the line in the form (y = m*x + c)
* Discard the lines which which are outliers from both the group using 2 standard deviation as the threshold
* Get the mean slope and mean intercept for both the groups, so that we have one line representing each group
* Draw these lines on the image

Sample output using the above pipeline:

![Output from the pipeline on a image][image1]


### 2. Shortcomings with current pipeline


One potential shortcoming would be, if the road has multiple objects which are aligned in the direction of lanes and is a part of the region of interest, then the pipeline might detect them as lane lines. (eg: The edge of the road) As you can see in the last output video, in some frames the divider at the edge of road is detected as lane line.

Another shortcoming could be if the lane lines don't fall in the region of interest due to a change in alignment of the camera, the pipeline would not detect any lines.

Another short coming would be road patches with very sharp turn.


### 3. Possible improvements to the pipeline

A possible improvement would be to have a more robust approach which discards the other edges being detceted by only keeping the two lines which don't have any other line between them and also have slopes in opposite direction, one positive and the other negative.

## Sample output from the pipeline on a real video

![Sample results on a video][video1]