---
layout: post
title: Best Project I've Worked on!
subtitle: My work on Autonomous vehicle at IIIT Delhi
thumbnail-img: /assets/img/linkedin_banner.jpeg
share-img: /assets/img/linkedin_banner.jpeg
tags: [Portfolio]
---


One of the best projects I worked on was developing and deploying an end-to-end **Traffic Light Following** ADAS feature on an autonomous vehicle at IIIT Delhi. Here is a demo:

<div style="text-align: center;">
  <iframe width="560" height="315" src="https://www.youtube.com/embed/Yhju9OP4RS8" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

Typically, an autonomous vehicle is given a start and goal pose, and it must plan an optimal path, avoid obstacles, and maintain a desired velocity while reaching the destination. For the traffic light following feature, the challenge was slightly different: given a start and end position, the vehicle needed to find an optimal path, stop before the stop line if a red light was detected, and resume movement when the light turned green.

The project was divided into several components:

1. **Perception - Traffic Light Detection:**

<div style="text-align: center;">
<iframe width="560" height="315" src="https://www.youtube.com/embed/Xk02kCv9rg4?si=R9IiWfNY6XajW9CX" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>>
</div>

- **Fine‑tuned a YOLOP model** with a SORT tracker using a combination of Indian (IDD), BDD (foreign), and our own testbed datasets.
- Applied **extensive data augmentation** and trained on both cropped traffic‑light patches and full‑scene images to capture local and global context.
- Achieved a +0.2 mAP improvement in traffic‑light detection accuracy.
- Optimized the model using **TensorRT** and ported to **NVIDIA Orin**, yielding a 20% reduction in inference latency.

2. **HD Map Creation:**


<table>
  <tr>
    <td>
      <iframe width="400" height="250" src="https://www.youtube.com/embed/KkV2-nVDkjs?si=NwUHQomD-fs62hSY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
    </td>
    <td>
      <img src="/assets/img/lanelet_map_josm.png" alt="Placeholder image" width="450" height="230" />
    </td>
  </tr>
</table>

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

5. **Testing:**

<div style="text-align: center;">
<iframe width="560" height="315" src="https://www.youtube.com/embed/Di3NJoGd9E0?si=QR66OrS7gf-aodUt" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
</div>

- Developed an XR test-bed with an RViz-based interface for efficient spawning and visualization of static and dynamic virtual obstacles.
- Created a C++ ROS package to publish obstacle positions to the obstacle mapper and overlay them on the camera image stream.

Apart from this project, I have worked on other ADAS features such as Forward Collision Warning and Rear Collision Warning. For more details, please check my resume and portfolio.
