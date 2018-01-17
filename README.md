# Vehicle Detection

In this project, it is aimed that cars on the road are detected

**Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier 
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run the pipeline on a video stream and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)

[image1]: ./examples/cars_noncars.png "Dataset"
[image2]: ./examples/car_hog.png "Image HOG of a Car Image"
[image3]: ./examples/noncar_hog.png "Image HOG of a Noncar Image"
[image4]: ./examples/sliding_window.png "Sliding Window Search"
[image5]: ./examples/multiple_sliding_window.png "Multiple Sliding Window Search"
[image6]: ./examples/heatmap.png "Heatmap Image"
[image7]: ./examples/heatmap_thresholded.png "Thresholded Heatmap Image"
[image8]: ./examples/advanced_sliding_window.png "Advanced Sliding Windows Search"
[image9]: ./examples/test_boxes.png "Test Image Results"

First, we need to see the dataset which we will use to train the classifier

![alt text][image1]

## HOG (Histogram of Oriented Gradients)

The code to extract HOG features from an image is defined by the method `get_hog_features` The figure below shows a comparison of a car image and its associated histogram of oriented gradients, as well as the same for a non-car image.

![alt text][image2]
![alt text][image3]

## Feature Extraction

The `extract_features` function, which is used to extract HOG Features from an array of Car and non-Car images, accepts a list of image paths and HOG parameters (as well as one of a variety of destination color spaces, to which the input image is converted), and produces a flattened array of HOG features for each image in the list. HOG parameters are tuned based upon the performance of the classifier used for training.

## Classification

Linear support vector classifier is used for classification. Data is shuffled and 20% of data is separated as test data. The accuracy of test data is not only considered but the speed of the classifier is also considered as metrics. The accuracy of test data is 98.2% and it took 1.32s. to train the classifier. According to these results, linear support vector classifier performs very well based upon the two criteria

## Sliding Window Search

The method combines HOG feature extraction with a sliding window search, but rather than perform feature extraction on each window individually which can be time consuming, the HOG features are extracted for the entire image (or a selected portion of it) and then these full-image features are subsampled according to the size of the window and then fed to the classifier. The method performs the classifier prediction on the HOG features for each window region and returns a list of rectangle objects corresponding to the windows that generated a positive ("car") prediction.

The image below shows the first attempt at using find_cars on one of the test images, using a single window size:

![alt text][image4]

In order to detect different-sized cars in the image, different windows sizes are defined. The image below shows the rectangles returned by find_cars drawn onto one of the test images in the final implementation. Notice that there are several positive predictions on each of the near-field cars

![alt text][image5]

Because a true positive is typically accompanied by several positive detections, while false positives are typically accompanied by only one or two detections, a combined heatmap and threshold is used to differentiate the two. The add_heat function increments the pixel value (referred to as "heat") of an all-black image the size of the original image at the location of each detection rectangle. Areas encompassed by more overlapping rectangles are assigned higher levels of heat. The following image is the resulting heatmap from the detections in the image above:

![alt text][image6]

A threshold is applied to the heatmap (in this example, with a value of 1), setting all pixels that don't exceed the threshold to zero. The result is below:

![alt text][image7]

The final result of the algorithm is like below:

![alt text][image8]

The results for test images from the video stream are below

![alt text][image9]

## Acknowledgment

Jeremy-shannon's work helped me a lot during the process of tuning HOG extraction and multiple window sliding search functions.
