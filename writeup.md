## Project: Perception Pick & Place

### Exercise 1, 2 and 3 pipeline implemented
#### 1. Complete Exercise 1 steps. Pipeline for filtering and RANSAC plane fitting implemented.
pcl_callback() function creates pipeline for filtering and RANSAC plane fitting.  
#### 2. Complete Exercise 2 steps: Pipeline including clustering for segmentation implemented.  

#### 2. Complete Exercise 3 Steps.  Features extracted and SVM trained.  Object recognition implemented.
I used 50 samples of each object to train the model and was able to achieve 94% accuracy.
![img-1](/normal_confusion_matrix_exercises.jpeg)


### Pick and Place Setup

#### 1. For all three tabletop setups (`test*.world`), perform object recognition, then read in respective pick list (`pick_list_*.yaml`). Next construct the messages that would comprise a valid `PickPlace` request output them to `.yaml` format.

## World 1
3/3 objects successfully identified in world 1:
![world-1](/world1.jpeg)

## World 2
5/5 objects successfully identified in world 2:
![world-2](/world2.jpeg)

## World 3
6/8 objects successfully identified in world 3:
![world-3](/world3.jpeg)

Spend some time at the end to discuss your code, what techniques you used, what worked and why, where the implementation might fail and how you might improve it if you were going to pursue this project further.  



