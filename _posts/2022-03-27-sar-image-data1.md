---
layout: post
title: SAR-cus! And we are the clowns - 101
subtitle: SAR Image Data 101 Gonna discuss all bout SAR data, from pros and cons, data collection, data interpretation and Deep Learning applications.
cover-img: /assets/img/dataset-cover.jpg
thumbnail-img: https://spaceplace.nasa.gov/review/build-a-spacecraft/generic-sat.en.png
share-img: https://spaceplace.nasa.gov/review/build-a-spacecraft/generic-sat.en.png
tags: [SAR, remote sensing]
---



<p align="center">
<img src="https://cdn.cnn.com/cnnnext/dam/assets/220202125455-07-russia-buildup-satellite-exlarge-169.jpg">
</p>

<p align="center"><a href="https://edition.cnn.com/2022/02/02/europe/russia-troops-ukraine-buildup-satellite-images-intl/index.html">Source</a></p>

# TOC:
1. Introduction 
2. EM Wave Basics
3. Comparison of the optical image and SAR image
4. Comparison b/t different Remote Sensing Methods
6. need of sar 
7. how it is captured 
8. deep into range and azimuth resolution 
9. different ranges k-band,x-band etc and applications 
10. scattering 
11. why it uses side projection 
12. comparison with bats 
13. what is footprints 
14. what is antena 
15. complex form of a sar image  
16. Deep learning 
17. polarization 



## Introduction:
I have always been a computer vision fanatic, and I got to learn a lot more about this domain and its real-life applications from my internship at IIT, Kharagpur. I was assigned to work on PolSAR images for various tasks like image segmentation, colorization, etc. I knew only the bare minimum information that was required for applying deep learning, but a few weeks back I got some time to look back on this field and learn some of the basics, and this blog post is a part of that. This field extensively involves maths, and physics [spacially electromagnetic waves], reading only the theory is never that exciting but when we see the applications it suddenly makes the field very interesting, here you will mostly learn about the application of EM waves in satellite imagery. While learning I found out that the resources on these topics and other advanced SAR image topics are very less shared on the internet. This is another reason behind writing this article. I'm definitely not an expert in this field, I learned about them from all the links provided in the reference part, if you find any faults or miss information please comment below.

Ever thought about your life without Google maps, people use this google product all the time. Even your uber won't move an inch without google maps(or any software for aerial maps, apple maps excluded [meme]). Or think about life without your smartphone's weather forecasting, you won't be able to take an extra umbrella for your crush on a rainy day, just kidding..., life would really suck, right? And these aerial maps and weather forecasting are by-products of satellite imagery, and not only that forest fire detection, seasonal vegetation change tracking, flood mapping, ship detection, oil spill detection, crop classification, and the list goes on. What is really creepy/sussy is, that we can measure, identify and track human activity with it. Even satellite imageries are intensively getting used to track the current war between Ukraine and Russia. Posibilities are infinite XD

<!-- <p align="center">
<img width="300" src="https://user-images.githubusercontent.com/54326088/162196201-6b7050c5-d825-4dc4-9414-80ae59d09a4c.png">
</p>
 -->
 
<blockquote class="twitter-tweet"><p lang="en" dir="ltr">The average Apple Maps user <a href="https://t.co/8KRSlPiL2F">pic.twitter.com/8KRSlPiL2F</a></p>&mdash; Ecuadorian DeLorean (@EcuaDeLorean99) <a href="https://twitter.com/EcuaDeLorean99/status/1386864354881122304?ref_src=twsrc%5Etfw">April 27, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

<br>

# Comparison b/t different Remote Sensing Methods:
Satellite imagery comes under a broader topic called remote sensing, the name is pretty much self-explanatory. The basic concept is to sense different information about an object without getting into direct contact with the object. For that different kinds of sensors are used, for example, optical multispectral, thermal remote sensing, radar remote sensing, hyperspectral remote sensing, microwave remote sensing technique, etc. Optical RST is a passive remote sensing technique and microwave RST is an active imaging multilook RST.

