## citation
S. VY, S. Sen, and K. Santosh, “Analyzing and Processing of Astronomical Images using Deep Learning Techniques,” in 2021 IEEE International Conference on Electronics, Computing and Communication Technologies (CONECCT), Jul. 2021, pp. 01–06. doi: 10.1109/CONECCT52877.2021.9622583.

## summary
Degenerated into a survey paper and was extremely high-level/amateur (ex., got their data from a kaggle challenge). Concluded that AlexNet and custom CNNs perform about the same. 
They did not compare different deep learning models as they said they would, spent too much time talking about data preprocessing (only got rid of null values). 
Overall not really worth a read but they have good information on intro to CNNs.

## notes
SOM-CNN model (self-organizing map) is superior to the back propagation model
- Combination of CNN and K-nearest neighbors is superior to locally-weighted regression and feed-forward NN (most basic NN)

#### CNN intro
- Unlike other Multi-Layer Perceptrons, CNN doesn't lose any spatial information of images when flattened
- With filters, CNN examines the impact of adjacent neighboring pixels
- Filter is assigned a user-specified size and pushed across the image from top left to bottom right with a convolution operation determining the corresponding value for each point on the image
- CNN
	- convolutional layer: filters are convolved over the original image or to other feature maps; it slides the filter over the smaller image pixels of the galaxy image
	- pooling layer: this layer decreases the network's dimensionality by taking the maximum or average values from feature maps. 
	- fully-connected layer: gathers the output provided by the convolutional and pooling layers and applies logic to determine what image it is

