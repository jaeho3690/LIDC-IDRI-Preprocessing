# LIDC Preprocessing with Pylidc library
[Medium Link](https://medium.com/@jaeho3690/how-to-start-your-very-first-lung-cancer-detection-project-using-python-part-1-3ab490964aae)

This repository would preprocess the LIDC-IDRI dataset. We use pylidc library to save nodule images into an .npy file format.
The code file structure is as below

```
+-- LIDC-IDRI
|    # This file should contain the original LIDC dataset
+-- data
|    # This file contains the preprocessed data
|   |-- _Clean
|       +-- Image
|       +-- Mask
|   |-- Image
|       +-- LIDC-IDRI-0001
|       +-- LIDC-IDRI-0002
|       +-- ...
|   |-- Mask
|       +-- LIDC-IDRI-0001
|       +-- LIDC-IDRI-0002
|       +-- ...
|   |-- Meta
|       +-- meta.csv
+-- figures
|    # Save figures here
+-- notebook
|    # This notebook file edits the meta.csv file to make indexing easier
+-- config_file_create.py
|    # Creates configuration file. You can edit the hyperparameters of the Pylidc library here
+-- prepare_dataset.py
|    # Run this file to preprocess the LIDC-IDRI dicom files. Results would be saved in the data folder
+-- utils.py
     # Utility script

```
![Segmented Image](/figures/output_segment.png)
## 1.Download LIDC-IDRI dataset
First you would have to download the whole LIDC-IDRI [dataset](https://wiki.cancerimagingarchive.net/display/Public/LIDC-IDRI).
On the website, you will see the Data Acess section. You would need to click Search button to specify the images modality.
I clicked on CT only and downloaded total of 1010 patients.

## 2. Set up pylidc library
You would need to set up the pylidc library for preprocessing. There is an instruction in the [documentation](https://pylidc.github.io/install.html).
Make sure to create the configuration file as stated in the instruction. Right now I am using library version 0.2.1

## 3. Explanation for each python file
```bash
python config_file_create.py
```
This python script contains the configuration setting for the directories. Change the directories settings to where you want to save your output files. Without modification, it will automatically save the preprocessed file in the data folder.
Running this script will create a configuration file 'lung.conf'

This utils.py script contains function to segment the lung. Segmenting the lung and nodule are two different things. Segmenting the lung leaves the lung region only, while segmenting the nodule is finding prosepctive lung nodule regions in the lung. Don't get confused. 

```bash
python prepare_dataset.py
```
This python script will create the image, mask files and save them to the data folder. The script will also create a meta_info.csv file containing information about whether the nodule is
cancerous. In the LIDC Dataset, each nodule is annotated at a maximum of 4 doctors. Each doctors have annotated the malignancy of each nodule in the scale of 1 to 5. 
I have chosed the median high label for each nodule as the final malignancy. The meta_csv data contains all the information and will be used later in the classification stage.
This prepare_dataset.py looks for the lung.conf file. The configuration file should be in the same directory. Running this script will output .npy files for each slice with a size of 512*512

To make a train/ val/ test split run the jupyter file in notebook folder. This will create an additional clean_meta.csv, meta.csv containing information about the nodules, train/val/test split.

A nodule may contain several slices of images. Some researches have taken each of these slices indpendent from one another. 
However, I believe that these image slices should not be seen as independent from adjacent slice image. 
Thus, I have tried to maintain a same set of nodule images to be included in the same split. Although this apporach reduces the accuracy of test results, it seems to be the honest approach.



## 4. Data folder
the data folder stores all the output images,masks.
inside the data folder there are 3 subfolders. 

### 1. Clean

The Clean folder contains two subfolders. Image and Mask folders.
Some patients don't have nodules. In the actual implementation, a person will have more slices of image without a nodule. To evaluate our generalization on real world application, we save lung images without nodules for testing purpose.
These images will be used in the test set.

### 2. Image

The Image folder contains the segmented lung .npy folders for each patient's folder

### 3. Mask

The Mask folder contains the mask files for the nodule.

### 4. Meta

The Meta folder contains the meta.csv file. The csv file contains information of each slice of image: Malignancy, whether the image should be used in train/val/test for the whole process, etc.


## 5. Contributing and Acknowledgement
I started this Lung cancer detection project a year ago. I was really a newbie to python. I didn't even understand what a directory setting is at the time! However, I had to complete this project
for some personal reasons. I looked through google and other githubs. But most of them were too hard to understand and the code itself lacked information. I hope my codes here could help
other researchers first starting to do lung cancer detection projects. Please give a star if you found this repository useful.

here is the link of github where I learned a lot from. Some of the codes are sourced from below.
1. https://github.com/mikejhuang/LungNoduleDetectionClassification

