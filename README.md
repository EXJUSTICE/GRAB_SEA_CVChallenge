# GRAB AI For SEA Computer Vision Challenge
## Optimizing a MobileNet classifier on the constrained Stanford Cars Dataset

This is a MobileNetV2 based submission for the Grab AI For SEA Challenge 2- rapid visual recognition of the Stanford Cars-196 Dataset.
The criteria of submissions were:


* Accuracy
* Precision
* Recall
* Speed & lightweight (inferred from discussions with Dr. Hannes Krupa during kick-off ceremony)

As such, a MobileNetV2 based classifier was chosen due to its lightweight footprint, low number of parameters, and capability for near real-time mobile-based recognition. 

This repository consists of three Google Colaboratory Notebooks
1. ImageAI crop (Preprocessing)
2. Training, Finetuning, MobileNetV2 (Preprocessing & training)
3. Prediction(Evaluation)


For detailed instructions to run this repository, please see the "Instructions" section of this README.

## Instructions
All Notebooks were run in Google's Colaboratory envrionment, with the GPU accelerator turned on. Notebook's 1 and 2 are provided to illustrate the preprocessing and training of the network, and can be rerun to replicate our results. Notebook 4 is designed to provide predictions for individual images, together with probabilities for each prediction.

Notebook 3 loads in models trained using books 1 & 2 from Google Drive. 

For evaluation, it is suggested that the Judging team utilize book 3 on their custom dataset to generate the predictions together with confidence intervals.

GDrive (Online as of 17-06-2019) Colaboratory instances of the notebooks can be found here:

* https://drive.google.com/open?id=1exYHUnpYhTY-GswnnNJJP3R4KNTHi6Ea

* https://drive.google.com/open?id=14UBY2iahaYq4EGa5HKDHC0uSZOt34omA

* https://drive.google.com/open?id=1wOWeYKDEWt5Mk28fDwWUstNxUHvausVp

## Dataset
The Stanford Cars-196 dataset consists of 16185 images of automobiles of 196 classes. Despite it's large size, the number of images per class is relatively small, and as cars are visually highly similiar, this makes for a challenging exercise in make and model differentiation.

For our approach, a version of the dataset pre-separated into folders was obtained from Kaggle. This is the exact same dataset, only with the training data arranged in their respective labelled directories for convenience.

For training purposes, the dataset was split into training and validation datasets at a ratio of 80:20.

## Implementation
The Stanford Cars-196 dataset has been extensively studied on various networks, including on MobileNet platforms. 
[Source](https://arxiv.org/pdf/1806.02987.pdf?fbclid=IwAR26yjKltuRmb9q9U8Dj3F-oGDXWVrp1UW_ipq3_ZanYmFWglijwbatqO2g)

Our approach relied on extensive pre-processing data augmentation processes to improve the quality and quantity of training data available, together with a light MobileNetV2 architecture for possible mobile app-based rollout.

#### Background cropping data

As Each image featured excessive amounts of background noise. While boundary boxes were provided by the dataset, it was decided that a YOLO-based cropping detector would be more scalable under real-world data collection circumstances. Such a detector was implemented in Notebook 1, and the outputs collected and stored on Google Drive as inputs for Notebook 2.

#### __Segmentation cropping__
<p align="center">
  <img src="https://github.com/EXJUSTICE/GRAB_SEA_CVChallenge/blob/master/example.jpg" >
</p>
While different classes of vehicles, i.e. sedans and SUV's, are visually distinct, vehicles within the same class are harder to distinguish. This was believed to have led to a poor initial validation accuracy of less than 50%. It was identified that the primary feature differences between vehicle model and maker of the same type were in the front and rear, respectively. 


To isolate this area from rest of the features within the raw image, a segmentation cropping-based functiono was implemented. Briefly, this worked by dividing the image into two sets of halves, determined by width and height, respectively. A quartering function was also evaluated but found to perform worse, which was attributed to the each quarter possessing incomplete feature information.


#### __Just-In-Time pre-processing__

Before each batch of images were fed into our network, they underwent a set of preprocessing data-augmentation methods derived from the Keras and Tensorflow libraries designed to improve the performance and robustness of our model. While the Keras-based methods focused on translations and transformations, the Tensorflow library provided more advanced capabilities to modify the colorspace.

**Keras**

* Rescaling
* Shear-based transformations
* Zoom-based transformations
* Translations
* Flipping

**Tensorflow**

* Hue randomization 
* Saturation randomization
* Brightness randomization
* Contrast randomization



#### __Architecture__

<p align="center">
  <img src="https://github.com/EXJUSTICE/GRAB_SEA_CVChallenge/blob/master/GrabSEAarchitecture.png" >
</p>

Our model consists of the base MobileNetV2 model, with the top layers relaced with a two densely connected layers (of size 1024 and 196, respectively), separated by a 50% dropout layer to prevent overfitting. The network was pre-loaded with ImageNet weights, and training was done using an ADAM optimizer at a learning rate of 0.0002.

As the car class of ImageNet is relatively small and varied, training of the base model weights was allowed beyond 75 layers of th base network, with fine-tuning permitted at 30 layers onwards. Fine-tuning was done using an RMSProp optimizer at a learning rate of 2E-5.The model was trained for 50 epochs, with fine-tuning permitted for 10 epochs.


## Performance
<p align="center">
  <img src="https://github.com/EXJUSTICE/GRAB_SEA_CVChallenge/blob/master/accuracy.png" >
</p>

A plot of accuracy and accuracy versus epochs during the inital training process is shown above.

After training for 50 epochs, a mean validation acccuracy of 85.3% was achieved. After fine-tuning was executed for 10 epochs, this increased to 89.6 % and stabilized.

