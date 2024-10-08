## citation
Y. Guo _et al._, “Adaptive optics based on machine learning: a review,” _OEA_, vol. 5, no. 7, pp. 200082–200082, 2022, doi: [10.29026/oea.2022.200082](https://doi.org/10.29026/oea.2022.200082).

## summary


## notes
### first pass: abstract and introduction
- adaptive optics are used in almost all ground-based high-resolution imaging telescopes with apertures larger than 1 meter
- challenging problems in AO:
	- how to get the wavefront of wavefront sensor-less AOS or array telescopes with high speed
	- how to estimate the wavefront along the line of sight to the scientific target in multi-object adaptive optics systems
	- how to reduce time-delay error in extreme AOSs
- ML and DL techniques can solve some of these problems
- supervised learning - training data contains inputs and outputs (labels)
	- deep learning - composed of multiple layers of neural networks. can be used in AO for:
		- determining the aberration from optically modulated images in WFS (wavefront sensing)
		- predicting future wavefront with historical multi-source information
		- **reconstructing the high-res images from noisy and blurred ones**
	- typical traditional algorithms used were:
		- least-square fit (LSF)
		- singular value decomposition (SVD)
		- Gauss-Seidel et.al
		However, these methods required many iterations and had weak fitting capabilities (underfit). DL, on the other hand, also contains the prior information in a network's structure and weights. This makes it more powerful.
- Early attempts for deblurring using ML occurred in the 1990s, where ANNs (artificial neural networks) were used to estimate the aberration of Hubble space telescopes and god almost same result of Fourier-based phase retrieval methods. DL has moved on fast since then and there are even better techniques now.
- Machine Learning-assisted Intelligent AO (IAO) is a developing field, summarized in this paper

### 2️⃣ second pass: detailed read
#### traditional methods
- An adaptive optics system includes:
	- deformable mirror (DM)
	- WFS (wavefront sensing)
	- real-time controller (RTC)
	- post-processing program
- WFS is a technique that provides a signal with which the shape of a wavefront can be estimated with sufficient accuracy --> does this in real-time

- WSFs are fast, but have a complex optical setup and require high-accuracy alignment
	- Also, they require certain conditions in order to perform well such as a continuous wavefront and uniform amplitude
- Wavefront reconstruction spatially filters the WFS measurements and recovers closed-loop wavefront error
- Wavefront control temporally filters the wavefront error and drives the DM
	- the most popular control in AO is proportional-integral-derivative (PID) algorithms 
		- this is not the best choice for AOSs because it performs poorly when correcting dynamic turbulence and narrow-bandwidth disturbances
	- Linear quadratic Gaussian (LQG) control based on Kalman filters are better control strategies. 
		- can obtain optimal correction (wrt residual phase variances) by performing optimal predictions of dynamic disturbance
		- This method quickly and consecutively builds the accurate dynamical model to track the time-varying dynamic disturbance
		- It is a linear prediction algorithm where any deviation from the estimated model would degrade the performance
- Spatial filtering operates on the spatial domain of an image, modifying pixel values based on the relationships between neighboring pixels within a specific spatial region, such as a 3x3 or 5x5 window. This type of filtering can be used for tasks like sharpening, blurring, edge detection, and noise reduction. Spatial filters are often implemented using convolution operations.
- Temporal filtering, on the other hand, operates on the time domain of a sequence of images, such as a video. It involves processing pixel values across multiple frames to extract information that is consistent over time, while suppressing variations that are inconsistent. Temporal filtering can be used for tasks like motion estimation, object tracking, and video stabilization. Techniques like frame averaging, median filtering, and Kalman filtering are examples of temporal filtering methods.

- Post-facto image reconstruction is necessary because the AOS can only partially correct the distortions in real-time

##### equations
$i(x) = o(x) * p(x)$
where $i(x)$ is the image of the astronomical object, 
$o(x)$ is the object intensity
$p(x)$ is the point-spread function of the whole atmosphere + telescope + instrument setup

To deconvolve the object intensity from the observed intensity, the PSF of the system is needed. 
- this is rarely known in actual systems; thus, blind deconvolution is proposed using only the prior assumption of the system to simultaneously obtain the object and the PSF
- Another method - phase diversity method (PD) (special case of the BD) --> with additional information on the phase (defocus of one image), the method can detect the residual wavefront simultaneously and can be used for wavefront sensing in the AOS.
- Another method - adding more information to the BD method (MFBD) - assumes that the observed object is not changed during the time used to capture the multiple image frames (solutions are more robust than the BD)
- statistical information of the turbulence can also be applied in the reconstruction of the astronomical object
All of these methods work well but are computationally expensive and can require a lot of iterations within the process.

#### intelligent adaptive optics
- SHWFS (Shack-Hartmann wavefront sensing) is the most widely used WFS in AOS because it has a simple structure, alignment, and is less computationally expensive than other methods
	- however, reconstructed result can be sensitive to noise. ML can be used for correction purposes
- ML based methods:
	- Improve the gradient-based method - build relationship between aberrations and gradients with nonlinear fitting tools like neural networks
	- Improve the spot centroid accuracy by doing the spot classification with neural networks before centroid calculation
	- Extract the aberration from the SHWFS image directly with deep learning instead of calculating the gradient
	- Deep learning can be used to solve the nonlinear phase retrieval problem without many iterations

#### how do Shack-Hartmann wavefront sensors work?
- wavefront is calculated from sub-aperture spot displacements using least-square fit or singular value decomposition
	- Instead SHWFS proposed to use neural network to reconstruct the wavefront from noisy spot displacements 
	- For training on noisy patterns, best network is three-layer feed forward back-propagation network with 90 neurons in the hidden layer
	- can also use a U-Net architecture to learn a mapping from SHWFS slopes to the true wavefront 
- to overcome the strong environment light and noise pollutions Li et al. proposed another method based on neural networks called SHWFS-Neural Network (SHNN) 
	- SHNNs firstly find out the spot center, then calculate the centroid, which transforms spot detection problems into a classification problem

- to avoid information loss, Suarez Gomez et al. proposed CNN as a reconstruction alternative in MOAO
	- this method relies on the use of a full image as input instead of only the centroids
	- this had promising results and opened up possibilities in further work in the topic:
		- improving the topology of the network,
		- setting more solid testing with sets of multilayer turbulence profiles
		- using optical measurements for the comparison of errors
	- calculated the slops of on-axis targets and used them to reconstruct the wavefront --> there are limitations to this method
		- DuBose et al. developed the Intensity/Slopes or ISNet, a deep convolution network using both the standard SHWFS slopes and the total intensity of each sub-aperture to reconstruct aberration phase. - provides superior reconstruction performance

- biological applications have found ways to apply deep learning to detect high-order aberration directly from SHWFS images without image segmentation or centroid positioning - Hu et al. proposed learning-based Shack-Hartmann wavefront sensor (LSHWS) 
- Hu et al. also proposed another deep learning assisted SHWFS called SH-Net which can directly predict the wavefront distribution without slope measurements or Zernike modes calculations - has the highest accuracy among the five deep learning-based methods (detection speed is lower but accuracy is much higher)

#### phase diversity wavefront sensor
- image-based wavefront sensing is a method that does not need additional optical components; it uses parameterized physical model and nonlinear optimization
- PD WFS is such a type of image-based WFS
- Kendrick et al. used general regression neural network to calculate wavefront errors from focused and defocused images in 1994
- Recently, many new methods based on convolutional neural networks for wavefront detection have been proposed
- **Guo et al. successfully used an improved CNN to establish the non-linear mapping between focal/defocused PSFs and the corresponding phase maps
	- **uses model called De-VGG --> adds three deconvolution layers on the basis of the well-known visual geometry group (VGG) network**
- Ma et al. proposed similar PD wavefront sensing method
	- the AlexNet is used to extract the features from the focal and defocused intensity images and obtain the corresponding Zernike coefficients - results are not clear because no real data was used to investigate the accuracy
- Wu et al. proposed a real-time non-iterative phase-diversity wavefront sensing based CNN that achieved sub-millisecond phase retrieval
	- uses NVIDIA TensorRT (SDK for high-performance deep learning inference)
	- reduces aberration measurement error by fusing the focal and defocused intensity images
	- there is some accuracy loss after inference acceleration

- Nishizaki et al. proposed a variety of image-based wavefront sensing architectures named deep learning WFS (DLWFS) that can directly estimate aberration from single intensity image using Xception 
	- preconditioners include overexposure, defocus, and scatter 

#### time delay
- AOS suffers 2-3 frames time delay
- traditional control methods don't consider this wavefront distortion change from measurement to correction so there is a so-called time-delay error
- one way to mitigate this error is by predicting the future wavefront with several previous frames
- First demonstrated by Jorgenson and Aitken
- LMMSE (linear minimum mean square error) based on statistical knowledge of the atmosphere, noise, etc. were investigated
	- however, this was unsuccessful because accurate a priori knowledge about atmosphere turbulence and noise cannot be obtained easily in real systems especially for non-stationary turbulences
		- to solve this problem, we can estimate the statistical properties in quasi real-time
		- another solution is to train the predictor with big data to adapt the variation of the turbulence (machine learning-based predictor)
- Recurrent neural networks having dynamic feedback connections and sharing parameters across all time steps are better for sequence data processing tasks like wavefront prediction

#### deep learning
- Ramos et al. proposed using CNN where input was frames and the output was corrected frames
- To process a larger number of frames, the deep NN is used in batches until all frames are exhausted
- Another approach is to use a DNN with some type of recurrence so that frames are processed 
	- New frames are injected into the network and corrected frames emerge from the output. Introducing new frames on the input will slowly improve the quality of the output
	- this procedure is iterated until a good enough final image is obtained.
	- Shi et al. proposed end-to-end blind restoration method for ground-based space target images based on conditional generative adversarial networks (cGAN) without estimating PSF - very good option (clear contours and details, faster than traditional methods; results good mostly due to improved loss function)
- DL methods only work when the data is identically distributed, which is difficult given random atmospherical conditions

#### deep reinforcement learning methods
- Hu proposed a self-learning control framework for WFS-less AOS through deep RL. 
- Aberration correction process is a Markov decision process
- Xu proposed a DL control model (DLCM) to cope with the disturbance of slope response matrix and improve the adaptive ability of AO control system

### conclusion
- ANN can be used to build the relationship between the measurement (image) and the wavefront for wavefront reconstruction
- features extracted from data may perform better than the manually selected linear ones
- some iterative methods for phase retrieval may be replaced by deep learning for faster speed
- ANNs can also be used to do the nonlinear wavefront prediction for better accuracy and improved time-delay
- Data is super important for this purpose - some data can be generated by the computer for training but its adaptability to real systems needs to be demonstrated
- unsupervised learning and reinforcement learning are future treasure troves for more ideas and progress in the field

NOTE: references include useful resources.
