# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. 
1. First I resized all images to 960 by 540 as per the quiz/tutorial because I used vertices that were somewhat hardcoded.
2. I converted the image to grayscale and then used a canny filter with thresholds of 150 and 75 to extract edges from the bw image
3. I used a mask generated from the vertices I used and after masking used hough transform to extract lines from the masked bw image
4. Following this I used the lines information to calculate the slope and bias for each line and used a filter of slope > 0.5 for right lanes and slope < -0.5 for left lanes.
5. This results in nan values especially in the last video, I circumvented this issue by making my slope and bias means for the two lanes global and using old values in case of nans .
6. I further removed outliers that were beyond 1.2 standard deviations from the mean and then used the means recalculated from the outlier free data to plot lanes

One potential shortcoming would be what would happen when ... 
1. In this case I had to use a lot of filtering to plot relatively stable lanes like the example video,especially for the last video in the project.
2. In case of random inputs to the steering wheel, sudden changes in direction will not be registered and other methods may have to be used with drastic steering inputs.
3.The same can be said when high values of acceleration are recorded.


Another shortcoming could be ...
1. The region of interest is hardcoded and should be set dynamically or after calibration to compensate for changes in the vehicle or changes in the setup.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...
1. One possile improvement would be to use a dynamic set of vertices instead of fixed vertices for the region of interest.
2. This will help to determine the region of interest automatically depending of the camera setup.

Another potential improvement could be to ...
1. In order to accomodate gradual changes in the lanes in case of turns, lane markers can be detected using more sophisticated techniques to fit non linear curves through the detected lane markers.
2. If the confidence in detected lane markers is high, piecewise curve fitting can be used with a 2nd degree polynomial.
3. In my case I assumed that the angle of the lane markers to the vehicle is not in the range (-30 to 30) degrees which is not optimal.
