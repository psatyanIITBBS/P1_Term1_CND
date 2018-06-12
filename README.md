# **Finding Lane Lines on the Road** 
## Self Driving Car Engineer Nanodegree Program - Term 01
### Project 1 - Car Lane Detection

**Finding Lane Lines on the Road**

The first step in developing a Self Driving Car is to be able to make the car sense its position with respect to the road. Therefore, it is very important to get this information with the least expensive piece of equipment - a cammera. So, the goals / steps of this project are the following:

* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

The idea is to prepare a pipeline for getting the laned identified and annotated as shown in the picture below.

<img src="examples/laneLines_thirdPass.jpg" width="480" alt="Combined Image" />

### Reflection

### 1. Description of the pipeline. 

My pipeline consisted of the following seven (7) steps. 

1. First, the image (i.e., each frame from the video) is first converted into grayscale.
2. Then a Gaussian blurring is carried out.
3. The blurred image then is passed through an edge detection filter.
4. The image is masked to keep only that portion of the picture, where the probabity of finding the road is maximum.
5. The masked image then is taken through a Hough transformation filter so that the staight edges of the lanes on the roads will be captured.
6. The broken lanes are then averaged out and extrapolated to get a continuous line representing the lane.
7. a Kalman filter is used to compensate for the uncertainty in the detection technique.

### 2. Averaging/Extrapolation of Lane markers
In order to draw a single line on the left and right lanes, the draw_lines() function has been modified as follows:

1. The left and right lanes are assumed to have negative and positive slopes with respect to the camera.
2. The output of the Hough filter is expected to be in multiple pieces of line segments. 
3. These line segments are grouped according to their slopes to be part of left or right lane.
4. The extreme end points of each group are found out.
5. from the two extreme points on each group (they may not be on the border of our region of interest) the slope of the line is estimated. 
6. Using the point on the outer extreme, and the slope, the equation of the line is formulated (point-slope formula).
7. with this equation the whole line is drawn on both the sides.

### 3. Detection of Lane Segments has Uncertanties

The detection of straight edges through Hough transform will induce some uncertainties because of the variations in the photograph conditions such as lighting, shadow, vibrations etc. This makes the calculations of the slopes and the end points fluctuate within a certain zone. In order to avoid this noise, a Kalman filter has been used to smoothen out the fluctuations in the slope and end point estimation.

### 4. Identify Potential Shortcomings with the Current Pipeline

One potential shortcoming would be what would happen when the lighting condition renders the Canny edge detection filter completely useles (as in the case of the challenge videos). The above pipeline completely fails to identify the lanes under those conditions. 

Another shortcoming could be the inability to measure the curvature  of the road. Present pipeline will fail under highly curved roads.

Another limitation of the present filter is that it cannot (probably, I have not checked) detect lanes in the night.

One more limitation: my Kalman filter hardly is making any impact in reducing the jerks that are observed in the estimated lane marking lines.


### 5. Suggestion for Possible Improvements in the Pipeline

A definite possible improvement would be to set the Kalman Filter so that the annoying fluctuations can be removed. 

Another potential improvement could be to use the perspective transform for taking care of the curvature of the road.

