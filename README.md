# Biomedical-Segmentation-of-Brain-MRI-images

![image](https://user-images.githubusercontent.com/71550473/116810295-a77b0700-ab60-11eb-80b5-8aa2a608edc4.png)


### Abstract

Magnetic resonance imaging is a procedure in which radio waves and a powerful magnet linked to a computer are used to create detailed pictures of areas inside the body. These pictures can show the difference between normal and diseased tissue. MRI makes better images of organs and soft tissue than other scanning techniques, such as computed tomography (CT) or x-ray Various methods have been used for MRI image segmentation, but since the advent of CNNs, the efficiency of this classification of pixels into various diseased or non-diseased parts have seed significant improvements.

### Introduction

3D brain MRI scan data set available at http://medicaldecathlon.com/ have been used. It is an online challenge focusing on semantic segmentation tasks, where almost all preprocessed data was provided and abnormality (or in our case) tumorous regions of brain needed to be detected from MRI scans.

### Background

The goal is to build a multi-class deep learning segmentation model, which can first take in labeled data as training and accordingly tune the network parameters to accurately classify unlabeled data. 3 different abnormalities in each image are identified: edemas, non-enhancing tumors, and enhancing tumors. 

### Dataset Information
Every training input will be composed of two individual files:

1. The first file contains a 4D array in the shape of (240, 240, 155, 4). Since we have a 3D image data, so just like a pixel in 2D image, we have a voxel in 3D volume, where each voxel has certain characteristics (just like pixel has in 2D). So first 3 dimensions are the X, Y, and Z values for a given voxel and the 4th dimension gives information about a certain sequence at voxel ( not label). The difference between a pixel and a voxel is that a pixel is a square inside of a 2D image with a position in a 2D grid and
a single color value, whereas a voxel is a cube inside of a 3D model that contains a position inside a 3D grid and a single color value.

2. The second file is a label file containing a 3D array with the shape of (240, 240, 155). This holds labels of all the voxels in the 3D volume which can be 0 for background, 1 for edema, 2 for non-enhancing tumor, 3 for enhancing tumor.

We have a total of 484 training samples and 266 testing samples

### Packages
Some of the important packages used are as follows:
• Keras : a framework to build deep learning models.
• Nibabel : extraction of the images and labels from the 3D MRI files.
• numpy : mathematical operations.
• pandas : handling the data.

### Methodology

As feeding the entire 3D image data to the deep learning model can be computationally intensive, we can first generate sub volumes or “patches” of our dataand then feed to the network. More specifically, we have generated sub-volumes of shape [N, M, K] where N = M = 160 and K = 16 from our images. Also a large portion of the MRI volumes could just be black background without any tumor so we are only going to select the patches containing a maximum of 0.95 portion of non-tumor region and at least 0.05 portion of tumor region,
which can be done by filtering the volumes based on the values present in the background labels.
We standardize the values to have a mean zero and standard deviation equal to 1. Standardization is performed to prevent features with wider ranges from
dominating the distance metric.

### Model Architecture
As far as the deep learning model architecture is considered, we have made use of 3D U-net model, which is the most preferred model for biomedical image
segmentation tasks. The 3D-Unet consists of multiple contracting encoder layers to analyze the whole image and multiple successive expanding decoder layers to
produce a full-resolution segmentation. It basically comprises of an analysis path followed by a synthesis path. There are connections between the analysis and synthesis path of same resolution. These help the synthesis path to receive the essential details from the analysis path to generate the proper output.

![image](https://user-images.githubusercontent.com/71550473/116810141-94b40280-ab5f-11eb-9de7-8b110b420df7.png)


### Results
Output will give 4 dimensions and corresponding to each voxel we will have an array containing 3 probability values for the three conditions. A threshold value is then used to classify each voxel as tumorous or not. So finally in output we are getting the model prediction over the whole image along with a visual of the ground truth vs. prediction.
Before running the U-net model on entire MRI scan, we have first checked the model’s performance on a patch and using the evaluation metric of specificity and sensitivity, found out how well the model is performing in segmenting the tumorous region of image. 
After running the model on single patches of image, we have run it on the entire MRI scan and below are the results we obtained with red color indicating edema, green indicating non-enhancing tumor and blue is an enhancing tumor.

![image](https://user-images.githubusercontent.com/71550473/116810157-bad9a280-ab5f-11eb-98d7-da922df18873.png)


