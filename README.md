
# Finding Lane Lines on the Road

[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

<img src="laneLines_thirdPass.jpg" width="480" alt="Combined Image" />

Overview
---

When we drive, we use our eyes to decide where to go.  The lines on the road that show us where the lanes are act as our constant reference for where to steer the vehicle.  Naturally, one of the first things we would like to do in developing a self-driving car is to automatically detect lane lines using an algorithm.

The goal of this project is to detect lane lines in images using Python and OpenCV.  OpenCV means "Open-Source Computer Vision", which is a package that has many useful tools for analyzing images.

[image1]: ./examples/white.png
[image2]: ./examples/yellow.png 
[image3]: ./examples/gray.png
[image4]: ./examples/gray_blur.png
[image5]: ./examples/canny.png
[image6]: ./examples/region.png
[image7]: ./examples/hough.png
[image8]: ./examples/result.png




## Reflection

### My pipeline that finds lane lines on the road
1. Extracts white or yellow colors in some range and then merge into a grayscale image.

    * ![alt text][image1] ![alt text][image2]
    * When I converted the image only to grayscale, the challenge.mp4 case don't work well. The yellow lane in the video are not distinguished well in the bright road, so I extracted the yellow color also.
    * And then bitewise_or them

    * ![][image3]

2. Blurring

    * ![][image4]

3. Canny edge dection

    * ![][image5]

4. Region masking

    * ![][image6]

5. Hough line detection

    * ![][image7]

* Result
    * ![][image8]

## Making a single lane
I made some assumptions in this project.
* The colors of the lane are yellow or white.
* The bottom end of lanes are within the width of image.
* The horizontal lanes should be ignored but the vertical lanes are possible.
* The lines in curve should not exceed the vertical center line of the region of interest.



In order to make a single lane per each side, I first tried to use **the slope** for the criteria to seperate lines.
For the vertical lanes, I calculated the x/y slope, *instead of y/x*, but in case of the slope of 0, it's side is not determined.

*I wanted to consider the cases that the lanes look vertical or lanes get broader so the slope of the each side can get opposite.*

So I decided to use **the vertical center line** of the region of interest. The lines in the left side of the center are included in the left lane.
I calculated both the average of the slope and the regression of the line points to decide which is better.
When I used the slope criteria, if the error lines were in the opposite side then the regression was not good.
But when I used the center line criteria, the regression looked better than the simple average of slope.


## Shortcomings with the current pipeline

My pipeline's shortcoming comes from my assumptions.
* If the colors of lane is not yellow or white (because of something like the light effect), 
* If the lanes' width are outside of the image width, it's not able to detect the lanes.
* If the lines in curve are over the vertical center line of the region of interest, 
the height of the region of interest should be reduced according to the curve.


## Possible improvements
* Auto resizing of the height of the region of interest so the curve lane are not to exceed the center of the region.
* Tuning of the Color range.
* Tring to exclude outliers before regression, like RANSAC.
