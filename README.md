# Bioimage-NECSTNL1
## Description

This project was the end assigment for the "Biomedical Computer Vision Course" at NecstCamp. It is based on the [Kits19 Challenge](https://github.com/neheller/kits19)

## First analysis

I first analysed the dataset and realised that the images are 512x512 in size, with the exception of patient 160, and that the number of patient *slices* is not uniform. The masks were available for the first 209 patients, which would ideally constitute the amount of data needed for training and validation, with the remaining cases dedicated to testing. 

However, due to hardware limitations of the free version of Colab, I limited myself to only using the first 100 patients and a subset of slices, arbitrarily choosing for resampling the minimum present in the set, i.e. 29 for case 61, scaled to 256x256.

## Binary Segmentation

In the first code `binary_segmentation.ipynb`, I perform a binary segmentation, removing the distinction between kidney and tumour in the masks, by setting all values other than background equal to 1. In this case the dataset and the neural network, implemented following the [uNet](https://arxiv.org/abs/1505.04597) model, proved to be sufficient to segment the kidneys from the original CTs.

## Multiclass Segmentation

In the second notebook `multiclass_segmentation.ipynb` I try to segment tumours and kidneys. Firstly, I modified the CT images, so that they have three channels, and, secondly, transformed the masks using `one_hot_encoding` to have three indexes, each for each class (background, kidney and tumour); in this way I could do multiclass segmentation and add specific weights, based on the percentage each class occupied in the mask. In this case, the dataset proved to be sufficient in detecting kidney but insufficient in detecting tumours due to the size of the dataset. A solution to this could be increasing the number of patients for training/the number of slices per patient, or even doing some data augmentation, by mirroring the image or rotating it, but I wasn't able to test it due to the hardware limitations mentioned above.

In this solution, I use the [`efficientnetb3`](https://arxiv.org/pdf/1905.11946.pdf) from the [segmentation_model](https://segmentation-models.readthedocs.io/en/latest/) library for the neural network.
