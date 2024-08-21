## citation
B. Arcelin _et al._, “Deblending galaxies with variational autoencoders”, 2021

## summary
Method using variational autoencoders (VAE) to deblend galaxy images, achieving low biases and errors in shape parameters and magnitudes, and discuss the potential applications and limitations of this approach using galaxy images from Legacy Survey of Space and Time (LSST) and the Euclid satellite and generated images using GalSim (https://github.com/GalSim-developers/GalSim) software and based on a public catalogue from observations of the COSMOS field by the Hubble Space Telescope. The main goal was to develop a method to deblend galaxy images and evaluate its performance in terms of statistical reconstruction errors on ellipticities and magnitudes. 

## notes

###  method and implementation
-  Two VAE-like neural networks: one to learn a generative model of isolated galaxy images, and another to perform deblending. They train the networks on simulated images and evaluate their performance using reconstruction metrics.
-  Two-step approach, where the first step involves training a VAE to learn features of galaxy images, and the second step involves training another network to perform deblending in a latent space. The study also uses simulated images and evaluates the performance of the deblender network in terms of statistical reconstruction errors on ellipticities and magnitudes.
-  Distribution of measured errors: signal-to-noise ratio (SNR) and blendedness metrics

### deep learning approach
- VAEs parametrized by CNNs (Convolutional Neural Network) provide powerful models able to learn features of galaxy images from a noisy sample with sufficient accuracy to recover ellipticities and magnitudes. The deblender network is able to isolate galaxies and recover their properties with low biases, and the method is robust to pixelization-related and/or modest decentring.

### conclusion
- Good approach for deblending galaxy images, achieving low biases and errors in shape parameters and magnitudes.
- Limitations include:need for accurate centroid localization and the potential impact of decentring on the results & applying this method to real data. + Effective PSF on a stacked image not being exactly the median of the distribution, and the fixed stamp size chosen to work with simple network architectures. Method may degrade with larger decentring errors caused by the associated peak detection algorithm.

### possible improvements:
- Applying their method to real data, improving the detection pipeline, and exploring other applications of VAE in weak-lensing measurement pipelines
- Exploring the use of transfer learning techniques
- Replacing the decoder by a simpler network that would parametrize the joint likelihood of the ellipticities and magnitude, and using multiple stamp sizes for each instrument

### resources 
Public code: https://github.com/burke86/astro_rcnn
