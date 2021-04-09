## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images (Done)
* Apply a distortion correction to raw images (Done)
* Use color transforms, gradients, etc., to create a thresholded binary image.(color filter applied) (Done)
* Apply a perspective transform to rectify binary image ("birds-eye view").(Done)
* Detect lane pixels and fit to find the lane boundary. (Done)
* Determine the curvature of the lane and vehicle position with respect to center.(Curvature is very big!!!)
* Warp the detected lane boundaries back onto the original image.(Done)
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.(lane curvature should be estimated)

[//]: # (Image References)

[image1]: ./examples/undistort_output.png "Undistorted"
[image2]: ./test_images/test1.jpg "Road Transformed"
[image3]: ./examples/binary_combo_example.jpg "Binary Example"
[image4]: ./examples/warped_straight_lines.jpg "Warp Example"
[image5]: ./examples/color_fit_lines.jpg "Fit Visual"
[image6]: ./examples/example_output.jpg "Output"
[video1]: ./project_video.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  




---

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image( TODO)

The code for this step is contained in the section "Step1. Pipeline for distortion elimination" of the IPython notebook located in the root directory  "./examples/P2_Advanced_LL_Detection.ipynb".
The methods for the camera calibration are located in Distortion.py library

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][https://github.com/nygilmanov/testRep/blob/main/writeup_images/undistorted.png]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][image2]


#### 2. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `warper()`, which appears in lines 1 through 8 in the file `example.py` 

Number of function have been developed todo the perspective transform

- save_PERSP_to_pickle_file
- load_PERSP_from_pickle_file
- generate_perspective_pipeline - builds trapezoid on straight lines using by using methods developed in the previous project

    For these purposes separate library have been developed and all its methods are locaed in DrawLines.py library
    Once we get trapezoid on the original image we define 4 desired points.
    
    Finally we get perspactive matrix and the matrix for the opposite transformation by using  cv2.getPerspectiveTransform method
    
    These matrixes are saved in pickle file and will are used in the later stages of the project.


get_unwarped_image - gets the original image and returns warped image 

We can see the result of transforamation of the original image to to the warped one below


(output_images/examples/example.py) (or, for example, in the 3rd code cell of the IPython notebook).  The `warper()` function takes as inputs an image (`img`), as well as source (`src`) and destination (`dst`) points.  I chose the hardcode the source and destination points in the following manner:



This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 585, 460      | 320, 0        | 
| 203, 720      | 320, 720      |
| 1127, 720     | 960, 720      |
| 695, 460      | 960, 0        |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

TODO

![alt text][image4]


#### 3. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and gradient thresholds to generate a binary image (thresholding steps at section TRANSFORMS of the main notebook).


Sobel gradient by x and y axis with various parameters have been tested

Final combined image was defined assuming varuis aspects 
such as

Gradient by x axis
Magnitude (TODO)
Directions of the gradient

as well as colorspace thresholds 

- in particulare saturation and red channel


Final combination of all aspects

combined[(gradx == 1) | ((mag_binary == 1) & (dir_binary == 1)) | (sbinary ==1 ) | (rbinary == 1)] = 1
   
Give the following result



Here's an example of my output for this step.  (note: this is not actually from one of the test images)

TODO




![alt text][image3]




#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Then I did some other stuff and fit my lane lines with a 2nd order polynomial kinda like this:

I have built the histogram based on the warped image to identify the starting  positions of the left and right lane lines on the x axis

TODO  After that a special algorithm makes number of steps by moving rectangle nnext and 



![alt text][image5]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in lines # through # in my code in `my_other_file.py`

TODO



#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in lines # through # in my code in `yet_another_file.py` in the function `map_lane()`.  Here is an example of my result on a test image:

![alt text][image6]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  
