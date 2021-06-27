# **Finding Lane Lines on the Road** 

## Repo Guide


---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Give an overview of how the project works


[//]: # (Image References)

[image1]: ./test_images_output/solidWhiteCurve.jpg

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

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

![Lanes detected in a image][image1]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be, if the road has multiple objects which are aligned in the direction of lanes and is a part of the region of interest, then the pipeline might detect them as lane lines. (eg: The edge of the road) As you can see in the last output video, in some frames the divider at the edge of road is detected as lane line.

Another shortcoming could be if the lane lines don't fall in the region of interest due to a change in alignment of the camera, the pipeline would not detect any lines.

Another short coming would be road patches with very sharp turn.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to have a more robust approach which discards the other edges being detceted by only keeping the two lines which don't have any other line between them and also have slopes in opposite direction, one positive and the other negative.