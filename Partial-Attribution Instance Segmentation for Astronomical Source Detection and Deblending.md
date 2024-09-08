# citation
[1] R. Hausen and B. Robertson, “Partial-Attribution Instance Segmentation for Astronomical Source Detection and Deblending,” Jan. 12, 2022, arXiv: arXiv:2201.04714. doi: 10.48550/arXiv.2201.04714.

# summary
Focused on improving image segmentation techniques to better identify sources when deblending. 

# notes

## abstract + intro
- astronomical imaging data can challenge off-the-shelf computer vision algorithms owing to its high dynamic range, low signal-to-noise ratio, and unconventional image format
- source deblending is an open area of astronomical research
- this paper introduces a new approach called Partial-Attribution Instance Segmentation that enables source detection and deblending for deep learning models
- Novel neural network implementation as demonstration of the method

- astronomical images can contains tens of thousands of stars and galaxies (sources)
- sources of data:
	- Vera Rubin Observatory
	- James Webb Space Telescope
	- Nancy Grace Roman Space Telescope
- must (1) detect sources by identifying statistically significant local maxima in an image and (2) deblend those sources by isolating potentially overlapping flux distributions of each object
- the core challenge of deblending is the process of deconstructing an image into a set of individual sources 

### previous work
- detection and deblending methods are characterized by their detection capacity and deblend type
	- capacity represents number of sources a method can detect within a single image, with N meaning the method can detect all sources
	- deblend type indicates how the flux in a single pixel may be split between overlapping/blended sources
- disjoint deblender assigns all flux in a pixel to a single source exclusively
- intersecting/discrete deblender can assign the flux to more than one source with uniform weighting across pixels 
- intersecting/continuous deblender can assign the flux to more than one source with variable weighting across pixels --> ideal; this is what the paper is proposing
- Astronomical analysis methods vary in their detection and deblending methods. 
	- Bertin & Arnouts [2] introduced SExtractor that uses a convolution and thresholding approach for detection, and an isophotal analysis using binned pixel intensity for deblending.
	- Hausen & Robertson [7] introduced Morpheus, a U-Net [23] style convolutional neural network (CNN) model that filters out background pixels, uses a thresholding approach for detection, and combines watershed and peak finding algorithms for deblending. 
	- Another U-Net based model called blend2mask [3] performs detection and deblending using the U-Net alone. 
	- Reiman & Göhre [22] use a modified Super-Resolution GAN (SRGAN) [16] to deblend overlapping sources. 
	- Burke et al. [4] trained a Mask R-CNN [8] model to detect and deblend sources.
	- SCARLET [19] deblends sources using constrained matrix factorization
- ^ None of these have an intersecting/continuous deblending algorithm with a detection capacity of N. This paper does.

## PAIS
- new extension of the image segmentation paradigm that allows for weighed, overlapping segmentation maps
	- image segmentation involves classifying each pixel/voxel of a given image/volume to a particular class along with assigning a unique identity to pixels/voxels of individual objects. There are many other segmentation schemes:
		- cell segmentation
		- interacting surface segmentation
		- amodal instance segmentation
- PAIS differs from other segmentation schemes because it isolates objects appearing in an image while preserving their measurable quantities within areas of overlap
- for PAIS, we can approximate the segmentation as:
![Pasted image 20240829163911](https://github.com/user-attachments/assets/3d61d9dd-bd7b-4cb2-b164-8068ade42072)

- I is the background-subtracted flux image
- M_i, jkl = 1 constitutes the pixel-level fractional contribution of source i to I and the circle symbolizes the Hadamard product
- This is tractable for deep learning models, allowing the model to learn the bounded quantities M_i rather than the unbounded source images S_i
- PAIS format should be represented by a CNN
	- construct an encoding for the M_i in Equation 2
	- Encoding is Partial Contribution Representation
The goal of PCR is to encode for any single pixel j, k, l the fractional contribution to its intensity from the closest n sources
Using PCR, a variable number N of sources can be encoded per image. 

PCR consists of three tensors:
(1) Center of mass --> encodes locations of all the sources in an image (per pixel of a source --> 1 if center of mass, 0 if not)
(2) Contribution vectors --> encodes Cartesian offset to the closest n sources
(3) Contribution maps --> connects the fractional contribution of the n sources with the associated contribution vectors

All three tensors are fixed dimensions, making PCR tractable for deep learning algorithms.

## the approach
- make a PAIS dataset using PCR and implement using a novel neural network architecture
- PAIS input samples (dataset) --> Hubble Legacy Fields (GOODS-South F160W) flux images along with 3D-HST source catalog
- HLF images were split into training and tests sets of pixel subregions (not images! maybe because they didn't have enough images?)
- input labels = center of mass, contribution vectors, and contribution maps
- Center of mass training images are generated by placing pixelated 2D Gaussians with standard deviation of 8 pixels at the locations of sources in the 3D-HST catalog
- The contribution vectors are generated by the Cartesian offset to the nearest n = 3 sources to each pixel
- Contribution maps require the Mi values from the above equation
- M_i is determined using SCARLET with F125W, F160W, F606W, and F850LP flux and weight images from the HLF GOODS-South data and the TinyTim point-spread functions to deblend the sources from the 3D HST catalog
- Use PCR to encode the M_i from SCARLET
- The complete dataset generation routine found in github https://github.com/ryanhausen/morpheus-deblend/
- to evaluate efficacy of PCR to encode M_i, define two metrics: use mean difference between total flux determined by SCARLET for each input source and that recovered by our encoding; also use two sample Kolmogorov-Smirnov test to compare the normalized cumulative surface brightness profile within the radius encompassing 90% of the total flux of each source to evaluate the encoding of the spatial flux distribution
- PCR encoding approximately preserves both the total flux and the spatial flux distribution for each source
- With this verification, we can train a network to recover the PCR encoding for each input image

## model
- to recover the PCR for training images, developed a novel neural network architecture inspired by Cheng et al. based on the Fast Attention Network, implemented in TensorFlow
- The model features two decoders that share a single encoder
	- spatial decoder --> predicts values for Center of mass and contribution vectors
	- attribution decoder --> predicts values for contribution maps
So the model is used to figure out what the three tensors are (PCR). The PCR decoder is then used to actually deblend the sources

### how does the model recover PCR of the input images?
- to train the model, use the Adam Optimizer
- Model was trained for 1000 epochs using NVIDIA V100 32GB GPU
- Spatial decoder outputs for center of mass and contribution vectors; is penalized according to mean squared error and mean absolute error, respectively
- attribution decoder, which outputs contribution map, is penalized using cross-entropy loss with additional entropy regularization term
	- the additional regularization helped incentivize the network to learn information about multiple sources in C_m. 
- Each loss term is weighted and combined into a single loss function described by 
![Pasted image 20240902214303](https://github.com/user-attachments/assets/f79be6d7-31e2-49d9-b407-7ac9f1cd367b)
