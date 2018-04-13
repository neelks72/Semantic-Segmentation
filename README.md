# Semantic Segmentation
### Introduction
Semantic segmentation using neural networks is the process of assigning a image class to each pixel of the image.   This project uses a Fully Convolutional  Network (FCN) to identify the pixels that correspond to the road in a given image.  The model classifies the pixels into two classes, road class or everything else (background)

### Fully Convolutional Network Model.
The FCN model developed in this project is derived from pre-trained "modified" version of a VGG-16 model. The model consist of  encoder  and decoder layers. 

**Encoder Layers:**
The purpose the encoder is to extract features from the original image. The encoder layers are derived from the pre-trained VGG-16 model. The fully connected layers of the VGG-16 model is replaced with 1x1 convolutional layer. The function load_vgg (line 20, main.py) downloads the vgg-16 model and extracts the layers 3,4 and 7

**Decoder Layers:**
The decoder layers upscale the output of the encoder layers such it is of the same size of the original image. The decoder layers consists of two up-sampling layers and 2 skip connection layers.  The up-sampling is performed using transposed convolutional layers. The skip connection is used to restore some of the spatial information lost during the encoding process (lines 61, 65 in main.py). The pooling layers 3 and 4 are scaled by a factor of 0.0001 and 0.01 (line 56,57 in main.py) respectively as indicated in the FCN-8 paper and other forums before they are up-sampled

**Regularization**
In order to prevent over-fitting the L2 regularization of 0.001 to the layers. The  regularization penalty is also added to the loss function in addition to the L2 regularization added to the layers (line 89, main.py) 
 
The network uses random kernel initialization with standard deviation of  0.01


**Optimization**
The model uses cross entropy to calculate the loss and is optimized using the Adams Optimizer (line 85, main.py)

### Training the Model.

**Training Data**
The project uses the Kitti Road Dataset. The training data consists of the image and the corresponding ground truth image. The ground truth image shows the road and non-road pixels (everything else).  The images are organized into the training and testing folders.

The non-road pixels are identified by the RGB values [255,0,0]. The following image shows the original and ground truth image:

![enter image description here](https://raw.githubusercontent.com/neelks72/Semantic-Segmentation/master/um_000022.png)


![Segmented image showing background in Red](https://raw.githubusercontent.com/neelks72/Semantic-Segmentation/master/umm_road_000022.png)

**Pre-Processing and Labeling**
The images from Kitti Road data set is resized to 160x576 pixels (lines 87,88 in helper.py). Two labelled dataset corresponding to the road class and the background class is generated from each ground truth image (lines 90-92, helper.py)

**Training Parameters**
The model is trained using the following parameters:
Batch Size=5
No of Epoch=15
Learning Rate=0.0007
Keep Probability=0.5

### Results.
The following are some sample output image. The model trained does a fairly good job of identifying the pixels that correspond to the road.

![enter image description here](https://raw.githubusercontent.com/neelks72/Semantic-Segmentation/master/um_000006.png)

![](https://raw.githubusercontent.com/neelks72/Semantic-Segmentation/master/um_000043.png)

![](https://raw.githubusercontent.com/neelks72/Semantic-Segmentation/master/um_000077.png)

![](https://raw.githubusercontent.com/neelks72/Semantic-Segmentation/master/um_000082.png)
