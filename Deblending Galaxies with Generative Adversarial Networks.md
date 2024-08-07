## citation 

## summary 

## notes

* dataset used (high resolution sample): Hubble Space Telescope (HST) Advance Camera for Survey (ACS) images from the Cosmic Assembly Near-infrared Deep Extragalactic Legacy Survey (CANDELS)
* takes inverse hyperbolic sine function of flux for use in training of 
* low res sample: degraded images similar to those taken by Subaru Hyper Suprime Cam (HSC)
* artificially blended sample: stacks galaxies on top of each other and degrading images 
* authors use Generative Adversarial Network 
	* **generator** tries to generate data/images while the **discriminator** tries to determine if images are real or fake
	* models trained simultaneously against each other 
* All GAN Frameworks in image below:
	* Framework 1: GAN model able to generate galaxy images from noise
	* Framework 2: Takes row res image and attempts to generate higher res images (single waveband)
	* Framework 3: Takes row res image and generates higher res images (multi-waveband)
* Used minimax loss function and Adam optimizer
* further research: other blends other than just 2 galaxies, more realistic dist. of angular distance, other depths 
![image](https://github.com/user-attachments/assets/35d1339a-e689-4fdc-8357-fb5ef18e6209)

![image](https://github.com/user-attachments/assets/227c6a4f-77f5-46e1-b35d-08d1ee64d0d5)

![image](https://github.com/user-attachments/assets/12da4a83-f37c-4d76-8097-490463ca00db)
## other research referenced (might be a good place to look)

Schawinski, K., Zhang, C., Zhang, H., Fowler, L., & Santhanam, G. K. 2017, MNRAS, 467, L110

Lanusse, F., Melchior, P., & Moolekamp, F. 2019, arXiv e-prints, arXiv:1912.03980

Arcelin, B., Doux, C., Aubourg, E., & Roucelle, C. 2020, MNRAS, arXiv:2005.12039

