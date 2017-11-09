## Project: Perception Pick & Place

### Exercise 1, 2 and 3 pipeline implemented
#### 1. Complete Exercise 1 steps. Pipeline for filtering and RANSAC plane fitting implemented.
pcl_callback() function creates pipeline for filtering and RANSAC plane fitting.  Additionally, I created two helper functions, a voxel_downsample() function and passthrough_filter function.  These were created in an attempt to make the pcl_callback() function more efficient.

The following are the results of completing voxel grid downsampling, a passthrough filter (to get rid of the table), and RANSAC plane segmentation.  

Objects isolated:
![exercise1-1](/objects.jpeg)

Isolated table (eliminated from passthrough filter):
![exercise1-1](/table.jpeg)
#### 2. Complete Exercise 2 steps: Pipeline including clustering for segmentation implemented.  
After applying filters and RANSAC plane segmentation clustering for segmentation was implemented to identify objects individually, ultimately to be able to train the model.  

The following code implemented euclidean clustering on the point cloud.  I played around with the cluster tolerance and min/max cluster sizes in order to see what best fit the model.

```
white_cloud = XYZRGB_to_XYZ(extracted_inliers)
    tree = white_cloud.make_kdtree()
    ec = white_cloud.make_EuclideanClusterExtraction()
    ec.set_ClusterTolerance(0.02)
    ec.set_MinClusterSize(60)
    ec.set_MaxClusterSize(2000)
    ec.set_SearchMethod(tree)
    cluster_indices = ec.Extract()
```

The point clouds were then given unique colors using helper code provided in the lesson.  The following is the result of clustering:

![exercise2-1](/clustering.jpeg)

#### 2. Complete Exercise 3 Steps.  Features extracted and SVM trained.  Object recognition implemented.
I used 50 samples of each object to train the model and was able to achieve 94% accuracy.  The normalized confusion matrix I obtained is as follows:

![img-1](/normal_confusion_matrix_exercises.jpeg)


### Pick and Place Setup

#### 1. For all three tabletop setups (`test*.world`), perform object recognition, then read in respective pick list (`pick_list_*.yaml`). Next construct the messages that would comprise a valid `PickPlace` request output them to `.yaml` format.
All three outputs are located in `/pr2_Robot/scripts/output_*.yaml`.  I was able to correctly identify 100% of the items in worlds 1 and 2, but came up short with only 6/8 in world 3.  I believe this was due to the glue slightly hiding behind the book.  My results for the 3 worlds are given below:  

###### World 1
3/3 objects successfully identified in world 1:
![world-1](/world1.jpeg)

###### World 2
5/5 objects successfully identified in world 2:
![world-2](/world2.jpeg)

###### World 3
6/8 objects successfully identified in world 3:
![world-3](/world3.jpeg)
Glue mis-identified as "soap" and "soap2" misidentified as glue.

### Wrap-Up
Overall the model I generated for identification worked pretty well. The code established in the lab exercises 1-3 translated well into the project.  I did change a few parameters and added an additional function to eliminate the side tables from the view of the pr2 robot.  

To eliminate the side tables I simply added another passthrough filter in the Y direction as follows:
```
def passthrough_filter_y(cloud):
    passthrough = cloud.make_passthrough_filter()
    filter_axis = 'y'
    passthrough.set_filter_field_name(filter_axis)
    axis_min = -0.5
    axis_max = 0.5
    passthrough.set_filter_limits(axis_min, axis_max)
    cloud_filtered = passthrough.filter()

    return cloud_filtered
```

From the exercises to the project I also modified parameters used for clustering.  I found that the clustering tolerance needed to be lower than in the exercises (0.01 vs 0.02) and that both the max and min cluster sizes needed to increase to better identify the objects.

I did not complete the challenge part of this project, but plan on continuing to refine this model and completing the pick and place operation in the future.  



