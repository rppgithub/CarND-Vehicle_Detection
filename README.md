#  CarND-Vehicle_Detection
## Vehicle Detection Project

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector.
  Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a   heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.
## Rubric Points

Here I will consider the rubric points individually and describe how I addressed each point in my implementation.


# Histogram of Oriented Gradients (HOG)

1. Explain how (and identify where in your code) you extracted HOG features from the training images.

  The code for this step is contained in the Cell $ 6 of the IPython notebook Vehicle_Detection.

  I started by reading in all the vehicle and non-vehicle images. Here is an example of one of each of the vehicle and non- vehicle classes:

  alt text

  I then explored different color spaces and different skimage.hog() parameters (orientations, pixels_per_cell, and   cells_per_block). I grabbed random images from each of the two classes and displayed them to get a feel for what the skimage.hog() output looks like.

 Here is an example using the YUV  color space and HOG parameters of orientations=8, pixels_per_cell=(8, 8) and cells_per_block=(2, 2):

alt text

2. Explain how you settled on your final choice of HOG parameters.

   I tried various combinations of parameters (RGB, HSV, LUV, YCrCb ) with different orientations and finally settled on
   
   
     |Parameter      |Value|
     |---------------|-----|
     |color_space    |YUV  |
     |orientations   |8    |
     |pixels_per_cell|8    |
   

3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

   I do this in Cell 10. Used LinearSVM using only the parameters above since it gave me fairly good results. This was       arrived at after a great deal of experimentation. I chose to not use the spatial or color histogram features.
   
   ```
   Using: 8 orientations 8 pixels per cell and 2 cells per block
   Feature vector length: 4704
   21.35 Seconds to train SVC...
   Test Accuracy of SVC =  0.9882
   My SVC predicts:  [ 1.  1.  1.  1.  1.  0.  0.  0.  1.  0.]
   For these 10 labels:  [ 1.  1.  1.  1.  1.  0.  0.  0.  1.  0.]
   0.00545 Seconds to predict 10 labels with SVC
   ```

## Sliding Window Search

1. Describe how (and identify where in your code) you implemented a sliding window search. How did you decide what scales to search and how much to overlap windows?

  This is done in Cell #20.

alt text

2. Show some examples of test images to demonstrate how your pipeline is working. What did you do to optimize the performance of your classifier?

   Ultimately I searched on two scales using YCrCb 3-channel HOG features plus spatially binned color and histograms of color in the feature vector, which provided a nice result. Here are some example images:

alt text

Video Implementation

1. Provide a link to your final video output. Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)

Here's a link to my video result

2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

   I recorded the positions of positive detections in each frame of the video. From the positive detections I created a heatmap and then thresholded that map to identify vehicle positions. I then used scipy.ndimage.measurements.label() to identify individual blobs in the heatmap. I then assumed each blob corresponded to a vehicle. I constructed bounding boxes to cover the area of each blob detected.

   Here's an example result showing the heatmap from a series of frames of video, the result of scipy.ndimage.measurements.label() and the bounding boxes then overlaid on the last frame of video:

  Here are six frames and their corresponding heatmaps:

alt text

  Here is the output of scipy.ndimage.measurements.label() on the integrated heatmap from all six frames:

alt text

  Here the resulting bounding boxes are drawn onto the last frame in the series:

alt text

## Discussion

1. Briefly discuss any problems / issues you faced in your implementation of this project. Where will your pipeline likely fail? What could you do to make it more robust?

  Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.
