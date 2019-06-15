# GRAB_SEA_CVChallenge
## Optimizing a MobileNet classifier on the constrained Stanford Cars Dataset

This is a MobileNetV2 based submission for the Grab Ai For SEA Challenge 2- rapid visual recognition of the Stanford Car Dataset.
The criteria for performance were as follows


* Accuracy
* Precision
* Recall
* Speed/performance (derived from discussions with judges during opening ceremony)

As such, a MobileNetV2 based classifier was chosen due to its lightweight footprint, low number of parameters, and capability for near real-time mobile-based recognition.

## Dataset
The Stanford car dataset consists of 16185 images of automobiles of 196 classes. Despite it's large size, the number of images per class is relatively small, and as cars are visually highly similiar, this makes for a challenging exercise in make and model differentiation.

## Implementation
The Stanford car dataset has been extensively studied on various networks, including on MobileNet platforms. 
[Source](https://arxiv.org/pdf/1806.02987.pdf?fbclid=IwAR26yjKltuRmb9q9U8Dj3F-oGDXWVrp1UW_ipq3_ZanYmFWglijwbatqO2g)

Our approach relies on extensive pre-processing and data augmentation process, detailed below:


