##Writeup Template
###You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: ./car_image.png
[image2]: ./notcar_image.png
[image3]: ./Example_Car_hviz.png
[image4]: ./output_test1.png
[image5]: ./output_test2.jpg
[image6]: ./output_test3.png
[image7]: ./output_test4.png
[image8]: ./hmap.png

[video1]: ./project_video.mp4

## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
###Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
###Writeup / README

####1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

###Histogram of Oriented Gradients (HOG)

####1. Explain how (and identify where in your code) you extracted HOG features from the training images.

The code for this step is contained in the code cell 3 of the IPython notebook VehicleDetection.ipynb.  

I started by reading in all the `cars` and `not cars` images.  Here is an example of one of each of the `car` and `not-car` classes:

![car image][image1]
![not car image][image2]

I then explored different color spaces and different `skimage.hog()` parameters (`orientations`, `pixels_per_cell`, and `cells_per_block`).  I grabbed random images from each of the two classes and displayed them to get a feel for what the `skimage.hog()` output looks like.

Here is an example using the `YCrCb` color space and HOG parameters of `orientations=8`, `pixels_per_cell=(8, 8)` and `cells_per_block=(5, 5)`:


![Car image with Hog Visualization][image3]

####2. Explain how you settled on your final choice of HOG parameters.

I tried various combinations of parameters. This process was pretty much trial and error. I am not sure if there's a better way to do it. I just looked at the output of the accuracy of the classifier and vehicle detection accuracy.
I finally settled with the Hog parameters.

####3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

I trained a linear SVM using LinearSVC() function. The code for the classifier is in code cell 19 and lines of code are between 57 and 85

###Sliding Window Search

####1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

I decided to lower half of the search space as cars do not appear in the sky. I used a y_start_stop variable to bound the search area. I set the search area parameter to 400 and 720 pixel values. This setting is noted in code cell 19, line number 14.
The sliding window search is implemented was tested on a series of test images as noted in code cell 4, as noted in lines 10 - 20

![sliding window example][image4]

####2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

Ultimately I searched using YCrCb 3-channel HOG features and did not use spatially binned color and histograms of color in the feature vector. Initially, I thought using more featuers would give me a better result. But the classifier accuracy dropped after 
introducing the spatially binned and color histograms. I guess, the classifier was overfitting. Here are some example images:

![example 1][image5]
![example 2][image6]
![example 3][image7]
---

### Video Implementation

####1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to my video result](./output.mp4)


####2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

I recorded the positions of positive detections in each frame of the video.  From the positive detections I created a heatmap and then thresholded that map to identify vehicle positions. I experimented with multiple threshold values, but settled with a threshold value of 5.  I then used `scipy.ndimage.measurements.label()` to identify individual blobs in the heatmap.  I then assumed each blob corresponded to a vehicle.  I constructed bounding boxes to cover the area of each blob detected.  

Here's an example result showing the heatmap from a series of frames of video, the result of `scipy.ndimage.measurements.label()` and the bounding boxes then overlaid on the last frame of video:

### Here are six frames and their corresponding heatmaps:

![Heat map][image8]



---

###Discussion

####1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

I had originally guessed that hog features along with color histograms and spatially binned spaces would have performed better. Only after lot of trial error, did I just 
discover that hog features alone provide with a good accuracy. 
The pipeline might fail in areas where there are shadows.
I do not have a complete understanding of the hog parameters and how to effectively identify a good set of parameters is something I would like to improve upon. Right now, my approach seems to be very random and unscientific   

