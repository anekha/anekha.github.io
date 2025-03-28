---
title: "3D Reconstruction for Ornaments: From Camera Poses to Geometry"
date: 2025-03-28
categories:
  - research
  - computer vision
  - 3D
excerpt: "Exploring 3D reconstruction methods for ornaments using photogrammetry, NeRF, Gaussian Splatting, and camera-pose transformers."
tags:
  - 3D reconstruction
  - NeRF
  - Gaussian Splatting
  - Photogrammetry
  - COLMAP
  - Camera Poses
---

## Introduction 💍

One of my current areas of exploration is 3D reconstruction of **ornaments** — specifically jewelry and intricate objects — using only images. This is challenging due to the reflective nature of metals, small surface details, and the lack of standard 3D data.

In this post, I’m documenting my journey through various 3D reconstruction techniques and what I've learned so far.


## The Foundation: Camera Poses 🧭

Everything starts with accurate **camera poses**. Without them, reconstruction fails.

- **COLMAP** is the traditional starting point — great for Structure from Motion (SfM), but it struggles with:
  - Reflective surfaces
  - Internet-scraped images (no metadata)
  - Sparse keypoint matching across different views

- **Manual calibration** is tedious and doesn't scale.

- **Transformer-based pose estimators** like **DUST3R**, **Mast3r**, and newer retrieval-augmented models are promising because they:
  - Don’t require camera metadata
  - Work on unstructured multi-view images
  - Leverage global context via attention

These models have been a game-changer for me, particularly when working with jewelry images from different angles and sources.


## From Poses to Depth 🕳️

Once you have camera poses, the next step is generating **depth maps** — which are crucial for any reconstruction pipeline.

- Tools like **MVSNet**, **COLMAP's dense stereo**, or **depth estimation models** help build these maps.
- Depth maps help create sparse and dense point clouds — but quality varies greatly with lighting, reflections, and occlusions.


## Classic vs Modern Methods 🔧

### 🔹 Traditional Photogrammetry
- Pipeline: Feature Matching → SfM → MVS → Meshing
- Tools: COLMAP, Meshroom, Agisoft Metashape
- Limitations: Breaks under poor texture, shiny surfaces, sparse views

### 🔸 Neural Rendering
- **NeRF** opened the door for implicit reconstruction, using neural networks to learn scene radiance fields.
- However, training NeRFs is slow and often doesn’t generalize well to new scenes.

### 🔸 Gaussian Splatting (2023+)
- A faster, more efficient alternative to NeRF
- Better real-time performance and quality tradeoff
- Works well with known camera poses


## Benchmarks & Geometry 📐

When thinking about reconstruction quality, one benchmark is **CAD models**. They're:
- Precise
- Watertight
- Manufacturing-ready

But most real-world pipelines can’t match that fidelity — yet.

I’m especially interested in how **geometry-aware neural fields** (e.g., combining geometry priors with implicit representations) can bridge this gap.


## Ongoing Exploration 🔬

I'm still actively exploring this space, looking at both:
- **Monocular reconstruction**: depth-from-single-image (e.g., DPT, Midas)
- **Multi-view reconstruction**: SfM + MVS + NeRF or Gaussian Splatting

There are exciting open-source models from:
- **Stability AI** (on Hugging Face)
- **Tencent**
- **Mast3r / Dust3r** stack


## Inspiration 💡
One blog that helped me early on:
- [Photogrammetry Explained: From Multi-View Stereo to SfM](https://pyimagesearch.com/2024/10/14/photogrammetry-explained-from-multi-view-stereo-to-structure-from-motion/)


## What’s Next?
I'm continuing to prototype different pipelines and evaluate which combination of camera pose recovery, depth estimation, and neural rendering yields the best quality for **high-fidelity, small-object reconstructions**.

This is especially important for **jewelry**, where even 0.1mm deviations matter.

Let me know if you're working on similar problems — I’d love to compare notes!

