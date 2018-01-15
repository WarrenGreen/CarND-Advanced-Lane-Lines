## Writeup 

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


---

### Writeup / README

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the first code cell of the IPython notebook located in "./examples/example.ipynb" (or in lines # through # of the file called `some_file.py`).  

I created reference points based on the  chessboard corners. I used these reference points to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function. Using these calibration coefficients, I can undistort the example imagery captured using the same cameras.

![distorted][https://github.com/WarrenGreen/CarND-Advanced-Lane-Lines/blob/master/output_images/distorted.png]
![undistorted]['output_images/undistorted.png']

### Pipeline (single images)

#### 1. Undistort imagery as shown above

#### 2. Image processing

I used a combination of S-channel color thresholding and sobel thresholding to filter image features in a binary format.

![processed]['output_images/processed.png']

#### 3. Perspective Transform

I determined `src` and `dst` points to perform a perspective transform. This transform warped the image ROI into a topdown view so that we could better fit a polynomial to the lane lines. I found that applying the perspective transform prior to the image processing caused the processing to be less effective. I suspect this to be the case since previously sharp edges become blury edges after the transform. This especially would effect the sobel transform.

![topdown]['output_images/topdown.png']

#### 4. Sliding box and line fit

Line fitting occured by splitting the images in half so that we could process the left and right lanes individually. By analyzing the the histogram the image ROI, where the image is the processed binary image, we could detect a starting point for the lane. We could have then moved up by the ROI height and searched the entire next row but we know that the next row must be within some margin of the previous detection to be a valid lane. Knowing this, we can constrain our next row search. 

![top_down_processed]['output_images/top_down_processed.png']


#### 5. Radius and lane offset

At the end of the `sliding_fit` function, I calculate the radius and lane offset of the turn/vehicle in meters. The radius is done by redefining the polynomial fit in terms of meters and using the equation supplied in the course lesson to estimate the radius per lane line. The average of the two lane lines is returned. The offset is simply finding the center of the image and assuming that the camera is perfectly aligned with the car. Then finding the center between the two estimated lane lines and finding the difference. Before returning this value, we convert this pixel value to meters with our pixel to meter ratio.


#### 6.



![final]['output_images/final.png']

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result]('./project_video_solved.avi')

