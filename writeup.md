## 3D Object detection

I have done below configurations

- Configured the ranges channel to 8 bit and view the range /intensity image (ID_S1_EX1)
- Use the Open3D library to display the lidar point cloud on a 3d viewer and identifying 10 images from point cloud.(ID_S1_EX2)
- Create Birds Eye View perspective (BEV) of the point cloud,assign lidar intensity values to BEV,normalize the heightmap of each BEV 
- used fpn resnet model
- Convert BEV coordinates into pixel coordinates and convert model output to bounding box format
- Compute intersection over union, assign detected objects to label if IOU exceeds threshold
- Compute false positives and false negatives, precision and recall



## Step-1: Compute Lidar point cloud from Range Image

We are first previewing the range image and convert range and intensity channels to 8 bit format. After that, we use the openCV library to stack the range and intensity channel vertically to visualize the image.

- Convert "range" channel to 8 bit
- Convert "intensity" channel to 8 bit
- Crop range image to +/- 90 degrees  left and right of forward facing x axis
- Stack up range and intensity channels vertically in openCV




We then use the Open3D library to display the lidar point cloud on a 3D viewer and identify 10 images from point cloud
- Visualize the point cloud in Open3D
- 10 examples from point cloud  with varying degrees of visibility



Point cloud images

![img1](img/pointcloud1.png)

![img1](img/pointcloud2.png)

![img1](img/pointcloud3.png)

![img1](img/pointcloud4.png)

![img1](img/pointcloud5.png)

![img1](img/pointcloud6.png)

![img1](img/pointcloud7.png)

![img1](img/pointcloud8.png)

![img1](img/pointcloud9.png)

![img1](img/pointcloud10.png)


Stable features include the tail lights, the rear bumper ,car front lights, rear window shields. Bounding boxes are correctly assigned to the cars .


## Step-2: Creaate BEV from Lidar pointcloudL

we are:
- Converting the coordinates to pixel values
- Assigning lidar intensity values to the birds eye view BEV mapping
- Using sorted and pruned point cloud lidar from the  previous task
- Normalizing the height map in the BEV
- Compute and map the intensity values





## Step-3: Model Based Object Detection in BEV Image


- Instantiating the fpn resnet model from the cloned repository configs
- Extracting 3d bounding boxes from the responses
- Transforming the pixel to vehicle coordinates
- Model output tuned to the bounding box format [class-id, x, y, z, h, w, l, yaw]



## Step-4: Performance detection for 3D Object Detection

In this step, the performance is computed by getting the IOU  between labels and detections to get the false positive and false negative values.The task is to compute the geometric overlap between the bounding boxes of labels and the detected objects:

- Assigning a detected object to a label if IOU exceeds threshold
- Computing the degree of geometric overlap
- For multiple matches objects/detections pair with maximum IOU are kept
- Computing the false negative and false positive values
- Computing precision and recall over the false positive and false negative values



The precision recall curve is plotted showing similar results of precision =0.996 and recall=0.81372


## Gist:

High Necessity is there for the conversion of range data to point cloud . I have used resnet and YOLO to convert these high dimensional point cloud representations to object detections through bounding boxes which is essential for 3D object detection. Evaluated the performance with help of maximal IOU mapping ,mAP, and representing the precision/recall of the bounding boxes are essential to understand the effectiveness of Lidar based detection.