There are many advantages of using microwave remote sensing[also known as sar] over others, but a few very important ones are
  1. SAR images can see through clouds, but other imaging techniques like optical or lidar are blocked by clouds. This is because of the different wavelengths used in different techniques, as the wavelength for SAR is from 3 cm up to a few meters, because of that it can easily ignore the dust particles of the cloud, but others can't. These wavelengths fall within the microwave part of the spectrum in the figure below.

<p align="center">
<img src="https://user-images.githubusercontent.com/54326088/162561535-c1c05bab-7240-4bbd-83f0-91a3132fdc67.png">
</p>
<p align="center">image 1, image 2[sahara desert]: <a href="https://medium.com/the-downlinq/sar-101-an-introduction-to-synthetic-aperture-radar-2f0b6246c4a0">Source</a></p>


  2. It can be used to detect objects day and night the times, same goes for lidar sensors but unlike optical imagery. 
  3. Multifrequency dataset can be accessed from the optical sensors but in the case of SAR sensor, it is only designed for one frequency/wavelength.

But unfortunately, there is only a handful of spaceborne and airbornes are out for collecting the SAR data, but with time applications are seemingly increasing and more and more of them are about to be deployed in near future.

A detailed comparison between SAR vs LIDAR vs Optical is provided below, 

<p align="center">
<img src="https://user-images.githubusercontent.com/54326088/162561513-c36a5c79-0076-48db-8961-53c0e97c9d8a.png">
</p>
<p align="center">image 1, image 2[sahara desert]: <a href="https://youtu.be/6cS-tq85Oic">Source</a></p>




# Comparison of the optical image and SAR image:
- Microwaves are primarily sensitive to structure, water content, and also dielectric nature of the material. but Optical data are reflectance spectra obtained from sunlight and are primarily sensitive to the illumination characteristics and molecular components of the region being imaged. Optical images are most sensitive to color information but not SAR images. Here in the image, the right side image is the SAR image and the left side image is the optical image. You can see that there is a dam at the bottom of the SAR image but that information is a mission in the optical image. And this is because optical images take color information into consideration.

In this image, we can see the clear topographical information for the SAR image but not for the optical image. And rever channels are also not visible in the optical images.




