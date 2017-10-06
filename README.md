

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.
1. Made a copy of the image for processing.
2. Converted the image to Grayscale (single channel) to reduce the image processing time and complexity.
3. Applied Gaussian blur before Canny edge detection to remove noisy artifacts.
4. Applied Canny edge detection to detect the edges.
5. The camera is mounted on top of the car hence the lanes would appear in the same general position. Given that, a trapezoidal region (region of interest) is selected to capture the lane lines on the road and reject everything else. This is the masked image.
6. Applied Hough transform to detect the lines in the masked image. 
7. Applied the following techniques to extrapolate and draw the lines.
   * Calculated the slope of individual lines from Hough transform.
   * Ran few iterations to identify the slopes of the lines to separate the left and right lanes. 
   * Slopes < -0.5 captured Left lanes lines. 
   * Slopes > 0.5 & <0.9 captured Right lane lines.
   * Middle points ( (x1+x2)/2, (y1+y2)/2) of the individual lines identified in Hough transform are used to come up with the first line equation (line AB in the figure - unextrapolated line) . This line is later extrapolated to come up with the final drawn line (line A_ex B_ex in the figure). 
 ![extrapolation](/Extrapolation1.jpg)
   * We have the equation of the top most (y=Ytop) and the bottom most (y=Ybot) edges of the trapezoid.
   * Calculated the coordinates of A_ex using the equation of line AB and y=Ytop.
   * Calculated the coordinates of B_ex using the equation of line AB and y=Ybot.
   * A_ex B_ex is the final drawn line on top of the detected lane. 
8. The difference (delta) between the two consecutive frames (current frame and previous frame) is used to reduce the jitter in the video. The difference is calculated for the coordinates of A_ex and B_ex.
9. Applied a scaling factor (0.9) to reduce (smoothing) the delta.



### 2. Identify potential shortcomings with your current pipeline

1. The video output still shows some jitter. 
2. The pipeline and the area of interest may work in a very specific conditions - good sunlight, very small curvature on the road and good visibility. 
3. The pipeline need some more tuning so that it can work on the challange video.

### 3. Suggest possible improvements to your pipeline

1. Currently only 2 frames are used to reduce the jitter in the video, potentally more frames (and use their mean) can be used to smooth out the jitter in the video.
2. For curved roads, multiple region of interest can be created and stacked on top of each other.
