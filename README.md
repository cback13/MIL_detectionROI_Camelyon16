# MIL_detectionROI_Camelyon16

## Context:

- Immunotherapy gradually emerging as a mainstream therapeutic protocol but is expensive and could cause serious adverse effects.
- The role of the tumor microenvironment in influencing the treatment responses of cancer cells identified as a critical point: need to study the spatial distribution of immune cells.
- The majority of histopathological image analysis's techniques require localized annotation masks made by biologists: very costly to obtain.
## Project:

Implement an algorithm that automatically detects regions of interest on the slide using artificial intelligence and so continue the implementation of Spatiopath: a spatial analysis framework for histopathology.

## Problem description: 
Characterization of tumor ROI1 is a difficult step towards automated digital cytology analysis, as it involves many challenges:
- Deal with WSIs
- Complicated segmentation due to very noisy background
- Unavailability of large datasets to evaluate the algorithm.

We will use the public dataset Camelyon16, composed of a total of 400 whole-slide images (WSIs) of sentinel lymph node. These are .tif type pyramid images. We will work for training on 60 images of normal type and 60 of tumor type.
We will work at level 0, which corresponds to the maximum 
resolution.

![image](https://user-images.githubusercontent.com/111517884/185421768-69288104-084a-4297-99c2-8b3e8cc5b03f.png)

## Mask Segmentation: 
- Conversion in HSV.
- Mask on the saturation chanel with an Otsu filter.
- Superposition of the first mask and our image in order to remove the black elements.
- Second Otsu filter.

## Extracting patches:
![image](https://user-images.githubusercontent.com/111517884/185422360-99726b17-bf1d-4c85-aa05-6f23dbe34d0a.png)
Extraction of 224x224 tiles, between and patches by images. On the right, visualization of the patches positioned on the slide. No sampling because the image sizes vary too much and there is a great risk of losing information.

## MIL
The goal is to obtain, after training, scores for each patch indicating the negative (no tumor) and positive (tumor) areas of interest.

Features Extraction: We use ResNet-50 pre-trained on ImageNet to extract features from tiles.

![image](https://user-images.githubusercontent.com/111517884/185426279-155164df-54a8-4c16-8c0f-472510e8a874.png)



