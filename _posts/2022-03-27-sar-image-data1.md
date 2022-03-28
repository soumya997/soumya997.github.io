---
layout: post
title: SAR-cus! yeah and we are the clowns
subtitle: SAR Image Data 101 Gonna discuss all bout SAR data, from pros and cons, data collection, data interpretation and Deep Learning applications.
cover-img: /assets/img/kaggle_banner.png
thumbnail-img: https://pytorch.org/tutorials/_static/img/thumbnails/cropped/profiler.png
share-img: https://pytorch.org/tutorials/_static/img/thumbnails/cropped/profiler.png
tags: [SAR, remote sensing]
---


TOC:
1. Introduction
2. different types of satelite imagenaries
3. need of sar 
4. how it is captured
5. deep into range and azimuth resolution
6. different ranges k-band,x-band etc and applications
7. scattering
8. why it uses side projection
9. Different sar modes
10. different defaults
11. comparison with bats
12. what is footprints
13. what is antena [iitb-1]
14. complex form of a sar image
15. Deep learning
16. Interferometry and polarization


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





























































































All i mean to say is that the importance of satelite





I have worked in sar data as a part of my internship, I did not get a chance to collect the data[form softwares] and create different dicompositions, but I had to work on it 
considering the basic principals of a sar data. I had very minimal information about the dataset, good enough to get my job done. But looking backward I feel I 
should have studied more deeply into it. its been almost a year now, and now I got the time to dig deeper, and went through a lot of yt NPTL/IIRS videos and read
few articles. There is a lot to cover but the amount of information that I got going throught them is vast. I dont know I will be give justice to the consumed content
but I will surely try. 

This blog will be devided into 2-3 parts and I will go through different perspective of it, like the need of sar data, data collection, pros and cons, meaning behind the
collected data, how to interpret them. It might seem boring







To be specific it was polsar data. 


