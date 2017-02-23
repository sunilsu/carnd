**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images/solidYellowLeft.jpg "SolidYellowLeft"
[image2]: ./test_images/gray.jpg "gray"
[image3]: ./test_images/solidYellowLeft_blur.jpg "blur"
[image4]: ./test_images/solidYellowLeft_edges.jpg "edges"
[image5]: ./test_images/solidYellowLeft_masked_edges.jpg "masked_edges"
[image6]: ./test_images/solidYellowLeft_lines.jpg "hough lines"
[image7]: ./test_images/pr_solidYellowLeft.jpg "hough lines"

---

### Reflection

My pipeline consisted of 6 steps. Illustrating the steps on the image.

![image][image1]

1. Convert image to grayscale image.
![Grayscale][image2]
2. Blur the image with a gaussian kernel of size 11
![Blur][image3]
3. Run Canny edge detection with threshold of (40, 80)
![Edge][image4]
4. Define a region of interest and keep only the region inside the polygon
![Masked Image][image5]
5. Calculate the Hough lines from the masked image, detect the left and right lines, draw a best fit line and extrapolate the line to start from the bottom of the image till a particular height.
![HoughL][image6]
6. Overlay the lines on the original image and return
![Processed][image7]
7. Also implemented a mask based on colors yellow and white in the image. This show identify the lanes better. I still need to tune rest of the pipeline to use this mask.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by
* Separated the lines into **left** and **right** lists depending on whether the slope of the line is positive or negative.
* Filtered the lists to make sure the slopes were at least 0.3. This was to avoid lines that are close to horizontal. I also tried to filter the outliers based on slope percentiles, but it did not improve overall result and did not use in the end.
* Assigned weights to points depending on the length of the line. Longer lines were given more weight.
* Fit a least squares line to the set of points in the right and left lists.
* Extended the line to start from bottom of the image and reaching upto a particular height.


### Identify potential shortcomings with your current pipeline


1. One potential shortcoming would be when the lanes curve at greater angles than what is the case with the test images. The lanes are extrapolated to a fixed height and this can result in crossing over to the adjacent lane.
2. Another shortcoming is when there are cars in the adjacent lanes close to our car, they can fall within the region of interest and interfere with lines calculated from canny edge detection and hough lines. This can throw off the lines in a very different angle.
3. Another shortcoming is in the way the mask for region of interest is calculated. Its based on best polynomial that satified the conditions in the test images. But this can be wrong shape for other scenarios and angles.


### Suggest possible improvements to your pipeline

1. A possible improvement would be to fit a 2nd degree curve to the lane instead of line.
2. Another potential improvement could be to dynamically calculate the mask based on lane shapes. If the lanes are curving the shape of the mask will be different that when the lanes are straight to the horizon.
3. I tried to create a mask based on yellow/white colors of the lanes. This was done by changing color space to HSV and filtering colors in range for those colors. However, I have not been able to use this mask to solve the challenge and will need some feedback / more time to move forward on the challenge.
