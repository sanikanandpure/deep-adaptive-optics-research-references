## citation 

## summary 



## notes

* pretty similar to last paper I looked at (both look at GANs for deblending), but I think this one is more applicable to what we want to do 
* This time the generator is being used for deblending while the discriminator evaluates how accurate the deblender is  
* Generator architecture uses a residual neural network (similar to the first paper I looked at), and discriminator uses a CNN 
* The loss functions has 2 parts: adversarial loss (based on the discriminator - does it look like a galaxy) and content loss (does it look like the actual galaxies from the original image)
* data: images used from Galaxy Zoo, blended with each other by selecting max intensity in each image (one of the galaxies is always centered while the other one is transformed)
* potential future work: *ambiguous blends* (this one seems like an interesting area), other methods of making "blended" data, multiband input, different redshifts

![[Pasted image 20240814183116.png]]

![[Pasted image 20240814183125.png]]