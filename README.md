## Advanced Lane Finding


In this project, my goal is to write a software pipeline to identify the lane boundaries in a video, but the main output or product is a detailed writeup of the project. You can find my write up in this repository. 

The Project
---

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

The images for camera calibration are stored in the folder called `camera_cal`.  The images in `test_images` are for testing your pipeline on single frames.  If you want to extract more test images from the videos, you can simply use an image writing method like `cv2.imwrite()`, i.e., you can read the video in frame by frame as usual, and for frames you want to save for later you can write to an image file.  

The video called `project_video.mp4` is the video my pipeline should work well on.  

The `challenge_video.mp4` video is an extra challenge.  The `harder_challenge.mp4` video is another optional challenge and is brutal! The current code doesn't work well with these two extra challenges. I will come back to them later.

## Installation
You might want to git clone this repository and install all the required packages using pip or anaconda. You can find all the package names in the jupyter notebook. Actually, all the codes are contained in this jupyter notebook.
### Package Names
* Numpy
* Opencv
* matplotlib
* glob
* Moverpi
* Ipython
## Usage
Run the jupyter notebook after you setup the environment.
