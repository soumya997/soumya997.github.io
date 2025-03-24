---
layout: post
title: My Best Work!
subtitle: Major robotics project and personal projects
cover-img: /assets/img/linkedin_banner.jpeg
thumbnail-img: /assets/img/linkedin_banner.jpeg
share-img: /assets/img/linkedin_banner.jpeg
tags: [Portfolio]
---



# Best Project I have worked on!

One of the best projects I worked on was developing and deploying an end-to-end **Traffic Light Following** ADAS feature on an autonomous vehicle at IIIT Delhi. Here is a demo,

<div class="col-md-6">
    <iframe width="560" height="315" src="https://www.youtube.com/embed/Yhju9OP4RS8" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

Typically, an autonomous vehicle is given a start and goal pose, and it must plan an optimal path, avoid obstacles, and maintain a desired velocity while reaching the destination. For the traffic light following feature, the challenge was slightly different: given a start and end position, the vehicle needed to find an optimal path, stop before the stop line if a red light was detected, and resume movement when the light turned green.

The project was divided into several components:

1. **Perception - Traffic Light Detection:**  
   Initially, we used a YOLOv5 detector paired with a GMM classifier for color classification. However, this approach struggled to detect traffic lights early enough for the vehicle to react. To overcome this, we fine-tuned a YOLOP model with a SORT tracker using both Indian (IDD) and foreign (BDD) datasets, as well as our testbed dataset, applying various augmentation techniques. One effective strategy was training the model with both cropped images of the traffic lights and the full scene, enabling it to learn both local and global features, resulted into 0.2 mAP hike for TL detection. Ported model to Nvidia Orin using TensorRT, reducing the latency down by 20%.

2. **HD Map Creation:**  
   We developed a pipeline to automate HD map creation for any environment. Made possible to embed, stop lines info, traffic light positions, slope info.  The pipeline utilizes LiDAR Map to create Lanelet Maps. Point selection from LiDAR Map is done via, YOLOPV2 road segmentation and Plane Estimation.

3. **Planning:**  
   For planning, we employed a Behavior Tree framework for the traffic light following logic. The planning stack consisted of a spline-based local planner and a global planner that computed the shortest path using lanelet maps and an occupancy grid. I implemented the stopping profile that stops the vehicle in red light before stop line on the Behavior Tree in C++.

4. **Simulation:**
	Created Simulation environment for traffic light following in Carla. Tested the TLF stack multiple time on different scenario on simulation before integration to ego vehicle. 		

Apart from this project I have worked few more ADAS features such as Forward Collison Warning, Rear Collision Warning. For more details please check my resume and portfolio.
