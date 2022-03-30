---
layout: post
title: SAR-cus! And we are the clowns - 101
subtitle: SAR Image Data 101 Gonna discuss all bout SAR data, from pros and cons, data collection, data interpretation and Deep Learning applications.
cover-img: /assets/img/kaggle_banner.png
thumbnail-img: https://pytorch.org/tutorials/_static/img/thumbnails/cropped/profiler.png
share-img: https://pytorch.org/tutorials/_static/img/thumbnails/cropped/profiler.png
tags: [SAR, remote sensing]
---


# TOC:
1. Introduction ✅ 
2. different types of satelite imagenaries ✅
3. need of sar ✅
4. how it is captured ✅
5. deep into range and azimuth resolution ✅
6. different ranges k-band,x-band etc and applications ✅
7. scattering ✅
8. why it uses side projection ✅
9. Different sar modes [pass]
10. different defaults [pass]
11. comparison with bats ✅
12. what is footprints ✅
13. what is antena [iitb-1] ✅
14. complex form of a sar image ✅ 
15. Deep learning 
16. polarization ✅


## Introduction:
Think bout life without Google maps, people use this all the time. Even your uber wont move an inch without google maps(or any software for aerial maps, apple maps excluded [meme]). Or think about life without your smartphone's weather forcasting, you wont be able to take a extra umbrale for your crush on a rainy day, life would really suck right? And these aerial maps, weather forcasting are by products of satellite imagery, and not only that forest fire detection, seasonal vegetation change tracking, flood mappiong, ship detection, oil spill detection, crop classification and the list goes on. What is really is creepy/sussy is we can measure, identify and track human activity with it. Even satellite imageries are intensively used to track the current war between Ukraein and Russia [image]. Posibilities are infinite [meme].

Satellite imagery comes under a broader topic called remote sensing, the name is pretty much self explanatory. The basic concept is to sense different information about
an object without getting into direct contact with the object. For that different kind of sensors are used, for example optical multispectral, thermal remote sensing, radar remote sensing, hyperspectral remote sensing, microwave remote sensing technique etc. Optical RST is a passive remote sensing technique and microwave RST is active imaging multilook RST. 

