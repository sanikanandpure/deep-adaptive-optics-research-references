## citation
F. Sureal _et al._, “Deep learning for a space-variant deconvolution in galaxy surveys”, 2019

## summary
The study uses simulations of realistic galaxy images derived from HST observations, with realistically sampled sparse-variant PSFs and noise, processed with the GalSim simulation code.

## notes
- deconvolution of images require consideration to space-variant point spread function
- deep learning 
- U-net neural networks where a U-shape is created using down and up-sampling (Simply put, it analyzes the image by small pieces -
zooming into tiny details / pixels (down sampling) and then pieces the details together (upsampling).
- 1st approach is a post-processing of a mere Tikhonov deconvolution with closed-form solution
- 2nd approach is an iterative deconvolution framework based on the alternating direction method of multipliers (ADMM).
- these two techniques outperform standard techniques
-- Tikhonov approach yields highly accurate resutls except for ellipticity errors at high signal-to-noise ratio, where the ADMM approach performs slightly better
- Tikhonov is more efficient.

### key points
- PSF (point spread function) ( The PSF field is usually estimated beforehand through parametric models and simulations as in Krist _et al._ (2011) or is directly estimated from
the (noisy) observations of stars in the field of view )
- Similar to the U-net approach by Vojtekova et. al except they used (Convolutional neural networks) while this use a deep neural network. 
- The proposed deep-learning approaches outperform standard techniques, with the Tikhonet approach achieving an improvement of about 14% in terms of the median pixel error and an improvement of about 13% 
for the median shape measurement errors compared to sparse recovery. We evaluated these approaches compared to the deconvolution techniques in Farrens et al (2017) in simulations of realistic galaxy images 
derived from HST observations, with realistically sampled sparse-variant PSFs and noise, processed with the GalSim simulation code. The situation is more balanced for the ADMMnet, where smaller pixel errors 
can be achieved at low signal-to-noise ratio (S/N) with the classical architecture, but the XDense U-net provides the best results for pixel errors at high S/N and ellipticity errors at high and low S/N
The two methods visually outperform the sparse recovery and low-rank techniques, which display artifacts at the low S/N we probed. This is confirmed in all S/N ranges and for a realistic distribution of S/N. 
In the latter, an improvement of about 14% is achieved in terms of the median pixel error and an improvement of about 13% for the median shape measurement errors for the Tikhonet approach compared to sparse recovery
At higher S/N, the ADMMnet approach leads to slightly smaller ellipticity errors. 

###  method and implementation
- The first method uses a deep neural network
(DNN) to post-process a Tikhonov deconvolution, and the second includes a DNN that is trained to denoise in an iterative
algorithm derived from convex optimization
- Tikhonet Approach:
Combines Tikhonov deconvolution with a DNN.
Acts as a post-processing step.
Tikhonov hyperparameter for this approach. 
- ADMMnet Approach:
Utilizes a DNN denoiser within an iterative ADMM Plug-and-Play (PnP) algorithm.
Incorporates regularization via DNN.
continuation hyperparameters for ADMMnet.
DNN Architecture:
- Compared with deconvolution techniques from Farrens et al. (2017).
Used simulations of realistic galaxy images (HST observations).
Considered sparse-variant PSFs and noise, simulated with GalSim.
- Modifications:
-- Dense blocks of separable convolutions.
Removal of skip connections.
Reduced number of parameters compared to standard U-net.

### deep learning approach
Key Features for Network Architecture:
- Translation Equivariance: The network needs to be translation equivariant to properly handle the forward model and task. This means the network should treat the same features consistently, regardless of their location in the image.
- Multiscale Processing: The network should capture distant correlations in the image, which requires multiscale processing.
- Parameter Efficiency: The network should minimize the number of trainable parameters to be efficient and facilitate learning, reducing GPU memory consumption.
##### Chosen Neural Network Architecture:
Global U-net structure, inspired by previous successful applications of U-net in solving inverse problems. They made several modifications to the classical U-net to enhance performance:
- Separable Convolutions: They replaced standard 2D convolutions with 2D separable convolutions, which reduce the number of parameters by independently capturing spatial correlations and correlations across feature maps.
- Dense Blocks: These were used at each scale to propagate all prior feature maps to the input of the current layer through concatenation, enabling feature reuse, preserving information, and reducing vanishing gradients.
- Average-Pooling: Instead of maximum-pooling, which led to oversegmentation, they used average-pooling to improve the final estimates.
- No Skip Connection: They removed the skip connection between the input and output layers, which was found to be detrimental to performance, especially at low signal-to-noise ratios. The dense blocks helped preserve the flow of relevant information, 
reducing the need for residual learning.
#### How It Works:
- Learning Features: The DNN learns features from existing galaxy image data in a supervised manner, improving the deconvolution process.
- Translation Equivariance: Convolutional layers ensure that the network treats the same features consistently, regardless of their position in the image.
- Multiscale Processing: The U-net structure and dense blocks allow the network to capture correlations at multiple scales, enhancing its ability to process complex images.
- Efficiency: The use of separable convolutions and dense blocks reduces the number of parameters, making the network efficient in terms of memory and computational resources.

## image generation to train networks
- Used GalSim3 to generate realistic to generate realistic
images of galaxies to train networks and test deconvolution approaches. 
- We essentially followed the approach used
in GREAT3 (Mandelbaum et al. 2014) to generate the realistic
space branch from high-resolution HST images, but chose the
PSFs in a set of 600 Euclid-like PSFs. 
- We then applied random shift (taken from
a uniform distribution in [−1, 1] pixel), rotation, and shear. The
same cut in S/N was performed as in GREAT3 (Mandelbaum
et al. 2014) to obtain a realistic set of galaxies that would be
observed in an S/N range [20, 100] when the noise level is the
same as in GREAT3. We here used the same definition of the
S/N as in this challenge:
- Process was repeated (210 000 simulated observed galaxies and their corresponding
targets). 
- For the learning, 190 000 galaxies were employed, and
for the validation set, 10 000 galaxies. The additional 10 000
galaxies were used to test approaches.

### sample approached logic for the DNN training using the Tikhonov approach
![image]((https://github.com/sanikanandpure/deep-adaptive-optics-research-references/blob/39ac18304226b107f9d0dbef04c67a780bb2ba97/sample%20logic.PNG))

### example results from the approaches: 
![image](https://www.aanda.org/articles/aa/full_html/2020/09/aa37039-19/F13.html)
- Deconvolved images with the various approaches for S/N 20. Each row corresponds to a different processed galaxy. From left to right: image to recover, observation with a noise realization, sparse and low-rank approaches, and the Tikhonet and ADMMnet results.

### conclusion
- 2 new space-variant deconvolution strategies for galaxy images based on deep neural networks 
- the Tikhonet approach is a post-processing approach of a simple Tikhonov deconvolution with a DNN, 
- the ADMMnet approach is based on regularization by a DNN denoiser inside an iterative ADMM PnP algorithm for deconvolution.

### possible improvements:
- ADMMnet approach can further be improved, additional constraints could be added easily to the framework

### useful resources we can use: (as used by this article)
https://github.com/pmelchior/shapelens 
https://github.com/sfarrens/sf_deconvolve 
https://github.com/GalSim-developers/GalSim  --- simulation code and potential database
