# **Finding Lane Lines on the Road** 
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

<img src="examples/laneLines_thirdPass.jpg" width="480" alt="Combined Image" />

Overview
---

When we drive, we use our eyes to decide where to go.  The lines on the road that show us where the lanes are act as our constant reference for where to steer the vehicle.  Naturally, one of the first things we would like to do in developing a self-driving car is to automatically detect lane lines using an algorithm.

In this project you will detect lane lines in images using Python and OpenCV.  OpenCV means "Open-Source Computer Vision", which is a package that has many useful tools for analyzing images.  

To complete the project, two files will be submitted: a file containing project code and a file containing a brief write up explaining your solution. We have included template files to be used both for the [code](https://github.com/udacity/CarND-LaneLines-P1/blob/master/P1.ipynb) and the [writeup](https://github.com/udacity/CarND-LaneLines-P1/blob/master/writeup_template.md).The code file is called P1.ipynb and the writeup template is writeup_template.md 

To meet specifications in the project, take a look at the requirements in the [project rubric](https://review.udacity.com/#!/rubrics/322/view)


[//]: # (Image References)

[image1]: ./test_images_output/solidWhiteCurve_annotated.jpg "solidWhiteCurve_processed"
[image2]: ./test_images_output/solidYellowCurve_annotated.jpg "solidYellowCurve_processed"

### 1. Below are the steps included in my pipeline :-
1. Identify the lane line color(can be yellow or white). So that based on the color(if yellow) we apply HSV(Hue Saturation value). HSV actually separates each color from other colors. 
2. The given image has to be converted into grayscale(which has only 1 channel). Grayscale images are faster to do processing.
3. Apply Gaussian Blur to reduce noise and make that image smoothie. Gaussian Blur takes one more required parameter called Kernel. The higher the kernel value the greater the blur effect on image. Kernel value only accept odd number(eg. 1, 3, 5, 7).
4. Apply Canny Edge detection algorithm so that we can detect edges of both the lanes and when the pixel value changes rapidly then only we will be able to find the strong edges. Canny algorithm takes two extra parameters called low_threshold and high_threshold in a ratio of 1:2 or 1:3.
5. Now lets get only the edges in Region of Interest(ROI). We will draw a ROI around the lanes from the bottom of the image to the little more mid of the image.
6. Based on the edges we got from ROI, we apply Hough Transform line detection algorithm to detect line in edges images. It has several parameters to tune 
    - rho – Distance resolution of the accumulator in pixels.
    - theta – Angle resolution of the accumulator in radians.
    - threshold – Accumulator threshold parameter. Only those lines are returned that get enough votes (> `threshold`).
    - minLineLength – Minimum line length. Line segments shorter than that are rejected.
    - maxLineGap – Maximum allowed gap between points on the same line to link them.
7. Output of Hough Transform line algorithm is list of lines and next step is to Draw line on image
8.  I observe that there are multiple lines detected for Lane, We need it be single averaged line. Also for some lane are partially recognized.
9.  For Averaging and extrapolating Lines - Separate positive slope line (Right lane) and negative slope line (Left line) and average them using new draw_lane_lines function, which separates left lane lines and right lane lines where ever we see dashed lane lines. Also draw_lane_lines function convert slope and intercept into pixel points. Finally draw_lane_lines function which take those list of lines which in form of pixel points and draw averaged and extrapolated lanes on image. 

![alt text][image1]
![alt text][image2]

- Same pipeline is applied on video clips and images out put stored in output files folder for reference and my p1.ipynb file has clear info for reference.

### 2. Identify potential shortcomings with your current pipeline

As of the current test images, the images has a day light and good weather. This pipeline will definitely fails in the night lighting and also in bad weather and where ever there is huge curves on roads.

### 3. Suggest possible improvements to your pipeline

I need to test with some other road lane images in different conditions and improve algorithm based on results it show.
