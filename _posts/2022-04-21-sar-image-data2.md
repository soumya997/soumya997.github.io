---
layout: post
title: SAR-cus! Tricks that you wanna learn - 102
subtitle: SAR Image Data 102 Gonna discuss all bout EM Wave Basics, need of SAR data, from pros and cons, data collection, more details.
cover-img: /assets/img/dataset-cover.jpg
thumbnail-img: https://spaceplace.nasa.gov/review/build-a-spacecraft/generic-sat.en.png
share-img: https://spaceplace.nasa.gov/review/build-a-spacecraft/generic-sat.en.png
tags: [SAR, remote sensing]
---


# complex form of a sar image:
Physical properties of the terrain change the phase and amplitude of the EM wave. And SAR is used to calculate these two properties of a wave, which are amplitude(A) and phase(phi). It is represented as a number pair, (A cos(shi), Asin(shi)). How bright or dark the pixels are depended upon the strength of eco received. This amplitude and phase information is captured in the form of a complex number. Every complex number consists of a real part and an imaginary part. Here,
 Real $part(a) = A \cos \phi$, Imaginary $part(b) = A \sin(\phi)$

Each pixel value equals to a+ib, where shi = $\tan^-1(a/b)$
and the amplitude is $\sqrt(a^2+b^2)$

Intensity ($I$) = $A^{2}$, logarithmic Intensity = $log(I)$



## Radar Polarization:
The emitted EM wave signal can be polarized and unpolarized. By unpolarized I mean, the unpolarized energy vibrates in all possible directions to the direction of travel. But polarized signal has a property where the Electric field vibrates in a single plane which is perpendicular to the direction of the radiation/travel and the same goes for Magnetic wave, it vibrates on a plane perpendicular to the travel.

Here we will discuss only the polarimetric radiation, or what we call PolSAR data. There are different polarimetric combinations present in the SAR dataset, and they are HH, VV, HV, and VH polarization. Here H is a horizontally polarized wave and V is a vertically polarized wave. HH and VV represent a horizontally polarized wave that is transmitted and received and the vertically polarized wave is transmitted and received. HV and VH are Horizontal transmit and Vertical receive and vice versa.

<p align="center">
<img width="500" src="https://user-images.githubusercontent.com/54326088/162222690-0768f963-2aef-4825-8093-c9da166adcc0.png">
</p>
<p align="center">image: <a href="https://youtu.be/wkpbDh2ysGs">Source</a></p>



Each of this polarization can create individual images by itself. Each image will be a complex image. When you have all 4 images, it becomes very easy to implement the modeling approaches to retrieve different scattering areas from small areas or single resolution cells. We can also use all four components to create a quad pole dataset, which looks like a color RGB image.

One of the modeling approaches is to stoke vector format of the dataset. It is consist of stoke parameters that are defined as $S = [S0,S1,S2,S3]$. stoke vector is the power representation of the SAR dataset. Using this power representation of the SAR dataset it is possible to do the decomposition modeling. There is a criterion when generating a quad pole/fully polarimetric dataset, and that is 
$S0^2 = S1^2 + S2^2 + S3^2$

If we get the values of stoke parameters then we can extract the orientation angle and ellipticity of the dataset. Below is the relationship. 

<p align="center">
<img width="500" src="https://user-images.githubusercontent.com/54326088/162222900-42c001e0-3b18-4a97-929c-33a2a7458c83.png">
</p>
<p align="center">image: <a href="https://youtu.be/TkqIw0CBq5c">Source</a></p>


Here $S0$, $S1$, $S2$, and $S3$ are the stoke parameters, $E_H$, $E_V$ is the horizontal and vertical components of the received electric field vector, Re and Im are the real and imaginary components, and `<>` indicated ensemble averaging.

Remember the intention behind combining all the different compontnts $[HH, VV, HV, VH]$ is to create a quad pole color image. and stoke vector format is one of the methods to do so. Here we will understand another method[important and extensively used] to combine all of the components, which is the scattering matrix,

## Scattering matrix: 
A scattering matrix is a 2x2 matrix that contains complex numbers, the diagonal elements represent the co-polarized information and off-diagonal information represents the cross-polarized information. Remember $S_HH$ and $S_VV$ are called light polarization channels, because received and transmitted signals are similarly polarized.  

