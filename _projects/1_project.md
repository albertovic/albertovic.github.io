---
layout: page
title: Semantic Annotation of LiDAR Data
description: Bridging the gap between raw depth data and semantic understanding using Foundation Models.
img: assets/img/projects/MiR_Robot.jpeg
importance: 1
category: work
---

### The Challenge
Standard 2D LiDAR maps are great for geometry, but they are "blind" to context. A robot sees a wall and a person as the same type of obstacle: a static line of points. In this project, we wanted to fix that by "painting" semantic labels from an RGB camera onto the LiDAR point cloud.

### My Role & Technical Approach
I took point on the **sensor calibration** and the **SAM 3 (Segment Anything Model)** integration. 

* **The Math Behind the Fusion:** I handled the intrinsic camera calibration using OpenCV and an ARUCO checkerboard. This was critical—if your focal lengths or distortion coefficients are off, the LiDAR points won't align with the pixels, and the whole "painting" process fails.
* **Deploying Foundation Models:** I integrated Meta’s **SAM 3** to handle the heavy lifting of segmentation. Since we were working on a laptop with only 8GB of VRAM, I had to get creative with resource management to prevent the pipeline from crashing.



### Solving the Memory Problem
You can't just run a massive Vision Transformer (ViT) on mobile hardware without a plan. I implemented two main strategies to keep the VRAM usage under control:
1. **Resolution Scaling:** I downscaled the input images, which gave us a massive 44% saving on activation memory.
2. **Feature Caching:** Since the visual features of a frame don't change regardless of what you're looking for, I cached the heavy image embeddings. This allowed us to query multiple labels (like "person" then "table") using only the lightweight mask decoder, keeping the VRAM usage flat.

### The Results
We compared **SAM 3** against **YOLOv11**. While YOLO was faster (real-time at 24 FPS), SAM 3 was significantly more accurate, with a **30% improvement in segmentation fidelity**. For a mapping mission where you care more about a clean map than moving fast, the trade-off was worth it.

### What I Took Away
This wasn't just about making a cool demo. It taught me how to:
* Handle the geometric reality of sensor fusion (Rotation and Translation matrices aren't just for textbooks).
* Optimize cutting-edge AI models for hardware that isn't a server-grade GPU.
* Work in a fast-paced team to solve a real industrial challenge.

[Download our Full Technical Report]({% link assets/pdf/EIA_Project.pdf %})