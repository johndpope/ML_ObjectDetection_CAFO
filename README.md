# Using a TensorFlow CNN to Detect Concentrated Animal Feeding Operations (CAFO) in Satellite Imagery
Repository containing model development, training, and execution for the CAFO satellite imagery detection process.

Summary: The Environmental Protection Agency is interested in building a dataset of Concentrated Animal Feeding Operation (CAFO) locations. This process is an exploration of how we can use a Convolutional Neural Network (CNN) to scan satellite imagery from the [National Agricultural Imagery Program (NAIP)](https://www.fsa.usda.gov/programs-and-services/aerial-photography/imagery-programs/naip-imagery/) and detect CAFO facilities, writing the detected objects as features in a GIS dataset. 

The process is encapsulated in the following steps:

Step 1: Data Preparation - Image "Chip" Extraction

Step 2: Annotation and Labeling of the Images

Step 3: CNN Model Training

Step 4: CNN Model visual verification

Step 5: Output feature transfer to GIS

Please consider this project and all aspects of this repository as beta, with extensive changes and further testing still needed to finalize.

## Step 1: Data Preparation - Image "Chip" Extraction

Goal: The goal of this step is to extract images containing satellite imagery over a known CAFO location. These images will be manually labeled for the PASCAL_VOC format that a TensorFlow model can interpret for CNN development. 

Description: Using an ArcGIS Pro project, we will leverage two data sources to build our model inputs: a [NAIP imagery service](https://naip.arcgis.com/arcgis/services/NAIP/ImageServer) and a feature class containing identified CAFO sites in Kentucky (gis_inputs/ky_afo_lonx_nonzero) used in a image classification workshop. 

The 1_image_exporter.py script iterates on each record of the Kentucky feature class containing CAFO sites, loading the NAIP imagery at the location at three different specified scales: 1:1000; 1:2000; and 1:3000, and exporting each as a .JPEG image in a designated directory. A total of 250 locations are used, resulting in 750 input features (3 for each location) for training and testing.

## Step 2: Annotation and Labeling of the Images

Goal: We need to generate TFRecords: the inputs to our CNN. To do this, we must use our exported images and manually label where the Kentucky CAFO site resides in each image. The outputs will be provided to TensorFlow as a PASCAL_VOC format that can be easily interpreted by an existing CNN or for development of a new CNN. 

Description: Using [LabelImg](https://github.com/tzutalin/labelImg), we can create bounding boxes on images and generate xmls with data that can be used to generate TFRecords for our CNN. 

Once installed (you can get it with git clone https://github.com/tzutalin/labelImg), labelImg is pointed at the directory of our exported images from step 1. A lengthy process ensues: we must iterate on each image and using our own Neural Network (our eyes and noggins!) we must annotate where the CAFO sites are in each image.

Example: 

![Extracted CAFO Image](https://github.com/Qberto/ML_ObjectDetection_CAFO/blob/master/doc/img/img_106_2000.jpg)

![Labeled for Test/Train Split](https://github.com/Qberto/ML_ObjectDetection_CAFO/blob/master/doc/img/img_106_2000_labeled.jpg)

This is a simple but repetitive process. It takes some time but the more images you have and the more detailed the labeling, the better the model can be trained for detection of these sites in any location.



## Step 3: Split labeled images into train/test samples

## Step 4: Generate TF Records from the train/test splits

## Step 5: Set up a configuration file containing CNN hyperparameters

## Step 6: Train

## Step 7: Export a computation graph from the new trained model

## Step 8: Detect CAFO sites in real time!

## Step 9: ...

## Step 10: Party.



### Appendix A: CNN Model Pseudocode

The CNN Model pseudocode can be categorized by two general steps: Feedforward and backpropagation. In other terms, the movement of the data through the neural network, and the learning or training of the model given results in an iteration. A single feedforward and backpropagation loop is known as an "epoch".

#### Feedforward: 
input > weight > hidden layer 1 (activation function) > weights > hidden layer 2 (activation function) > weights > output layer

#### Backpropagation:
compare output to intended output > cost function (cross entropy)
optimization function (optimizer) > minimize cost (AdamOptimizer, Stochastic Gradient Descent, AdaGrad, etc.)

#### Epoch:
feedforward + backpropagation
