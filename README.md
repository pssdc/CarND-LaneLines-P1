
**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

1. Made a copy of the image for processing
2. Converted the image to Grayscale (single channel) so reduce the image processing time and complexity.
3. Applied Gaussian blur before Canny edge detection to remove noisy artifacts.
4. Applied Canny edge detection to detect the edges.
5. The camera is mounted on top of the car hence the lanes would appear in the same general position. Given that, a trapezoidal region (region of interest) is selected to capture the lines of interest (lane lines on the road, reject everything else). We will call it masked image.
6. Applied Hough transform to detect the lines in the masked image. 
7. Applied the following techniques to extrapolate and draw the lines on top of detected lanes.
8. Calculated the slope of individual lines from Hough transform.
9. Ran few iterations to separate the positive and negative slopes that closely identified the lanes lines. 
10.  Slopes < -0.5 captured Left lanes lines. 
11.  Slopes > 0.5 & <0.9 captured Right lane lines.
12.  Middle points ( (x1+x2)/2, (y1+y2)/2) of the individual lines identified in Hough transform are used for to come up with the initial line equation (line AB in the figure) . This line is extrapolated to come up with the final drawn line (line A_ex B_ex in the figure). 
13. We have the equation of the top most (y=Ytop) and the bottom most (y=Ybot) line of the trapezoid.
14. Calculated the coordinates of A_ex using the equation of line AB and y=Ytop.
15. Calculated the coordinates of B_ex using the equation of line AB and y=Ybot.
16. A_ex B_ex is the final drawn line on top of the detected lane. 
17. The difference (delta) between the two consecutive frames (current frame and previous frame) to reduce the jitter in the video. The difference was calculated for the coordinates of A_ex and B_ex.
18. Applied a scaling factor (0.9) to reduce the delta.


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
