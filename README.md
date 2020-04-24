# LIDC Preprocessing with Pylidc library
This is the preprocessing step of the LIDC-IDRI dataset. We use pylidc library to save nodule images into an .npy file format.
The code file structure is as below

```
+-- config_file_create.py
+-- utils.py
+-- _data
|   +-- _Clean
|       +-- Image
|       +-- Mask
|   +-- Image
|       +-- LIDC-IDRI-0001
|       +-- '''
|   +-- Mask
|       +-- LIDC-IDRI-0001
|       +-- '''
+-- prepare_dataset.py
```

## 1.Download LIDC-IDRI dataset
First you would have to download the whole LIDC-IDRI [dataset](https://wiki.cancerimagingarchive.net/display/Public/LIDC-IDRI)
When you go through the website, you will see the Data Acess section. You would need to click Search button to specify the images modality.
I clicked on CT only and downloaded total of 1010 patients.

## 2. Set up pylidc library
You would need to set up the pylidc library for preprocessing. There is an instruction in the [documentation](https://pylidc.github.io/install.html)
make sure to create the configuration file as stated in the instruction

## 3. Explanation for each python file
1. config_file_create.py
This python script contains the configuration setting for the directories. Change the directories settings to where you want to save your output files.
This will create a configuration file 'lung.conf'

1. utils.py
This python script contains function to segment the lung. Segmenting the lung and nodule are two different thing. Don't get confused. 

1. prepare_dataset.py
This python script will create the image,mask files and save them to the data folder. The script will also create a meta_info.csv containing information about whether the nodule is
cancerous. In the Lidc Dataset, each nodule is annotated at a maximum of 4 doctors. Each doctors have annotated the malignancy of each nodule in the scale of 1 to 5. 
I have chosed the median high label for each nodule as the final malignancy. The meta_csv data contains all the information and will be used later in the classification stage.
This prepare_dataset.py looks for the lung.conf file. The configuration file should be in the same directory.


## 4. Data folder
the data folder stores all the output images,masks.
inside the data folder there are 3 subfolders. 
1. Clean
The Clean folder contains two subfolders. Image and Mask folders.
Some patients don't have nodules. In the actual implementation, a person will have more slices of image without a nodule. To consider this, we save lung images without nodules.
These images will be used in the test set 

1. Image
The Image folder contains the segmented lung .npy folders for each patient's folder

1. Mask
The Mask folder contains the mask files for the nodule.
