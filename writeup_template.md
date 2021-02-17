## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./examples/undistort_output.png "Undistorted"
[image2]: ./test_images/test3.jpg "Road Transformed"
[image3]: ./examples/binary_combo_example.png "Binary Example"
[image4]: ./examples/warped_straight_lines.png "Warp Example"
[image5]: ./examples/color_fit_lines.png "Fit Visual"
[image6]: ./examples/example_output.png "Output"
[video1]: ./project_video_out.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the function `cal_chessBoard()` (line10) & `cal_undistort begins()`(line41) in 1st code cell of the Jupyter notebook located in "./advanced_lane_findinge.ipynb".  

`objp` is the object points inn real world, and here is asumming z=0 for all points. 
all `objp` are appended into `objpoints` for all chessboard images.
`imgpoints` are the coners points detected with all chessboard imagess.
`objpoints` and `imgpoints` are the output of `cal_chessBoard()`.

`cal_undistort begins()` uses `objpoints` and `imgpoints` to calculate distortion matrix.
undistort result as below:

![alt text][image1]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][image2]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and gradient thresholds to generate a binary image (thresholding steps at lines 64 through 73 in cell1 of `edge_detection()`).  Here's an example of my output for this step.  (note: this is not actually from one of the test images)

![alt text][image3]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `corners_unwarp()`, which appears in lines 47 through 51 in `corners_unwarp()` in cell1 in the Jupyter notebook.  The `corners_unwarp()` function takes as inputs an image (`img`), as well as source (`src`) and destination (`dst`) points.  

src and dst points are chosed manually as below:
src = np.float32([[550,450],[730,450],[0,720],[1280,720]])
dst = np.float32([[-80,0],[1280,0],[140,720],[1060,720]]) 

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image4]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?
`fit_polynomial()`(from line173) to use sliding widow method and `search_around_poly()`(from line217) to use serching around fit line method to define the current fit curve lines.
the fitted lines are drawed onto the warped images as below:

![alt text][image5]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

`measure_curvature_real()` (from line 238 )to calculate the curvature of the both lane lines at the bottom of the image frame.

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

`draw_lanes()` (from line250) is to the detected lane lines and lane lane and warp back into the original image and showed as below:

![alt text][image6]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video_out.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Although I did some filtering to prevent too much noise to affect the precision of the lane lines detected, but still some situations such as at the BRIDGE (the color is obviously different from normal high way sufaces.) there will be fluctuations of the lane lines there.
maybe it can be improved by tuning the threshold and other position parameters and filtering parameters, or to use some other algorism.
