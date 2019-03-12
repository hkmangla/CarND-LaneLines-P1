# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/gray.jpg "Grayscale"
[image2]: ./examples/smoothed.jpg "Smoothed"
[image3]: ./examples/edge_image.jpg "Edges"
[image4]: ./examples/masked_image.jpg "Masked"
[image5]: ./examples/line_segments.jpg "Line segments"
[image6]: ./examples/lane_lines.jpg "Lane lines"
---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps.

Step 1: First I converted the image to grayscale.

![Grayscale Image][image1]

Step 2: In this step, I smoothed the grayscaled image using gaussian filter. Reason of using guassing filter is 'unsharp masking' which makes edge detection easier and more accurate.

![Smoothed Image][image2]

Step 3: Then I used canny edge detection method to detect edges from the image. The canny function use gradient to detect edges.

![Edge Image][image3]

Step 4: In this step, I masked the region of interest from the canny edges. This step filter out the edges of lane lines from all other unwanted edges of the image.

![Masked Image][image4]

Step 5: This is the most important step of my pipeline. In this step, I detect line segments of the lane lines from the edges using hough line transform. Then I draw those line segments on my initial image. I need to tuned the few parameters to get the desired result in this step.

![Line segment Image][image5]

In order to draw a single line on the left and right lanes, I modified the draw_lines() function. The modification of draw line function take following steps
- First I separated the hough line segments into left lane line segments and right lane line segments. To do so, I used the slope of the segments. Line segments which has negative slopes contribute to left lane line, and the positive slopes segments contribute to right lane line.
- Then I fit the lane line using their contributing line segments. But during the line fitting I faced the problem of the outliers. This may be because of some line segment on the left lane has positive slopes, hence contributing to right lane and vice versa.
- To remove the outlier line segments, I take the average of all line segments on a lane and remove the line segments which are far away from that average.
- Then I draw the lane lines on the image.

Here is the final output of my pipeline,

![Line segment Image][image6]


### 2. Identify potential shortcomings with your current pipeline


1. One potential shortcoming would be what would happen when the lane lines are curved. I used hough transform which detect only straight lines, but for curved line it may give unexpected results.

2. I used hit and trial to get the hyperparameters correct which is working for this case but may fail some of the cases, and another thing is region of interest is hardcoded which means I assumed camera position do not change.

3. My code can also fail if there is the shadows on the road. Shadows can cause uneven contrast which make chances of detection of false line segment very high.

### 3. Suggest possible improvements to your pipeline

1. A possible improvement would be we can darken the grayscale image to remove the contrast problem.

2. Another potential improvement could be to using higher degree polynomial instead of line to accurately detect the curved lane lines.
