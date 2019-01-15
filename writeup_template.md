# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/solidWhiteCurve_annotated.jpg "solidWhiteCurve_processed"
[image2]: ./test_images_output/solidYellowCurve_annotated.jpg "solidYellowCurve_processed"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

Below are the steps included in my pipeline :-
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
8. I observe that there are multiple lines detected for Lane, We need it be single averaged line. Also for some lane are partially recognized.
9. For Averaging and extrapolating Lines - Separate positive slope line (Right lane) and negative slope line (Left line) and average them using new draw_lane_lines function, which separates left lane lines and right lane lines where ever we see dashed lane lines. Also draw_lane_lines function convert slope and intercept into pixel points. Finally draw_lane_lines function which take those list of lines which in form of pixel points and draw averaged and extrapolated lanes on image. 

![alt text][image1]
![alt text][image2]

- Same pipeline is applied on video clips and images out put stored in output files folder for reference and my P1.ipynb file has clear info for reference.

### 2. Identify potential shortcomings with your current pipeline

As of the current test images, the images has a day light and good weather. This pipeline will definitely fails in the night lighting and also in bad weather and where ever there is huge curves on roads.

### 3. Suggest possible improvements to your pipeline

I need to test with some other road lane images in different conditions and improve algorithm based on results it show.
