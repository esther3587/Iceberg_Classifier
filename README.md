### Iceberg Classifier ###

## Introduction ##

Drifting icebergs present threats to navigation and activities in areas such as offshore of the East Coast of Canada.

Currently, many institutions and companies use aerial reconnaissance and shore-based support to monitor environmental conditions and assess risks from icebergs. However, in remote areas with particularly harsh weather, these methods are not feasible, and the only viable monitoring option is via satellite.

Statoil, an international energy company operating worldwide, has worked closely with companies like C-CORE. C-CORE have been using satellite data for over 30 years and have built a computer vision based surveillance system. To keep operations safe and efficient, Statoil is interested in getting a fresh new perspective on how to use machine learning to more accurately detect and discriminate against threatening icebergs as early as possible.

In this competition, you’re challenged to build an algorithm that automatically identifies if a remotely sensed target is a ship or iceberg. Improvements made will help drive the costs down for maintaining safe working conditions.

## Data Information

The data (train.json, test.json) is presented in json format. The files consist of a list of images, and for each image, you can find the following fields:

id - the id of the image
band_1, band_2 - the flattened image data. Each band has 75x75 pixel values in the list, so the list has 5625 elements. Note that these values are not the normal non-negative integers in image files since they have physical meanings - these are float numbers with unit being dB. Band 1 and Band 2 are signals characterized by radar backscatter produced from different polarizations at a particular incidence angle. The polarizations correspond to HH (transmit/receive horizontally) and HV (transmit horizontally and receive vertically). More background on the satellite imagery can be found here.
inc_angle - the incidence angle of which the image was taken. Note that this field has missing data marked as "na", and those images with "na" incidence angles are all in the training data to prevent leakage.
is_iceberg - the target variable, set to 1 if it is an iceberg, and 0 if it is a ship. This field only exists in train.json.

## Process Image Data

We create a third channel information by combining band_1 and band_2 data, and only use training data with incidence angle information available. This improves the accuracy. In order to get more training data, we also flip the images horizontally and vertically to generate more images.

## Keras with Tensorflow backend

For this project I use keras package for easy stacking several CNN layers in my training model. I used 4 convolutional layers with maxpooling and dropout, 2 dense layers, 1 flatten layer, and 1 sigmoid layer. Here I use Adam Optimizer and binary_crossentropy loss.

## Ensemble Learning

I stacked 5 different models and take median value as my final prediction. The logloss improves from 0.18 to 0.16. So far, top 26% on Kaggle LB.

## Required Package ##

- keras
- numpy
- pandas
- h5py
- sklearn
- cv2

## Reference
 - [Kaggle competition](https://www.kaggle.com/c/statoil-iceberg-classifier-challenge)
 - [Keras](https://keras.io/layers/convolutional/)
 - [Statoil](https://www.statoil.com)

