# TEST Vehicle Detection and Tracking 

## Overview

This project is part of my Self-Driving Car course. It identifies vehicles in a video from a front-facing camera on a car. 

## Implementation 

1. Data extraction

Get_hog_features returns HOG features and visualisation (if needed). It’s in the second cell of the IPython notebook included in the submission. Numerous parameters were including various color spaces. In the end, I decided to use parameters which are described below. 

color_space = 'YCrCb' 
orient = 9   
pix_per_cell = 8  
cell_per_block = 2  
hog_channel = 'ALL'  
spatial_size = (32, 32)  
hist_bins = 32     
spatial_feat = True  
hist_feat = True  
hog_feat = True  
y_start_stop = [400, 656] 
x_start_stop = [600 1280] 
cells_per _step = 2 (overlap) 
window = 64 
 
2. Classifier Training 

First, an array stack of feature vectors and labels vectors were created using vstack and hstack. Subsequently, the data was randomized and split into training and test sets (80:20 ratio). The StandardScaler was used to normalize the data and was applied to the training set. Then, the classifier was trained using LinearSVC. 

3. Sliding Window Search 

I tested various scales and overlaps. In the end, I decided to avoid using the smallest scales because they have prolonged the image analysis, provided a lot of false positive and didn’t contribute significantly to the result. This decision allowed me to use bigger scales from 1.2 to 2.25. Bigger scales also allowed to identify the biggest vehicles which are the closest and hence, are the biggest threat.  I searched only a specific part of the image. In y-direction, I got rid of the image above the horizon (the sky) by searching only in the range 400-656 pixels. In-x-direction, I only searched the area on the right (600-1280).  The vehicles are only on the right side (the car is driving on the most left lane) and this eliminates the vehicles moving from the opposite side. This is specified in the function process_image (6th cell in the notebook) whereas the function doing sliding window searching and making a prediction is called find_cars (5th cell). 

4. Video Implementation 

For each scale, the heatmap was created. Then, each heatmap was added up and the threshold of 2 was applied to get rid of false positives. Moreover, collections.deque was used which held last 15 images. For each iteration, last 15 images are added up. Then, the coordinates of the boxes were identified using labels function and boxes were plotted using draw_labeled_bboxes function (in process_image function).  

5. Examples 

I have attached examples in the examples folder. 

6. Comments 

The approach I took to ignore the left side of the image works perfectly in this case. However, the perfect solution should include the left side as well in case the vehicle is in the middle of the road. Moreover, the high number of scales makes the simulation significantly longer than it could be. However, detecting white vehicles was a challenge and this is how I got enough detection. There is also an issue of detecting vehicles near the edge of the image. I don’t think it is a major problem. However, applying sliding window search with bigger overlap could fix it.  Another issue is running time, I did my project on my laptop so it took an hour to accomplish it (10 hours mentioned in a notebook is a mistake as my laptop shut down during the simulation and I opened it the next day). Fewer scales would improve the performance. 
 

 