There are many advantages of using microwave remote sensing[also known as sar] over others, but few very important ones are
  1. SAR images can see through clouds, but other imagery techniques like optical or lidar are blocked by clouds. This is because of the different wavelengths used in different techniques, as the wavelength for SAR is from 3 cm up to a few meters, because of that it can easily ignore the dust particles of the cloud, but others cant. These wavelengths fall within the microwave part of the spectrum in the figure below. [https://miro.medium.com/max/550/0*wK5PY4PoJKHNjgYE]
  2. It can be used to detect objects in day and night both the times, same goes for lidar sensors,but unlike optical imagery. 
  3. Multi frequency dataset can be accessed from the optical sensors but in case of SAR sensor, it is only designed for one frequency/wavelength.

But unfortunately there are only handfull of spacebornes and airbornes are out the for collecting the SAR data, but with time application are seemingly increasing and more and more of them are about to be deployed in near future.

A detailed comparison between SAR vs LIDAR vs Optical is provided below [image], 

Comparison of optical image and sar image, 
- Microwaves are primarily sensitive to structure, water content and also dielectric nature of the material. but Optical data are reflectance spectra obtain from sunlight and primarily sensitive ti the illumination charecteristics and molecular componets of the region being imaged. Optical images are mostly sensitive toward color information but not the sar images. Here in the image, the right sind imaage is the SAR image and the left side image is the optical image. You can see that there is a dam at the bottom of the sar image but that information is mission in the optical image. And this is because optical images takes color information into consideration.

In this image we can see the clear topographycal information for the sar image but not for the optical image. And rever channels are also not visible in optical image.

Here is another example where optical sensors were unable to coloect the proper informations from the terrain, this is a image of sahara desert and if you focus on the streep then it reveals previously unmapped drainage channels from an earlier, wetter geological era. This thing is done by applying the L-band signals. The vegetation and surface penetration capabilities has been applied in the field of archeology as well.

Lets go through some basics of Electromagnetic radiations, below is the picture of a electro magnetic wave. as the name suggests, it is made of electric field and magnetic field. The electric field varies in magnitude in a direction perpendicular to the direction in which the radiation is traveling, and the magnetic field orient at the right angles to the electrical field(M), both the fields travel at the speed of light(c). 
- wavelength: length of one wave cycle, or the distance that a point has to travel to get back to its previus amplitude and phase is called wavelength.
- frequency: repeating event per unit of time

Relation between these two is, c = lambda/v



This might come to the mind that what is the need of multiple frequencies/wavelength for remote sensing, the answer is higher the wavelength, the higher the penetration capability. And based on that there are differnet application for differnt bands. Say someone is interested to do mapping and monitoring below forest cover or subsurface mapping or monitoring, they will need high signals with high wavelengths, in this case according to the requirements that person will use L band or P band. Below diagram  explains the situation properly[image],  



# Data collection:
SAR airbrones/spacebrones collecting data is similar to how bats locate objects. How bats locate objects are called "ecolocation", they emmit ultrasonic sound pulses, and recieve the ecos to create an image of its surroundings. Tis is as same as how sar satellites collecets data. Sar sensors throws EM waves out in a side projection manner[it is side projecttion because if the projection is done perpendicularly wrt the satellite, then there wont be much scattering that will lead to less ground coverage], and the amount of area that signals cover at one time stamp is called footprint. When the signals incidents with the ground it will scatter bepending on the kind of objects it is incidenting on. There are different categories of scattering occurs, like 1. surface scattering, 2. Bouble bounce scattering and 3. volume scattering. Depending upon the shape, building materials, dielectric value etc. this scattering happens, some singls gets lost and some reflects back tot the satellite antena and the SAR technology uses that eco/reflection to generate an image of the footprint. Here surface scattering is the scattering when the incedent ray interacts with a nearly smoote surface, generally its a scattering generated via surfaces. 2nd comes the `Double bounce scattering`, this happens when the signal get incident on first a surface and then on a tall wall, as it is based on two scattering so it is called double bounce scatttering. And describing `Volume scattering` as, when incident ray falls on a tree or vagitation some of the say get reflected to bottom parts of the tree, like reflection ude to upper leafs and brunches and then gradually moving down toward lower brnches and leafes. Because of this, entire body of the object causes the scattering. Below diagram describes this better[scattering image]

![[Pasted image 20220328215234.png]]
# Synthatic Aperture radar:
If you break down the name, it comes like Synthatic + Aperture + radar. synthetic is something is artificially created, and Aperture is defined as the area, oriented perpendicular to the direction of an incoming electromagnetic wave or you can think of it as antenna, and radar is a detection system what uses radio wave[EM waves comes under radio waves]. Now question arises why we need a synthatic aperture/antenna? Apparently bigger the antenna more the information you collect but antennas of a satellite is not tha big so we use the forward motion of the satellite to create a synthetic antenna. Pink eleptical circles are the footprint of the satellite.

![[Pasted image 20220328135109.png]]

	![[Pasted image 20220328134741.png]]

![[Pasted image 20220328215526.png]]

The satelite moves forward and it emmits the signals side ways[side looking geometry], and because of that the groud is scanned in two dimentions. One dimention is called the range dimention and other is the azimuth dimention. Just of clarity,
Azimuth is the direction where the satellite is moving and to the direction where the signals are emitting are called range direction [see the image]. the angle between nadir point and the direction of the emitted ray is called look angle and the angle between the incident ray and the parpendiculat to the ground is called incident angle. major axis of the ellipse is the swath and the slant range is the length b/w end point of the ellipse and the antenna. area near to the nadir point is near range and the area far away from nadir is far range.

We describe resolution in images are how many pixels are present that image, how clearly you can see every little details of the image. In audio data restoration is defined like this, sound is a longitudinal wave and to represent that kind of wave on computer system in a continuous manner you need to increase the smaple rate, this sample rate represnets the resolution of an audio, mainly if you represt a audio on computer then how continius it is that is measured by sample rate. But incase of sar images there are 2 dimention one is azimuth[vertica/height] and range[horizonta/width]. These two dimetions are defined as ability to differenticate between two closely spaced objects in the respective directions. If we elaburate this, then turns out somehting like this, when a signal hits two closely spaced targets then the ability to distinguish the ecos of these closely seperated objects is called resolution here. As like other data it also depends upon the ability of the device.


## Range Resolution:
![[Pasted image 20220328143800.png]]


Ability to distinguish between two ecos coming from a closely spaced objects in the range direction depends on the duration of microwave pulse trasmitted by radar[quantifies time]. 

Range resolution = c x t/2

if range resolution is defined as the saperation between the eco pulses of two targets then, In this case c x t_p/2 is the range resolution, because there is no gap between pulses generated from Ta and T2, then the T2 pulse is ending the T1 pulse is generating. so range resolution is the function of t_p. 


Here you can see as the distance between two airoplanes are shorter than the pulse length, the sensor is unable to detect the 

Check this portionn carefully, this pulse of t= 2us is emitted, so the pulse length is c x t = 600m and the range resoution is 600/2 m, now in this example as the distance b/w the two aircrafts are 200 which is less than the pulse, so the sent pulse will be unable to detect the two objects/aircrafts and may consider as the single image.

![[Pasted image 20220328202240.png]]

Range resolution has two types, slant resolution and ground resolution
![[Pasted image 20220328232723.png]]
![[Pasted image 20220328202326.png]]

There is another topic called `Chirped pulse`, which is used to describe range resolution. You can read more about them [here.](https://www.youtube.com/watch?v=wkpbDh2ysGs&t=1577s)

### Slant range ambiguity:
Objects apearing in the near range are compresed in their shape and size, this compression problem is called as the slant range ambiguity. This should be minimized before further processing.

## Azimuth resolution:
As discussed previous the azimuth resolution is the ability to distinguish two ecos coming from two closely spaced objects in the azimuth direction. According to the formula, azimuth resolution depends on few things as shown below,

![[Pasted image 20220328210534.png]]


# complex form of a sar image:
Physical properties of the terrain changes the phase and amplitude of the EM wave. And SAR is used to calculate this two properties of a wave, which is amplitude(A) and phase(shi). It is represented as a number pair, (A cos(shi), Asin(shi)). How bright or dark the pixels are depends upon the strength of eco recived. This amplitude and phase information is captured in the form of a complex number. Every complex number is consists of a real part and a imaginary part. Here,
 Real part(a) = A cos(shi), Imaginary part(b) = A sin(shi)

Each pixel value equals to a+ib, where shi = tan^-1(a/b)
and the amplitude is sqrt(a^2+b^2)

Intensity (I) = A^2, logarithmic Intensity = log(I)



## Radar Polarization:
The emitted EM wave signal can be polarized and unpolarized. By unpolarized I mean, the unpolarized energy vibrates in all possible direction to the direction of travel. But polarized signal has a property where, the Electric field vibrates in a single plane which is parpendicular to the direction of the radiation/travel and same goes for Magnetic wave, it vaibrates on a plane perpendicular to the travel.

Here we will discuss only the polarometric radiation, or what we call PolSAR data. There are different polarometric combinations present in the SAR dataset, and they are HH, VV,HV and VH polarization. Here H is horizontally polarized wave and V is vertically polarized wave. HH and VV represents horizontally polarized wave is transmitted and recieved  and verticall polarized wave is transmitted and recieved. HV and VH is Horizontal transmit and Vertical recieve and vice versa.

![[Pasted image 20220328234334.png]]

Each of this polarization can create individual iamges by them self. Each images will be a complex image. When you have all the 4 images, it becomes very easy to implement the modeling approaches to retrive different of scattering area from small area or single resolution cell. We can also use all the four components to create a quad pole dataset, which looks like color RGB image.

One of the modeling approach is stoke vector format of dataset. It is consist of stoke parameters that are defined as S = [S0,S1,S2,S3]. stoke vertor is the power representation of the SAR dataset. Using this power representation of the SAR dataset it is possible to do the decomposition modeling. There is a criteria when generating quad pole/fully polarometric dataset, and that is 
S0^2 = S1^2 + S2^2 + S3^2

If we get the values of stoke parameters then we can extract the orientation angle and ellipticity of the dataset. Below is the relationship. 

![[Pasted image 20220329192857.png]]

Here S0,S1,S2,S3 are the stoke parameters, E_H, E_V is the horizontal and vertical components of the recieved electric field vector, Re and Im are the real and imaginary components and `<>` indicated ensemble averaging.

Remember the intention behind combining all the different compontnts[HH,VV,HV,VH] is to create a quad pole color image. and stoke vector format is one of the methods to do so. Here we will understand another method[important and extensivel used] to combine all of the components, which is scattering matrix,

Scattering matrix: 
Scattering matrix is a 2x2 matrix that contains complex numbers, the diagonal elements represents the co-polarized information and off-diagonal information represents the cross-polarized information. Remember S_HH and S_VV are called light polarization channels, because recieved and transmitted signals are similarly polarized.  

![[Pasted image 20220329200336.png]]

We can represent scattering matrix by converting it to its 2nd order statistics for better decomposition. . Conversion of scattering matrix to a 2nd order statistics is also called as ensemble averaging.
![[Pasted image 20220329200943.png]]


We will discuss about the covariency matrix now, we can use different basis for defining the matix, eg. lexicographic basis(U_L) Pauli basis (U_P). the combination of the sattering matrix and the basis is represented by K_p, this K_p also called as pauli matrix. The metioned basis is the pauli basis

![[Pasted image 20220329211013.png]]


When we multiply the k_p with its transpose we get the covariency matrix. We can also represent <[T]> as <[T]> = K_p * K_pT. Here 9 represents that there are only 9 unique elements. as we are considering mono-static radar condition so S_HV = S_VH. All the elements relates to physical scattering nature. Also a reminder we are taking each elements/pixels of a single pole image and putting them together into these matrixes to generate the quad pole data. 
![[Pasted image 20220329212502.png]]

As I said before we can use any basis to get the decompose representation of the scattering matrix. Seems like basis are used to get a interpretation of different scattering on the generated color. Below image explains the corelation between the basis and colors. Here eigenvalue is the eigenvalues generated from the T matrix, and we can use these eigenvalues to calculate the entropy, alpha-angle etc. different physical properties. Scattering power decomposition looks interesting because you can interpret the scatterings from the color, like if the portion of the image has green color in them then you can understand that it is coming from the volume scattering which implies that there is more vegitation. In the same way, double bounce corelates to haing more buildings in the image and surface scattering corelated to plane surface. 

![[Pasted image 20220329213739.png]]


There are a ton of reasearch going on in the combined field of SAR imagery and Deep Learning. Few examples are, 1. object detection, generally the problem involves finding relatively small targets (vehicles, ships, electricity infrastructure, oil and gas infrastructure, etc.) in a large scene dominated by clutter. 2. Image classification and segmentation, one example of this that also happens tobe my internship project was to segmentation different urban areas, building, hills, crops. 3. Change detection, I also had a project in this domain, Given an old image and new image of a same footprint you need to find the chnages, those image went through in that time gap.  

A number of fascinating research projects have explored the application of generative adversarial networks (GANs) to the enhanced analysis of SAR data, and the fusion of data. Researchers have found that CNNs can be used to estimate and reduce speckle noise in SAR amplitude data, which also benefits object detection. A single feedforward process produces a "clean" SAR image. A GAN can also enhance the apparent resolution of SAR data. The literature contains examples of images translated from Sentinel-1 to TerraSAR-X resolution. Despite not being able to preserve feature structure well, these methods provide intriguing insights into the future of super-resolution and style transfer for SAR. Using GANs, high resolution SAR data can also be made to look like optical imagery through despeckling and colorization, which could facilitate its visual interpretation.


Im not covering two topics, which are Different sar modes and ambiguities and also speckle. You can read about it from here.



















































All i mean to say is that the importance of satelite





I have worked in sar data as a part of my internship, I did not get a chance to collect the data[form softwares] and create different dicompositions, but I had to work on it 
considering the basic principals of a sar data. I had very minimal information about the dataset, good enough to get my job done. But looking backward I feel I 
should have studied more deeply into it. its been almost a year now, and now I got the time to dig deeper, and went through a lot of yt NPTL/IIRS videos and read
few articles. There is a lot to cover but the amount of information that I got going throught them is vast. I dont know I will be give justice to the consumed content
but I will surely try. 

This blog will be devided into 2-3 parts and I will go through different perspective of it, like the need of sar data, data collection, pros and cons, meaning behind the
collected data, how to interpret them. It might seem boring







To be specific it was polsar data. 


