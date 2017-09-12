# Vehicle Detection

In this project, the goal is to provide a software pipeline to identify vehicles in a video from a front-facing camera on a car.

[//]: # (Image References)
[image1]: ./images/car.png
[image2]: ./images/notcar.png
[image3]: ./images/samplecar.png
[image4]: ./images/car_ch0.png
[image5]: ./images/car_ch1.png
[image6]: ./images/car_ch2.png
[image7]: ./images/samplenotcar.png
[image8]: ./images/non_ch0.png
[image9]: ./images/non_ch1.png
[image10]: ./images/non_ch2.png
[image11]: ./images/windowsearch.png
[image12]: ./images/test1.png
[image13]: ./images/test2.png
[image14]: ./images/test3.png
[image15]: ./images/test4.png
[image16]: ./images/test5.png
[image17]: ./images/test6.png
[image18]: ./images/heat1.png
[image19]: ./images/heat2.png
[image20]: ./images/heat3.png

### Histogram of Oriented Gradients (HOG)

#### Extract HOG features from the training images

I started by reading in all the vehicle and non-vehicle images. Here are some examples of the vehicle and non-vehicle classes:

![alt text][image1] ![alt text][image2]

I then explored different color spaces and different HOG parameters (orientations, pixels_per_cell, and cells_per_block).  I grabbed random images from each of the two classes and displayed them to get a feel how a HOG output looks like.
Here is an example using the YCrCb color space and HOG parameters of orientations=9, pixels_per_cell=(16, 16) and cells_per_block=(2, 2):

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

I've trained a linear SVM classifier with default paramters. I used a random split of 20% for the test data. The classifier achieved over 99% accuracy on the test data. 

### Sliding Window Search

#### HOG Sub-sampling window search

I decided to use a HOG sub-sampling window search which is a more efficient method for doing the sliding window approach.
Each window is defined by a scaling factor where a scale of 1 would result in a window that's 8 x 8 cells then the overlap of each window is in terms of the cell distance which I set to 1. Furthermore, I use different scale values to generate multiple-scaled search windows. Here is an example of the multiple-scaled search windows. 

![alt text][image11]

#### Examples of test images  

Ultimately I searched on two scales using YCrCb 3-channel HOG features, which provided a nice result. Here are some example images:

![alt text][image12]![alt text][image13]
![alt text][image14]![alt text][image15]
![alt text][image16]![alt text][image17]

### Video Implementation

#### Test on project video

The pipeline was applied on the provided project video and the final video result was quite well. 

Here's a [link to my video result](../output_videos/vehicle_detection_output.mp4)


#### Combining overlapping bounding boxes

I recorded the positions of positive detections in each frame of the video. From the positive detections I created a heatmap and then thresholded that map to identify vehicle positions. Further, I identified individual blobs in the heatmap where I assumed each blob corresponded to a vehicle. I constructed bounding boxes to cover the area of each blob detected.  

Here are six frames and their corresponding heatmaps without threshold:

![alt text][image12]![alt text][image18]
![alt text][image13]![alt text][image19]
![alt text][image14]![alt text][image20]

### Discussion

One possible problem of the algorithm could occur in the detection of vehicles in different weather and light conditions. The used dataset is too small, so more data sources and data augmentation techniques can help to improve the classifier. Further, an ensemble of different classifiers for detection can make the pipeline more robust and improve the final accuracy. In addition to that, an exploration and combination of different color spaces and the usage of spatial binning and histogram features could be helpful but also imporove the complexity. 

By increasing the complexity, the pipeline could suffer under performance issues. I believe that state-of-the art deep learning approaches like SSD and YOLO could help.  

Another possible problem is the appearance of false positives which are caused by misclassification. In the current pipeline we are filtering out these false positives with heatmap thresholds. An more intelligent filter technique like tracking the bounding boxes over several frames could decrease the appearance of false positives and improve the overall algorithm. 

Improvements:
- Fine tune of hype parameters 
- Usage of histogram and spatial binning features
- Ensemble of different classifiers 
- More training data and data augmetation techniques
- More sophisticated filter techniques 
