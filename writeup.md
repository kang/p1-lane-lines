# **Finding Lane Lines on the Road**

---

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consists of 7 steps:

1. Convert the image to grayscale. This step sets the lines up to be recognized by gradient instead of by color.

2. Add a gaussian blur to smooth it out and reduce erroneous edges in step 3.

3. Grab all of the edges of the image using canny edge detection.

4. Add a mask, so that we only keep the edges in our region of interest. This is a region that we think the lane lines are likely to reside.

5. Use houghLinesP to turn the edges that we found in step 3 into a set of lines.

6. Draw the lines (quite a few added steps here)
  - Find the slopes and intercepts for all lines in the frame
  - If the slope is acceptable (fits with previous data points), then add the line to the slopes and intercepts of previous frames for their respective lanes (left or right)
  - Get the average slope and intercept of each lane based on the newly updated lists of slopes and intercepts
  - Extrapolate the lane by using the left and right average x-intercepts that we found before and the average slopes to a point we determine our lanes should end.
  - The slopes and intercepts are kept in memory so that we reduce abrupt and needless updates to the lanes. This smooths out the lane tracking

7. Overlay the lanes we drew from step 6 onto the original image.


### 2. Identify potential shortcomings with your current pipeline

There are quite a few limitations on the current pipeline that affect the accuracy of the lanes.

It has only one single straight line for the left lane and another for the right lane. This limitation makes turns impossible to track.

The top of the lanes are currently based on the average slope, but I am using an arbitrary height. This means that going up/down hills or making turns will make the lane tracking unreliable.


### 3. Suggest possible improvements to your pipeline

One possible improvement would be to determine the height of the lane based on the endpoint of one of the highest lines instead of an arbitrary value.

Another improvement would be to break up the lines into lane parts instead of having only one line to represent a lane. This would be a stepping stone before introducing real curves.
