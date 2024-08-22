## citation
C. Burke _et al._, “Deblending and classifying astronomical sources with Mask R-CNN deep learning”, 2019

## summary
Deep learning technique using Mask Region-based Convolutional Neural Network (Mask R-CNN) to detect, classify, and deblend sources in multiband astronomical images, achieving high precision and recall rates for star and galaxy classes. Mask R-CNN algorithm is designed to identify each individual components in the image, classify, and essentially "assume" whether it is background objects, galaxies or stars, and combine them all into one hollistic pictures based on the identification of galaxies + stars. This model is trained using Using Photon Simulator (PhoSim - works by generating a catalogue from a three-dimensional space), we generate simulated images and catalogues used as a ground truth comparison for supervised machine learning and then images from Dark Energy Camera (DECam) images for actual classification testing. 

## notes

### accuracies and issues
- Precision of 92% at 80% recall for stars and a precision of 98% at 80% recall for galaxies in a typical field with ∼30 galaxies arcmin−2 with a minimum detection confidence threshold of 0.5
- The network is not sensitive to the number of density of sources in the image.
- Many classification and deblending codes operate monochromatically, not taking into account source colour gradient information
- Because of the region-based nature of the Mask R-CNN neural network, the results are insensitive to the density of sources in the image may be used at different galactic latitudes
- Insensitive to configuration parameters, such as the density of sources in the field, PSF size, and exposure time. Instead, a large and diverse set of training images and additional data augmentations is needed.
- Issues arise if the real images do not mimic the simulated images
- Near the galactic centre of the Milky Way, or images of large/extended galaxies may be challenging regimes.
- Not train for detections on sources that are completely obscured by another object
- It can be hard to correct for the systematic differences between simulated and real images

###  method and implementation
- The authors use PhoSim to generate a large, uniform, and realistic training set of DECam images, with approximately 150,000 astronomical sources.

### deep learning approach
- Mask R-CNN is a deep learning model that detects objects in images, classifies them, and precisely segments them by outlining their shapes.
- It works by first scanning the image to propose regions where objects might be, then refining these regions to accurately identify and classify the objects within them. The model also creates detailed masks that highlight the exact boundaries of each object.
- Used to identify and map galaxies in images of the night sky by detecting and segmenting them from the background, allowing for precise analysis of their shapes and positions.

### conclusion
- The authors achieve high precision and recall rates for star and galaxy classes, and demonstrate the robustness of their technique in handling clean deblends during object masking, even for significantly blended sources.
- Runs in 100 ms or less depending on GPU used

### possible improvements:
- Use this work as an example to apply even newer and rapidly advancing techniques from the computer vision community to astronomy
- explore masking additional features such as cosmic rays and bleed trails.
- explore different network architectures for instance segmentation, such as the popular U-Net (U-Net may be better-suited to full-scale astronomical images because it does not create regions around each object, which can lead to inefficiencies in the training. The down-side is that U-Net can only inherently perform semantic segmentation (i.e. not distinguishing masks of individual objects of the same class). Therefore, these methods must eventually fall-back on traditional techniques to handle mask occlusion, and may suffer from the same issues current pipelines face in crowded fields.)
- Explore (a) executing and tiling results on a full-scale data set with images taken under a variety of conditions, such as DES or HSC deep coadd images; (b) predicting probability contours for objects by training using different mask sizes or IOU cut-offs, add photometric redshift prediction, adding additional galaxy classes in place of the generic ‘galaxy’ classification, such as spiral, elliptical, irregular, and lensed; and (c) implementing a more robust interface for training and configurations. Mask R-CNN can also be used on video for real-time instance segmentation, which may have interesting applications for LSST and time-domain astronomy.


### resources 
Public code: https://github.com/burke86/astro_rcnn
