---
layout: post
title: Best Project I've Worked on!
subtitle: My work on Autonomous vehicle at IIIT Delhi
cover-img: /assets/img/kaggle_banner.png
thumbnail-img: /assets/img/kaggle_banner.png
share-img: /assets/img/kaggle_banner.png
tags: [Portfolio]
---

# Best Project I've Worked on!

One of the best projects I worked on was developing and deploying an end-to-end **Traffic Light Following** ADAS feature on an autonomous vehicle at IIIT Delhi. Here is a demo:

<iframe width="560" height="315" src="https://www.youtube.com/embed/Yhju9OP4RS8" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Typically, an autonomous vehicle is given a start and goal pose, and it must plan an optimal path, avoid obstacles, and maintain a desired velocity while reaching the destination. For the traffic light following feature, the challenge was slightly different: given a start and end position, the vehicle needed to find an optimal path, stop before the stop line if a red light was detected, and resume movement when the light turned green.

The project was divided into several components:

1. **Perception - Traffic Light Detection:**

<iframe width="560" height="315" src="https://youtu.be/Xk02kCv9rg4" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

- **Fine‑tuned a YOLOP model** with a SORT tracker using a combination of Indian (IDD), BDD (foreign), and our own testbed datasets.
- Applied **extensive data augmentation** and trained on both cropped traffic‑light patches and full‑scene images to capture local and global context.
- Achieved a +0.2 mAP improvement in traffic‑light detection accuracy.
- Optimized the model using **TensorRT** and ported to **NVIDIA Orin**, yielding a 20% reduction in inference latency.

2. **HD Map Creation:**

<iframe width="560" height="315" src="https://youtu.be/KkV2-nVDkjs" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

- Developed a fully **automated pipeline for HD‑map creation** in arbitrary environments.
- Automatically **embedded key map attributes**: stop‑line locations, traffic‑light positions, and road slope information.
- Converted raw **LiDAR maps into Lanelet‑formatted HD maps.**
- Selected relevant ground points from the LiDAR map using **YOLOPv2 road segmentation** combined with plane estimation.

3. **Planning:**  
- Utilized a **Behavior Tree** framework to implement traffic‑light‑following logic.
- Implemented a **C++ stopping profile** in the Behavior Tree to reliably stop the vehicle before the stop line at a red light.

4. **Simulation:**
- Created a **CARLA simulation environment** for traffic‑light‑following.
- Developed a diverse set of test scenarios (varying intersection layouts, signal timings, and traffic densities).
- Executed repeated simulation runs to validate and refine the traffic‑light‑following (TLF) stack.

Apart from this project, I have worked on other ADAS features such as Forward Collision Warning and Rear Collision Warning. For more details, please check my resume and portfolio.