<p align="center">
<img width="500" src="https://user-images.githubusercontent.com/54326088/162223604-3b7dacbd-8dfd-4fd4-9993-619793ea5fbb.png">
</p>
<p align="center">image: <a href="https://youtu.be/ZCtjQDe3Mvc">Source</a></p>



We can represent the scattering matrix by converting it to its 2nd order statistics for better decomposition. . Conversion of the scattering matrix to a 2nd order statistics is also called ensemble averaging.

<p align="center">
<img width="500" src="https://user-images.githubusercontent.com/54326088/162223909-cc6a22f5-60d1-4409-9341-a7675700acce.png">
</p>
<p align="center">image: <a href="https://youtu.be/ZCtjQDe3Mvc">Source</a></p>


We will discuss the covariency matrix now, we can use different basis for defining the matrix, eg. lexicographic basis($U_l$) Pauli basis ($U_p$). the combination of the scattering matrix and the basis is represented by $K_p$, this $K_p$ is also called as Pauli matrix. The mentioned basis is the Pauli basis


<p align="center">
<img width="500" src="https://user-images.githubusercontent.com/54326088/162224057-32770bc4-da1a-4ebb-987e-3ea2ea6d785f.png">
</p>
<p align="center">image: <a href="https://youtu.be/ZCtjQDe3Mvc">Source</a></p>


When we multiply the $K_p$ with its transpose we get the covariency matrix. We can also represent $<[T]> as <[T]> = K_p \times K_p T$. Here 9 represents that there are only 9 unique elements. as we are considering mono-static radar conditions so $S_HV = S_VH$. All the elements relate to physical scattering nature. Also a reminder we are taking each element/pixel of a single-pole image and putting them together into these matrixes to generate the quad pole data. 


<p align="center">
<img width="500" src="https://user-images.githubusercontent.com/54326088/162224262-86bc8c59-8d0d-4da6-ac72-73cdf9f29a33.png">
</p>
<p align="center">image: <a href="https://youtu.be/ZCtjQDe3Mvc">Source</a></p>



As I said before we can use any basis to get the decompose representation of the scattering matrix. Seems like basis are used to get an interpretation of different scattering on the generated color. The below image explains the correlation between the basis and colors. Here eigenvalue is the eigenvalues generated from the T matrix, and we can use these eigenvalues to calculate the entropy, alpha-angle, etc. different physical properties. Scattering power decomposition looks interesting because you can interpret the scatterings from the color, like if the portion of the image has green color in them then you can understand that it is coming from the volume scattering which implies that there is more vegetation. In the same way, double bounce correlates to having more buildings in the image and surface scattering correlated to plane surface. 

<p align="center">
<img width="500" src="https://user-images.githubusercontent.com/54326088/162224590-47c257e3-9a7c-4e85-ae19-34ae0bfd8bd4.png">
</p>
<p align="center">image: <a href="https://youtu.be/ZCtjQDe3Mvc">Source</a></p>

# Deep Learning in SAR data:

There is a ton of research going on in the combined field of SAR imagery and Deep Learning. A few examples are, 1. object detection, generally the problem involves finding relatively small targets (vehicles, ships, electricity infrastructure, oil and gas infrastructure, etc.) in a large scene dominated by clutter. 2. Image classification and segmentation, one example of this that also happened to be my internship project was to segment different urban areas, buildings, hills, and crops. 3. Change detection, I also had a project in this domain, Given an old image and a new image of the same footprint you need to find the changes, that image went through in that time gap.  

A number of fascinating research projects have explored the application of generative adversarial networks (GANs) to the enhanced analysis of SAR data and the fusion of data. Researchers have found that CNNs can be used to estimate and reduce speckle noise in SAR amplitude data, which also benefits object detection. A single feedforward process produces a "clean" SAR image. A GAN can also enhance the apparent resolution of SAR data. The literature contains examples of images translated from Sentinel-1 to TerraSAR-X resolution. Despite not being able to preserve feature structure well, these methods provide intriguing insights into the future of super-resolution and style transfer for SAR. Using GANs, high-resolution SAR data can also be made to look like optical imagery through despeckling and colorization, which could facilitate its visual interpretation.


I'm not covering two topics, which are Different SAR modes and ambiguities and also speckle. You can read about it here.
