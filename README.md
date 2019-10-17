# YOLOv3-Training
 This repo contains step-by-step instructions for training on YOLOv3.
 
 ## Arranging .txt Files
 For training a new class, you must collect adequate number of images. It is recommended that to use at least 2000 samples for each class. In order to train the network, a txt file must be available for each photo with the same name. For example if I my image name is horse.jpg, my txt file name must be horse.txt.
 Each txt file must contain the data in the desired format for the objects in the photo. 
 
```
[classId, normalizedXcenter, normalizedYcenter, normalizedWidth, normalizedHeight]
```

First item of the line corresponds to class number. If you have 1 class the classId must be 0. If you have 2 or more class you must match the bounding boxes with your class number. Other items give information about where the object is located. Basically, YOLOv3 wants object center point, width and height via normalized with image width and height. You can see an example below. 
```
normalizedXcenter = xCenter / imageWidth
```
You can find sample image and txt representation in [samples](samples/) directory. 

## Arranging Configuration File
Configuration files must be set before training starts. There are 3 basic files that need to be adjusted which are namely config file, names file an data file.

* The input width and height of the network must be specified. The input width and height of the network affect, network success, speed, computational cost etc. Therefore, it must be arranged carefully for the desired system. 
* Batch size specifies how many photos will be processed in one iteration so it is cruical for system convergence. Finding the average error of photos and updating the system according to this error will facilitate the convergence of the system. It is generally set to 64 or 32.
* Subdivision determines how many photos will be processed in a sub-section of batch size. Therefore subdivision must be smaller than batch size. How many transactions take place at the same time can be found as 
```
number of photos processed at the same time = batch size / subdivision
```
### Arranging Anchor Sizes
Since YOLOv3 makes estimations over anchors, it is important to calculate anchors depending on the given dataset in terms of successful training. It can be found with given function in the [AlexeyAB](https://github.com/AlexeyAB/darknet) darknet repo. Just open the terminal and write the following command

```
./darknet detector calc_anchors cfg/face.data -num_of_clusters 9 -width 800 -height 800
```
You must give the location of the arranged data file, you must set anchor number and network input width and height. This function calculates anchor size with using k-means clustering.

### Arranging Class and Filter Number
In the configuration file you must set the class number and filter number. In the configuration file 610, 696 and 783 lines, it must be arranged class number. Also, in lines 603, 689 and 776 it must be set the filter number. Filter number can bu calculated as 
```
filter number = (B x (5 + C))
```
In this formula C is the class number and B is the number of prediction across different scale. It is arranged to 3 in YOLOv3 because it predicts 3 different scale.

## Arranging Data File
In data file number of classes must be set. Also exact location of names, training and validation file must be arranged. YOLOv3 saves model file for each 100 iterations. Finally, in data file it must be adjusted to exact backup location of model files.


## Arranging Names File
In names file, each training class must be written respectively.

## Starting Training
For starting training, you must use a model file. You can use a model file which is produced by another training or a model file which consists of given random numbers or you can produce pre-trained model via using a model file. The following command will produce pre-trained model.
```
./darknet partial cfg/yolov3.cfg yolov3.weights yolov3.conv.81 81
```
With using produced pre-trained model, you can start your training with following command. It can be used -map function for observing the success rate of trained model with respect to given validation dataset. 
```
./darknet detector train cfg/face.data cfg/face.cfg yolov3.conv.81 -map
```
If training stops for some reasons, you can continue where the training is left. With using the following command you can continue the training.
```
./darknet detector train cfg/face.data cfg/face.cfg backup/face_last.weights -map
```
The name of the saved chart can be changed to avoid loss of previously extracted loss graph.
