# MIL_detectionROI_Camelyon16

## Context:

- Immunotherapy gradually emerging as a mainstream therapeutic protocol but is expensive and could cause serious adverse effects.
- To understand the role of the tumor microenvironment in influencing the treatment responses we need to study the spatial distribution of immune cells.
- Challenge: the majority of histopathological image analysis's techniques require localized annotation masks made by biologists: very costly to obtain.

## Project:

Implement an algorithm that automatically detects regions of interest on the slide using artificial intelligence and so adapt G-Soda (a spatial analysis framework for bio-Image analysis) to WSIs .

## Problem description: 

Characterization of tumor ROI is a difficult step towards automated digital cytology analysis, as it involves many challenges:
- Deal with WSIs
- Complicated segmentation due to very noisy background
- Unavailability of large datasets to evaluate the algorithm.

## Material

- Dataset:public dataset Camelyon16, 400 WSIs of sentinel lymph node, .tif type pyramid images
- Balance dataset: 110 WSIs of normal type and 110 of tumor type (for reasons of  memory size and calculation time limitations,), 80% for training, 20% for validation, work at level 0=the maximum resolution.
- Test dataset: 129 images of all types. 
- Annotations: global, tumor or normal

![image](https://user-images.githubusercontent.com/111517884/185421768-69288104-084a-4297-99c2-8b3e8cc5b03f.png)

## Tissue sample extraction: 
- Conversion in HSV.
- Mask on the saturation chanel with an Otsu filter.
- Superposition of the first mask and our image in order to remove the black elements.
- Second Otsu filter.

## Extracting patches:
![image](https://user-images.githubusercontent.com/111517884/185422360-99726b17-bf1d-4c85-aa05-6f23dbe34d0a.png)
Extraction of 500x500 patches, then resize to 244x244. Between 80 and 34 072 patches by images. We cut our images into patches in order to pass them through our neural network. On the right, visualization of the patches positioned on the slide. No sampling because the image sizes vary too much and there is a great risk of losing information.

## MIL
The interest of MIL is to obtain, after training, scores for each patch indicating the negative (no tumor) and positive (tumor) areas of interest.

Features Extraction: We use ResNet-50 pre-trained on ImageNet to extract features from patches.

![image](https://user-images.githubusercontent.com/111517884/187883268-5b6e8a6d-478c-47de-9ebd-42e8aea4193f.png)

## Results
![image](https://user-images.githubusercontent.com/111517884/187883514-a2403964-ce5d-4a75-9116-9e2e4cd108b7.png)

![image](https://user-images.githubusercontent.com/111517884/187948192-8148bbbb-45b0-4146-a2eb-4cb640eeb742.png)

![image](https://user-images.githubusercontent.com/111517884/187948134-b54be3ea-3a66-4f18-a9df-10c284ba833b.png)

![image](https://user-images.githubusercontent.com/111517884/187948107-5e978ce4-cad5-407d-99f2-5f2d3bcb4a3d.png)

![image](https://user-images.githubusercontent.com/111517884/187947958-bc377022-49e9-44e8-a605-c95895e659db.png)



## Discussions
Despite the fact that we are working on a sub-part of the dataset  we managed to reimplement the Chowder method, reproduce results close to theirs and detect tumor patches.
Next challenges to overcome: 
- The non-optimal threshold
- The unbalanced distribution of the patches (normal>>tumor)
- Finetune on ImageNet which features are far from our datase



