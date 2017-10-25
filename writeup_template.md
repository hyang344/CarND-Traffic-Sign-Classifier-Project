#**Traffic Sign Recognition** 

##Writeup Template

###You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Build a Traffic Sign Recognition Project**

The goals / steps of this project are the following:
* Load the data set (see below for links to the project data set)
* Explore, summarize and visualize the data set
* Design, train and test a model architecture
* Use the model to make predictions on new images
* Analyze the softmax probabilities of the new images
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./beforeBar.jpg
[image2]: ./afterBar.jpg
[image3]: ./allFiveSigns.jpg

## Rubric Points
###Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/481/view) individually and describe how I addressed each point in my implementation.  

---
###Writeup / README

####1. Provide a Writeup / README that includes all the rubric points and how you addressed each one. You can submit your writeup as markdown or pdf. You can use this template as a guide for writing the report. The submission includes the project code.

You're reading it! and my project can be found at the same directory as this file (The name will also be Traffic_Sign_Classifier.ipynb )

###Data Set Summary & Exploration

####1. Provide a basic summary of the data set. In the code, the analysis should be done using python, numpy and/or pandas methods rather than hardcoding results manually.

I used the pandas library to calculate summary statistics of the traffic
signs data set:

* The size of training set is 34799
* The size of the validation set is 4410
* The size of test set is 12630
* The shape of a traffic sign image is (32,32,3)
* The number of unique classes/labels in the data set is 43

####2. Include an exploratory visualization of the dataset.

Here is an exploratory visualization of the data set. It is a bar chart showing how the data are populated

![alt text][image1]

###Design and Test a Model Architecture

####1. Describe how you preprocessed the image data. What techniques were chosen and why did you choose these techniques? Consider including images showing the output of each preprocessing technique. Pre-processing refers to techniques such as converting to grayscale, normalization, etc. (OPTIONAL: As described in the "Stand Out Suggestions" part of the rubric, if you generated additional data for training, describe why you decided to generate additional data, how you generated the data, and provide example images of the additional data. Then describe the characteristics of the augmented training set like number of images in the set, number of images for each class, etc.)

I normalized the image data because it helps the net to understand what is more important in the photo, which helps learning.

I decided to generate additional data because I found that there are extremely unequal data amount in different classes, and that can inhibit the convnet from learning all 43 labels equally.

To add more data to the the data set, I first calculate the average data in each class, and I use a variable scale_factor to represent how far away they are from average, and I create a factor of the scale_factor more data than the original one. It turns out to be pretty helpful as I made all the data amount similar.

![alt text][image2]

Although I do create quite a few duplicate data, I did not augment them (like rotate, blur etc). This will definitely increase validation accuracy.


####2. Describe what your final model architecture looks like including model type, layers, layer sizes, connectivity, etc.) Consider including a diagram and/or table describing the final model.

My final model consisted of the following layers:

| Layer         		|     Description	        					| 
|:---------------------:|:---------------------------------------------:| 
| Input         		| 32x32x3 RGB image   							| 
| Convolution 5x5     	| 1x1 stride, same padding, outputs 28x28x16 	|
| RELU					|												|
| Max pooling	      	| 2x2 stride,  valid, outputs 14x14x16 				|
| Convolution 3x3	    | 1x1 stride, same padding, outputs 10x10x48 	|
| RELU					|												|
| Dropout | keep probability = 0.6 |
| Max pooling	      	| 2x2 stride,  valid, outputs 5x5x48 				|
| Flatten | outputs 1200 |
| Fully connected		| output 400     |
| RELU					|												|
| Dropout | keep probability = 0.6 |
| Fully connected		| output 43     |
 


####3. Describe how you trained your model. The discussion can include the type of optimizer, the batch size, number of epochs and any hyperparameters such as learning rate.

To train the model, I used an Adam optimizer, batch size of 128, 10 epochs and 0.001 learning rate.

####4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

My final model results were:
* validation set accuracy of 0.939
* test set accuracy of 1.000 (For a set of 5 images)

If an iterative approach was chosen:
* What was the first architecture that was tried and why was it chosen?
I tried LeNet because it's the one we learned in class and we actually implemented in the lab.
* What were some problems with the initial architecture?
The accuracy is not good enough (89%).
* How was the architecture adjusted and why was it adjusted? Typical adjustments could include choosing a different model architecture, adding or taking away layers (pooling, dropout, convolution, etc), using an activation function or changing the activation function. One common justification for adjusting an architecture would be due to overfitting or underfitting. A high accuracy on the training set but low accuracy on the validation set indicates over fitting; a low accuracy on both sets indicates under fitting.
I added a few dropout and pooling layers because I once read about Resnet and it inspires me that sometimes dropping data or picking the most significant data can increase accuracy and it helps the whole process run faster, so I implemented it. I also incremented the depth of the network and it kind of helped the whole accuracy.
* Which parameters were tuned? How were they adjusted and why?
The depth was increase because I believe it increments my accuracy.
* What are some of the important design choices and why were they chosen? For example, why might a convolution layer work well with this problem? How might a dropout layer help with creating a successful model?
For the convolution layer in the beginning, it helps getting rid of data that is at the sides and the corners, which are not important aspects for identifying signs(as the signs are in the middle). Dropouts & Pooling works because I believe similar to humans we only look at the most important characteristics and tend to not let details stop us from looking at the whole picture, which all in all helps identification.

###Test a Model on New Images

####1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

Here are five German traffic signs that I found on the web:

![alt text][image3] 

The first and third image are relatively easy to classify.

The second image might be difficult to classify because it is dark and the shape of the sign can be hard to identify.

The forth and fifth image might be difficult to classify as they contain quite a few blurred details on the sign.

####2. Discuss the model's predictions on these new traffic signs and compare the results to predicting on the test set. At a minimum, discuss what the predictions were, the accuracy on these new predictions, and compare the accuracy to the accuracy on the test set (OPTIONAL: Discuss the results in more detail as described in the "Stand Out Suggestions" part of the rubric).

Here are the results of the prediction:

| Image			        |     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| No passing for vehicles over 3.5 metric tons      		| No passing for vehicles over 3.5 metric tons   									| 
| End of all speed and passing limits     			| End of all speed and passing limits 										|
| Keep right					| Keep right											|
| Slippery road      		| Slippery road					 				|
| Road work			| Road work      							|


The model was able to correctly guess 5 of the 5 traffic signs, which gives an accuracy of 100%. This compares favorably to the accuracy on the validation set of 93.9%

####3. Describe how certain the model is when predicting on each of the five new images by looking at the softmax probabilities for each prediction. Provide the top 5 softmax probabilities for each image along with the sign type of each probability. (OPTIONAL: as described in the "Stand Out Suggestions" part of the rubric, visualizations can also be provided such as bar charts)

The code for making predictions on my final model is located in the 11th cell of the Ipython notebook.

For the first image, the model is relatively sure that this is a stop sign (probability of 0.6), and the image does contain a stop sign. The top five soft max probabilities were

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 1.00         			| No passing for vehicles over 3.5 metric tons   									| 
| 1.00     				| End of all speed and passing limits 										|
| 1.00				| Keep right											|
| 1.00      			| Slippery road					 				|
| 1.00			    | Road work      							|


### (Optional) Visualizing the Neural Network (See Step 4 of the Ipython notebook for more details)
####1. Discuss the visual output of your trained network's feature maps. What characteristics did the neural network use to make classifications?


