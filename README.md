# YOLOv3-Training
 This repo contains step-by-step instructions for training on YOLOv3.
 
 ### Arranging .txt Files
 For training a new class, you must collect adequate number of images. It is recommended that to use at least 2000 samples for each class. In order to train the network, a txt file must be available for each photo with the same name. For example if I my image name is horse.jpg, my txt file name must be horse.txt.
 Each txt file must contain the data in the desired format for the objects in the photo. 
 
```
[classId, normalizedXcenter, normalizedYcenter, normalizedWidth, normalizedHeight]
```

First item of the line corresponds to class number. If you have 1 class the classId must be 0. If you have 2 or more class you must match the bounding boxes with your class number. Other items give information about where the object is located. Basically, YOLOv3 wants object center point, width and height via normalized with image width and height. You can see an example below. 
```
normalizedXcenter = xCenter / imageWidth
```
