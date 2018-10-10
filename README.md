1. Create a directory where all the code for the custom YOLO detector will live.
```
mkdir myYOLO
cd myYOLO
```

2. Create `darknet.py` and `util.py` files inside this directory.
`darknet.py` will contain the code that creates the YOLO network.
`utils.py` will contian some helper functions.

3. The `.cfg` file describes the layout of the network block by block. We will use the official [cfg file](https://github.com/pjreddie/darknet/blob/master/cfg/yolov3.cfg) as released by the authors.
```
mkdir cfg
cd cfg
wget https://raw.githubusercontent.com/pjreddie/darknet/master/cfg/yolov3.cfg
```
There are 5 types of layers used in YOLO.
- Convolutional Layer
- Shortcut Layer :- It is a skip connection. If the `from` parameter is say -3, then the out of the shortcut layer  layer is obtained by adding the output features of the previous layer with the *3-rd* layer backwards from the shortcut layer.
- Upsample Layer :- Upsamples the output of the previous layer by a factor of `stride` using bilinear upsampling.
- Route :- Route layer has a `layers` attribute which can take either one or two parameters. When `layers` attribute has only one value, it outputs the feature maps of the layer indexed by that value. When `layers` attribute has two values, it outputs the concatenated feature maps of the layers indexed by those values. For example, if `layers = -1, 61`, then the route layer will output the feature map of the previous layer (-1) concatenated with the feature map of the 61st layer along the depth dimension.
- YOLO Layer :- This layer defines the anchors. In the original `cfg` file, 9 anchors are defined. Anchor boxes are defined using their (width, height). `mask` attribute defines the index of the anchor boxes which will be actually used in the algorithm. YOLO layer is oftenly referred as Detection Layer. In total, we have Detection layers at 3 scales and for each scale we will propose 2 anchor boxes, making up for a total of 9 anchors. YOLO doesnot handle few cases: if more than two objects are there in the same grid cell, then YOLO will break tie and will detect only 2 objects (because only 2 anchors are proposed), another case is if there are multiple objects in the cell whose center lies in the same anchor box, then YOLO will detect only 1 object (becaue each anchor box is responsible to detect only 1 object). To overcome this, anchor boxes are predicted using K-means clustering algorithm. Read [this](https://medium.com/@vivek.yadav/part-1-generating-anchor-boxes-for-yolo-like-network-for-vehicle-detection-using-kitti-dataset-b2fe033e5807) article to get more information on how YOLO proposes anchor boxes using K-means clustring.

There is one more block `[net]` in the `cfg` file. This block describes the informatin about the network input and training parameters.
