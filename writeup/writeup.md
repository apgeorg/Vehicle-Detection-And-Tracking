# Vehicle Detection

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: ./images/car.png
[image2]: ./images/notcar.png
[image3]: ./images/samplecar.png
[image4]: ./images/y.png
[image5]: ./images/u.png
[image6]: ./images/v.png

[image7]: ./images/samplenotcar.png
[image8]: ./images/nonY.png
[image9]: ./images/nonU.png
[image10]: ./images/nonV.png
[image11]: ./images/windowsearch.png
[video1]: ./project_video.mp4

### Histogram of Oriented Gradients (HOG)

#### Extract HOG features from the training images

I started by reading in all the vehicle and non-vehicle images. Here are some examples of the vehicle and non-vehicle classes:

![alt text][image1] ![alt text][image2]

I then explored different color spaces and different HOG parameters (orientations, pixels_per_cell, and cells_per_block).  I grabbed random images from each of the two classes and displayed them to get a feel how a HOG output looks like.
Here is an example using the YUV color space and HOG parameters of orientations=9, pixels_per_cell=(16, 16) and cells_per_block=(2, 2):

Sample images of the two classes:

![alt text][image3]![alt text][image7]

HOG output for vehicle sample image:

![alt text][image4]![alt text][image5]![alt text][image6]

HOG output for non-vehicle sample image:

![alt text][image8]![alt text][image9]![alt text][image10]

#### Final choice of HOG parameters

I've tried multiple combinations of parameters. So the final parameters are based on emperical results. 
Ideally, I would have used a grid search to test all different combinations including colour spaces. The best result obtained so far was using orientations=9, pixels_per_cell=(8, 8) and cells_per_block=(2, 2).

#### Train a classifier

I've trained a linear SVM classifier with default paramters. I used a random split of 20% for the test data. The classifier achieved an 99% accuracy on the test data. 

### Sliding Window Search

#### 1. Hog Sub-sampling Window Search

I decided to use a HOG sub-sampling window search which is a more efficient method for doing the sliding window approach.
Each window is defined by a scaling factor where a scale of 1 would result in a window that's 8 x 8 cells then the overlap of each window is in terms of the cell distance. Furthermore I use different scale values to generate multiple-scaled search windows. Here is an example of the multiple-scaled search windows. 

![alt text][image3]

#### 2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

Ultimately I searched on two scales using YCrCb 3-channel HOG features plus spatially binned color and histograms of color in the feature vector, which provided a nice result.  Here are some example images:

![alt text][image4]
---

### Video Implementation

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to my video result](./project_video.mp4)


#### 2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

I recorded the positions of positive detections in each frame of the video.  From the positive detections I created a heatmap and then thresholded that map to identify vehicle positions.  I then used `scipy.ndimage.measurements.label()` to identify individual blobs in the heatmap.  I then assumed each blob corresponded to a vehicle.  I constructed bounding boxes to cover the area of each blob detected.  

Here's an example result showing the heatmap from a series of frames of video, the result of `scipy.ndimage.measurements.label()` and the bounding boxes then overlaid on the last frame of video:

### Here are six frames and their corresponding heatmaps:

![alt text][image5]

### Here is the output of `scipy.ndimage.measurements.label()` on the integrated heatmap from all six frames:
![alt text][image6]

### Here the resulting bounding boxes are drawn onto the last frame in the series:
![alt text][image7]



---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  

