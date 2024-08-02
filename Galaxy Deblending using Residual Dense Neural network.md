## citation

## notes

* why: wide field of view surveys (ex. Legacy Survey of Space and Time) will have difficulty using adaptive optics (ao)
* this causes blending - incorrect measurement and analysis 
* psf (point spread function) - function that maps how a single point of light is distorted
* problems with previous research: assumes galaxy in middle, fixed # of galaxies in image assumed
* the framework has two main components -> a **classifier** and a **deblender** 
	* the classifier finds out how many galaxies there are 
	* deblender gets image of just one galaxy (brightest)
	* remove that galaxy from the blend 
	* process repeats until no more galaxies 

![image](https://github.com/user-attachments/assets/c77bdebb-3929-47f2-bbf4-e85f8d2a6ef8)

deblender arch:
* two convolution layers for feature extraction 
* series of residual dense blocks, each containing a series of convolution layers with an ReLU activation function 
classifier: VGG-16 network 

data: synthetic images generated with BlendingToolKit w/ LSST DM catalog 

Source Extractor - industry standard for this kind of stuff
this model outperforms on flux recovery 

limitations/future work: variable psf in different bands, 

this is missing a lot of the details about the specifics of the model, i just wanted to get the tldr down 
## other research referenced (might be a good place to look)

D. M. Reiman and B. E. G¨ohre, Deblending galaxy superpositions with branched generative adversarial networks, Monthly Notices of the Royal Astronomical Society 485 (2019).

B. Arcelin, C. Doux, E. Aubourg, C. Roucelle, and LSST Dark Energy Science Collaboration, Deblending galaxies with variational autoencoders: A joint multiband, multi-instrument approach, MNRAS 500, 531 (2021), arXiv:2005.12039 [astro-ph.IM].

A. Boucaud, C. Heneka, E. E. Ishida, N. Sedaghat, R. S. de Souza, B. Moews, H. Dole, M. Castellano, E. Merlin, V. Roscani, et al., Photometry of high-redshift blended galaxies using deep learning, Monthly Notices of the Royal Astronomical Society 491, 2481 (2020).