<!-- ![{7841C6BD-D4AA-4651-B7B6-6F07C1AC831C}](https://user-images.githubusercontent.com/54326088/162215195-3efdcdf9-74c4-4c2c-98fb-3a1efd053405.png) -->



Here is another example where optical sensors were unable to collect the proper information from the terrain, this is an image of the Sahara desert and if you focus on the streep then it reveals previously unmapped drainage channels from an earlier, wetter geological era. This thing is done by applying the L-band signals. The vegetation and surface penetration capabilities have been applied in the field of archeology as well.

<p align="center">
<img src="https://user-images.githubusercontent.com/54326088/162216896-ea02ca24-52e0-4663-ad79-1fce013d4fe0.png">
</p>
<p align="center">image 1, image 2[sahara desert]: <a href="https://youtu.be/6cS-tq85Oic">Source</a></p>


# EM Wave Basics:
Let's go through some basics of Electromagnetic radiation, below is a picture of an electromagnetic wave. as the name suggests, it is made of an electric field and magnetic field. The electric field varies in magnitude in a direction perpendicular to the direction in which the radiation is traveling, and the magnetic field is oriented at the right angle to the electrical field(M), both the fields travel at the speed of light(c). 
- wavelength: length of one wave cycle or the distance that a point has to travel to get back to its previous amplitude and phase is called wavelength.
- frequency: repeating event per unit of time

The relation between these two is, c = lambda/v



This might come to the mind what is the need for multiple frequencies/wavelengths for remote sensing, the answer is higher the wavelength, the higher the penetration capability. And based on that there are different applications for different bands. Say someone is interested to do mapping and monitoring below forest cover or subsurface mapping or monitoring, they will need high signals with high wavelengths, in this case, according to the requirements that person will use the L band or P band. The below diagram  explains the situation properly,  


<p align="center">
<img width="600" src="https://user-images.githubusercontent.com/54326088/162216490-c8381e36-8771-41aa-8828-54f21a9a5f68.png">
</p>
<p align="center">image 1 <a href="https://youtu.be/6cS-tq85Oic">Source</a></p>



# Data collection:
SAR airbornes/spaceborne collecting data is similar to how bats locate objects. How bats locate objects are called "echolocation", they emit ultrasonic sound pulses and receive the echoes to create an image of their surroundings. This is as same as how SAR satellites collect data. Sar sensors throw EM waves out in a side projection manner[it is side projection because if the projection is done perpendicularly wrt the satellite, then there won't be much scattering that will lead to less ground coverage], and the amount of area that signals cover at one-time stamp is called footprint. When the signals incident with the ground they will scatter depending on the kind of objects they incident on. There are different categories of scattering that occur, like 1. surface scattering, 2. Double bounce scattering and 3. volume scattering. Depending upon the shape, building materials, dielectric value, etc. this scattering happens, some singles get lost and some reflect back to the satellite antenna and the SAR technology uses that eco/reflection to generate an image of the footprint. Here surface scattering is the scattering when the incident ray interacts with a nearly smooth surface, generally, it's a scattering generated via surfaces. 2nd comes the `Double bounce scattering`, this happens when the signal gets incident on first a surface and then on a tall wall, as it is based on two scatterings so it is called double-bounce scattering. And describing `Volume scattering` as, when incident ray falls on a tree or vegetation some of the say get reflected bottom parts of the tree, like reflection due to upper leaves and brunches and then gradually moving down toward lower branches and leaves. Because of this, the entire body of the object causes scattering. The below diagram describes this better

<p align="center">
<img width="600" src="https://user-images.githubusercontent.com/54326088/162217603-593d43d5-80bc-478c-b815-bd56cd1512e2.png">
</p>
<p align="center">image: <a href="https://youtu.be/Z16a3Bi6Gq0">Source</a></p>





# Synthetic Aperture radar:
If you break down the name, it comes like Synthetic + Aperture + radar. synthetic is something that is artificially created, and Aperture is defined as the area, oriented perpendicular to the direction of an incoming electromagnetic wave or you can think of it as an antenna, and radar is a detection system that uses radio wave[EM waves comes under radio waves]. Now the question arises why do we need a synthetic aperture/antenna? Apparently bigger the antenna more the information you collect but the antennas of a satellite are not that big so we use the forward motion of the satellite to create a synthetic antenna. Pink elliptical circles are the footprint of the satellite.

<p align="center">
<img width="400" src="https://user-images.githubusercontent.com/54326088/162217883-11d40734-a857-489f-a57b-ee3b9d9fa20d.png">
</p>
<p align="center">image: <a href="https://youtu.be/Z16a3Bi6Gq0">Source</a></p>




The satellite moves forward and it emits the signals side ways[side looking geometry], and because of that, the group is scanned in two dimensions. One dimension is called the range dimension and the other is the azimuth dimension. Just of clarity, Azimuth is the direction where the satellite is moving and the direction where the signals are emitting is called range direction [see the image]. the angle between the nadir point and the direction of the emitted ray is called the look angle and the angle between the incident ray and the perpendicular to the ground is called the incident angle. the major axis of the ellipse is the swath and the slant range is the length b/w endpoint of the ellipse and the antenna. the area near the nadir point is called the near range and the area far away from the nadir is the far range.

<p align="center">
<img width="500" src="https://user-images.githubusercontent.com/54326088/162218835-be944ae0-a345-4987-9185-931db5e8e813.png">
</p>
<p align="center">image: <a href="https://youtu.be/Z16a3Bi6Gq0">Source</a></p>



## Resolution of SAR images:
We describe resolution in images are how many pixels are present in that image, and how clearly you can see every little detail of the image. In audio data restoration is defined like this, the sound is a longitudinal wave and to represent that kind of wave on a computer system in a continuous manner you need to increase the sample rate, this sample rate represents the resolution of audio, mainly if you represent audio on the computer then how continuous it is that is measured by the sample rate. But in the case of SAR images, there are 2 dimensions one is azimuth[vertica/height] and range[horizonta/width]. These two dimensions are defined as the ability to differentiate between two closely spaced objects in the respective directions. If we elaborate on this, then turns out something like this, when a signal hits two closely spaced targets then the ability to distinguish the echoes of these closely separated objects is called resolution here. Like other data, it also depends upon the ability of the device.



## Range Resolution:

<p align="center">
<img width="500" src="https://user-images.githubusercontent.com/54326088/162219811-6bb952ef-cbd0-4c75-be5d-e2b37b21cd33.png">
</p>
<p align="center">image: <a href="https://youtu.be/wkpbDh2ysGs">Source</a></p>



The ability to distinguish between two echos coming from closely spaced objects in the range direction depends on the duration of microwave pulse transmitted by radar[quantifies time]. 

Range resolution = c x t/2

if range resolution is defined as the separation between the eco pulses of two targets then, In this case, c x t_p/2 is the range resolution, because there is no gap between pulses generated from Ta and T2, then the T2 pulse is ending the T1 pulse is generating. so range resolution is the function of t_p. 


Here you can see as the distance between two aircraft are shorter than the pulse length, the sensor is unable to detect the 

Check this portion carefully, this pulse of t= 2us is emitted, so the pulse length is c x t = 600m and the range resolution is 600/2 m, now in this example as the distance b/w the two aircraft are 200 which is less than the pulse, so the sent pulse will be unable to detect the two objects/aircraft and may consider as the single image.

<p align="center">
<img width="500" src="https://user-images.githubusercontent.com/54326088/162221474-4663fcdb-c7b2-452a-a763-b388524fd760.png">
</p>
<p align="center">image: <a href="https://youtu.be/6cS-tq85Oic">Source</a></p>



Range resolution has two types, slant resolution and ground resolution,

<p align="center">
<img width="500" src="https://user-images.githubusercontent.com/54326088/162221080-203a7106-e8f4-4b93-bb20-9a5676489990.png">
</p>
<p align="center">image1: <a href="https://youtu.be/6cS-tq85Oic">Source</a>, image2: <a href="https://youtu.be/wkpbDh2ysGs">Source</a></p>




There is another topic called `Chirped pulse`, which is used to describe range resolution. You can read more about them [here.](https://www.youtube.com/watch?v=wkpbDh2ysGs&t=1577s)

### Slant range ambiguity:
Objects appearing in the near range are compressed in their shape and size, this compression problem is called the slant range ambiguity. This should be minimized before further processing.

## Azimuth resolution:
As discussed previously the azimuth resolution is the ability to distinguish two echos coming from two closely spaced objects in the azimuth direction. According to the formula, azimuth resolution depends on a few things as shown below,

<p align="center">
<img width="500" src="https://user-images.githubusercontent.com/54326088/162226905-de701c2c-d184-4d02-b89b-ce22fc5c3a30.png">
</p>
<p align="center">image: <a href="https://youtu.be/wkpbDh2ysGs">Source</a></p>



# complex form of a sar image:
Physical properties of the terrain change the phase and amplitude of the EM wave. And SAR is used to calculate these two properties of a wave, which are amplitude(A) and phase(phi). It is represented as a number pair, (A cos(shi), Asin(shi)). How bright or dark the pixels are depended upon the strength of eco received. This amplitude and phase information is captured in the form of a complex number. Every complex number consists of a real part and an imaginary part. Here,
 Real part(a) = A cos(shi), Imaginary part(b) = A sin(shi)

Each pixel value equals to a+ib, where shi = tan^-1(a/b)
and the amplitude is sqrt(a^2+b^2)

Intensity (I) = A^2, logarithmic Intensity = log(I)



## Radar Polarization:
The emitted EM wave signal can be polarized and unpolarized. By unpolarized I mean, the unpolarized energy vibrates in all possible directions to the direction of travel. But polarized signal has a property where the Electric field vibrates in a single plane which is perpendicular to the direction of the radiation/travel and the same goes for Magnetic wave, it vibrates on a plane perpendicular to the travel.

Here we will discuss only the polarimetric radiation, or what we call PolSAR data. There are different polarimetric combinations present in the SAR dataset, and they are HH, VV, HV, and VH polarization. Here H is a horizontally polarized wave and V is a vertically polarized wave. HH and VV represent a horizontally polarized wave that is transmitted and received and the vertically polarized wave is transmitted and received. HV and VH are Horizontal transmit and Vertical receive and vice versa.

<p align="center">
<img width="500" src="https://user-images.githubusercontent.com/54326088/162222690-0768f963-2aef-4825-8093-c9da166adcc0.png">
</p>
<p align="center">image: <a href="https://youtu.be/wkpbDh2ysGs">Source</a></p>



Each of this polarization can create individual images by itself. Each image will be a complex image. When you have all 4 images, it becomes very easy to implement the modeling approaches to retrieve different scattering areas from small areas or single resolution cells. We can also use all four components to create a quad pole dataset, which looks like a color RGB image.

One of the modeling approaches is to stoke vector format of the dataset. It is consist of stoke parameters that are defined as S = [S0,S1,S2,S3]. stoke vector is the power representation of the SAR dataset. Using this power representation of the SAR dataset it is possible to do the decomposition modeling. There is a criterion when generating a quad pole/fully polarimetric dataset, and that is 
S0^2 = S1^2 + S2^2 + S3^2

If we get the values of stoke parameters then we can extract the orientation angle and ellipticity of the dataset. Below is the relationship. 

<p align="center">
<img width="500" src="https://user-images.githubusercontent.com/54326088/162222900-42c001e0-3b18-4a97-929c-33a2a7458c83.png">
</p>
<p align="center">image: <a href="https://youtu.be/TkqIw0CBq5c">Source</a></p>


Here S0, S1, S2, and S3 are the stoke parameters, E_H, E_V is the horizontal and vertical components of the received electric field vector, Re and Im are the real and imaginary components, and `<>` indicated ensemble averaging.

Remember the intention behind combining all the different compontnts[HH, VV, HV, VH] is to create a quad pole color image. and stoke vector format is one of the methods to do so. Here we will understand another method[important and extensively used] to combine all of the components, which is the scattering matrix,

## Scattering matrix: 
A scattering matrix is a 2x2 matrix that contains complex numbers, the diagonal elements represent the co-polarized information and off-diagonal information represents the cross-polarized information. Remember S_HH and S_VV are called light polarization channels, because received and transmitted signals are similarly polarized.  

<p align="center">
<img width="500" src="https://user-images.githubusercontent.com/54326088/162223604-3b7dacbd-8dfd-4fd4-9993-619793ea5fbb.png">
</p>
<p align="center">image: <a href="https://youtu.be/ZCtjQDe3Mvc">Source</a></p>



We can represent the scattering matrix by converting it to its 2nd order statistics for better decomposition. . Conversion of the scattering matrix to a 2nd order statistics is also called ensemble averaging.

<p align="center">
<img width="500" src="https://user-images.githubusercontent.com/54326088/162223909-cc6a22f5-60d1-4409-9341-a7675700acce.png">
</p>
<p align="center">image: <a href="https://youtu.be/ZCtjQDe3Mvc">Source</a></p>


We will discuss the covariency matrix now, we can use different basis for defining the matrix, eg. lexicographic basis(U_L) Pauli basis (U_P). the combination of the scattering matrix and the basis is represented by K_p, this K_p is also called as Pauli matrix. The mentioned basis is the Pauli basis


<p align="center">
<img width="500" src="https://user-images.githubusercontent.com/54326088/162224057-32770bc4-da1a-4ebb-987e-3ea2ea6d785f.png">
</p>
<p align="center">image: <a href="https://youtu.be/ZCtjQDe3Mvc">Source</a></p>


When we multiply the k_p with its transpose we get the covariency matrix. We can also represent <[T]> as <[T]> = K_p * K_pT. Here 9 represents that there are only 9 unique elements. as we are considering mono-static radar conditions so S_HV = S_VH. All the elements relate to physical scattering nature. Also a reminder we are taking each element/pixel of a single-pole image and putting them together into these matrixes to generate the quad pole data. 


<p align="center">
<img width="500" src="https://user-images.githubusercontent.com/54326088/162224262-86bc8c59-8d0d-4da6-ac72-73cdf9f29a33.png">
</p>
<p align="center">image: <a href="https://youtu.be/ZCtjQDe3Mvc">Source</a></p>



As I said before we can use any basis to get the decompose representation of the scattering matrix. Seems like basis are used to get an interpretation of different scattering on the generated color. The below image explains the correlation between the basis and colors. Here eigenvalue is the eigenvalues generated from the T matrix, and we can use these eigenvalues to calculate the entropy, alpha-angle, etc. different physical properties. Scattering power decomposition looks interesting because you can interpret the scatterings from the color, like if the portion of the image has green color in them then you can understand that it is coming from the volume scattering which implies that there is more vegetation. In the same way, double bounce correlates to having more buildings in the image and surface scattering correlated to plane surface. 

<p align="center">
<img width="500" src="https://user-images.githubusercontent.com/54326088/162224590-47c257e3-9a7c-4e85-ae19-34ae0bfd8bd4.png">
</p>
<p align="center">image: <a href="https://youtu.be/ZCtjQDe3Mvc">Source</a></p>


There is a ton of research going on in the combined field of SAR imagery and Deep Learning. A few examples are, 1. object detection, generally the problem involves finding relatively small targets (vehicles, ships, electricity infrastructure, oil and gas infrastructure, etc.) in a large scene dominated by clutter. 2. Image classification and segmentation, one example of this that also happened to be my internship project was to segment different urban areas, buildings, hills, and crops. 3. Change detection, I also had a project in this domain, Given an old image and a new image of the same footprint you need to find the changes, that image went through in that time gap.  

A number of fascinating research projects have explored the application of generative adversarial networks (GANs) to the enhanced analysis of SAR data and the fusion of data. Researchers have found that CNNs can be used to estimate and reduce speckle noise in SAR amplitude data, which also benefits object detection. A single feedforward process produces a "clean" SAR image. A GAN can also enhance the apparent resolution of SAR data. The literature contains examples of images translated from Sentinel-1 to TerraSAR-X resolution. Despite not being able to preserve feature structure well, these methods provide intriguing insights into the future of super-resolution and style transfer for SAR. Using GANs, high-resolution SAR data can also be made to look like optical imagery through despeckling and colorization, which could facilitate its visual interpretation.


I'm not covering two topics, which are Different SAR modes and ambiguities and also speckle. You can read about it here.




















































All i mean to say is that the importance of satelite





I have worked in sar data as a part of my internship, I did not get a chance to collect the data[form softwares] and create different dicompositions, but I had to work on it 
considering the basic principals of a sar data. I had very minimal information about the dataset, good enough to get my job done. But looking backward I feel I 
should have studied more deeply into it. its been almost a year now, and now I got the time to dig deeper, and went through a lot of yt NPTL/IIRS videos and read
few articles. There is a lot to cover but the amount of information that I got going throught them is vast. I dont know I will be give justice to the consumed content
but I will surely try. 

This blog will be devided into 2-3 parts and I will go through different perspective of it, like the need of sar data, data collection, pros and cons, meaning behind the
collected data, how to interpret them. It might seem boring







To be specific it was polsar data. 


