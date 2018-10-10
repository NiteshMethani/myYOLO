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
