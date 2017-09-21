## Writeup Template

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
[image2]: ./examples/original_image.png "Original Road Image"
[image3]: ./examples/undistored_image.png "Road Transformed"
[image4]: ./examples/color_binary.png "Binary Example"
[image5]: ./examples/warped_straight_lines.png "Warp Example"
[image6]: ./examples/warped_straight_lines_next.png "Warp Example Next"
[image7]: ./examples/color_fit_lines.png "Fit Visual"
[image8]: ./examples/example_output.png "Output"
[video1]: ./project_video.mp4 "Video"
[image9]: ./examples/original_chessboard.png "Output"


### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the second code cell and third code cell of the IPython notebook located in "./CarND-Advanced-Lane-Lines/Advanced_Lane_Detection_Notebook.ipynb".  

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 
![alt text][image9]
![alt text][image1]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][image2]
![alt text][image3]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and gradient thresholds to generate a binary image. You can find the code in the Color/Gradient Threshold code block in the jupyter notebook.  Here's an example of my output for this step.

![alt text][image4]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `pers_trans()`, which appears in the "Perspective Transform" code cell of the IPython notebook.  The `pers_trans()` function takes as inputs an image (`img`), as well as source (`src`) and destination (`dst`) points.  I chose the hardcode the source and destination points in the following manner:

```python
x1 = 215
y1 = 680
x2 = 590
y2 = 450
x3 = 690
y3 = 450
x4 = 1080
y4 = 680
src = np.float32([(x1,y1), (x2,y2), (x3,y3), (x4,y4)])

dst = np.float32([[300, img_size[1]],[300, 0], [900, 0], [900, img_size[1]]])
```

This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 215, 680      | 300, 720        | 
| 590, 450      | 300, 0      |
| 690, 450     | 900, 700      |
| 1080, 680      | 900, 720        |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image5]
![alt text][image6]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Then I did some other stuff and fit my lane lines with a 2nd order polynomial kinda like this:

![alt text][image7]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I used the techniques described in the [Udacity tutorial](https://classroom.udacity.com/nanodegrees/nd013/parts/fbf77062-5703-404e-b60c-95b78b2f3f9e/modules/2b62a1c3-e151-4a0e-b6b6-e424fa46ceab/lessons/40ec78ee-fb7c-4b53-94a8-028c5c60b858/concepts/2f928913-21f6-4611-9055-01744acc344f). I did this in Video Processing code block in the ipython notebook.

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in Video Processing code block in the ipython notebook in function `process_image()`.  Here is an example of my result on a test image:

![alt text][image8]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video_processed.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further. I finished the whole project by usign the methods mentioned in the Udacity Lectures. There are the problems I found:
* The line detection part cannot recognized when there are several lines next to the real lane boundaries. (Challenge Video) 
* The threshold part is not robust when the contast is not clear.(Harder Challenge Video) 
* When the boundary is missing or is blocked by other vehicles, the pipeline likely fails. (Harder Challenge Video)
To solve these problems, I think it would be nice to:
* Teach the program to identify the lines which are the actual lane boundaries using classification in machine learning. 
* use a anti-contrast or light-condition filtering method to preprocess the image.
* Do more computer vision to filter and track the moving object in the image and recover from bad boundary prediction. In order to identify the bad boundary prediction, I believe a classification algorithm is needed. Therefore, the problem will either give up predicting the lane boundaries or give some inference of the current lane curvature based on the previous good prediction and the current image. 
