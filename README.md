# FC-RNN-MRI-Recon
MR Image reconstruction based on Fully Connected Recurrent Neural Networks (FC-RNN)
## Introduction
### References
The implementation is based on the Python Deep Learning library: [Keras](https://keras.io/)
## Methods
### FC-RNN for MR Image Recon
Parallel imaging is a common approach to accelerate the MRI acquisition by exploiting the data redundancy in multiple coil scans. Several other strategies, like compressed sensing (CS), have been proposed to achieve more aggressive undersampling factors, and deep learning is also capable of obtaining prior knowledge with the help of training datasets for MR image reconstruction. In this work, we propose an auto-calibrated image reconstruction method from the k-space domain using a fully connected recurrent neural network.   

In GRAPPA, unacquired k-space data can be synthesized by a linear combination of acquired neighboring k-space data from all coils. Instead of using a linear operator, our work applies a non-linear mapping for estimation of unacquired k-space data. The training period would take two steps: pre-training and fine-tuning. We used a large dataset (volunteer scans from Siemens 3T Prisma scanner, with T1 vibe water excitation breath-hold sequence) for pre-training, and the fine-tuning was performed for each undersampled slice using the auto-calibration area. 
### Data Preprocessing
Before feeding k-space data into the deep learning network, the input data (a 5x5 block with coils) need to be reshaped into a 2D matrix, where the 2D matrix was organized by their distance from the data to be reconstructed (blue circle). The green circles, far from blue, were placed at the beginning of the 2D matrix, while the orange circles, close to blue, were placed at the end (as shown in figure below). To account for complex numbers, the k-space data were split into real and imaginary parts, and separate FC-RNNs were used for real and imaginary parts. Finally, a binary indicator was used for each k-space position, indicating whether it was acquired or not. After reorganization, all the input data and output data were be normalized into (0,1) for improved network performance.
![Alt text](/imgs/preprocessing.jpg)  
### Nerwork Architecture
Our proposed FC-RNN architecture is shown in the figure below, consisting of three dense units, an LSTM layer and an output dense unit.
![Alt text](/imgs/network.jpg)  
### Undersampling
Since our model is designed for arbitrary k-space undersampling patterns, both uniform and random ones are tested with our proposed method. The undersampling masks could be seen in the figure below, which includes a 3x uniform sampling and a 4x random sampling. The abdominal dataset was obtained fully-sampled with 14-channel coil and 256x208 matrix.
![Alt text](/imgs/nundersampling.jpg)  
## Results
In the uniform undersampling case, as shown in the first figure our proposed method produces shallower artifacts and has an edge in the error map compared to SPIRiT and  -SPIRiT. Moreover, different from the rough images produced by GRAPPA, zoom-in image shows FC-RNN result is capable of preserving fine structures of the original image. This kind of characteristic has made it appear more natural in detail, which could also be concluded from the SSIM(structural similarity) index 10. Referring now to the second one, although CS based methods naturally fits the random pattern quite well, our approach is able to produce a similar result without explicitly applying CS theory. Compared to SPIRiT and GRAPPA, the reconstructed image still restores the textures of the original image and has maintained it advantage in SSIM index.
![Alt text](/imgs/result1.jpg)  
![Alt text](/imgs/result2.jpg)  
## Future Releases
The code for this project may be released in the future.
