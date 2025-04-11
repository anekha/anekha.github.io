---
title: "Exploring 3D Reconstruction: Classical and Deep Learning Approaches"
date: 2024-12-31
layout: single
classes: wide
categories:
  - research
  - computer vision
  - 3D
excerpt: "Exploring 3D reconstruction methods using photogrammetry, NeRF, Gaussian Splatting, and camera-pose transformers."
tags:
  - 3D reconstruction
  - NeRF
  - Gaussian Splatting
  - Photogrammetry
  - COLMAP
  - Camera Poses
  - Computer Vision
---

## Introduction 💍

It has been a steep and exciting learning curve, but I'm becoming increasingly fascinated by the field of 3D reconstruction. As someone self-taught in this area, I’ve come to appreciate how fundamental 3D assets are to many sectors of AI and digital innovation. Whether you're powering AR/VR applications, simulations, robotics, or gaming — 3D assets form the bedrock.

One of my current areas of exploration is 3D reconstruction of objects using only images. This is challenging due to lighting variation, reflective materials, and lack of standardized metadata.

In this post, I’m documenting my journey through various 3D reconstruction techniques and what I've learned so far.

---

## What Are 3D Assets?

3D assets are digital representations of objects, environments, or characters in three dimensions. These assets are made up of vertices, edges, and faces that define their geometry, often accompanied by textures and materials to enhance realism.

They are essential for:

- Product visualization
- Simulation and training
- Virtual and augmented reality
- Gaming and film
- Scientific modeling

### Types of 3D Models and Their Uses

| Type              | Description                                      | Applications                           |
|-------------------|--------------------------------------------------|----------------------------------------|
| Mesh Models       | Polygon-based geometry (e.g., .obj, .fbx)        | Gaming, AR/VR, design                  |
| CAD Models        | Precise, parametric designs                      | Manufacturing, industrial design       |
| Point Clouds      | Raw 3D data from scanners or photogrammetry      | Mapping, autonomous vehicles           |
| Volumetric Models | Voxels or implicit surfaces (e.g., NeRF, SDFs)   | Neural rendering, medical imaging      |
| Parametric Models | Controlled by parameters                         | Customization and product variation    |

---

## Benchmarks & Geometry 📀

**CAD files** remain the gold standard for accuracy:
- Precision and dimensional consistency
- Watertight surfaces for fabrication

Neural methods are evolving to close the gap between real-time renderable assets and CAD-level fidelity by incorporating geometric priors.

--- 

### Tools for Creating 3D Assets

| Tool             | Use Case                       | Industries                           |
|------------------|--------------------------------|--------------------------------------|
| Rhino            | Precision CAD modeling         | Architecture, industrial design       |
| Blender          | Open-source mesh modeling      | Film, AR/VR, games                    |
| ZBrush           | Organic sculpting              | Art, digital sculpture, gaming       |
| TinkerCAD        | Educational/simple prototyping | Education, hobbyist                  |
| Autodesk Fusion  | CAD and manufacturing          | Engineering, mechanical design        |

---

## Classical 3D Reconstruction Methods

Traditionally, creating 3D models from images involves photogrammetry or Structure from Motion (SfM) (explained below). This process can be broken into:

### General Steps in Classical Reconstruction

- Estimate camera poses (using metadata, feature matching, or calibration)
- Generate depth maps or disparity from image pairs
- Create a sparse point cloud (from matched features)
- Densify the cloud (via MVS or fusion)
- Mesh the point cloud to create a surface

These steps are computationally intensive and often brittle due to camera calibration issues, lighting variation, or poor image overlap.

---

### Key Concepts: Structure from Motion vs Multi-View Stereo 🧠

**Structure from Motion (SfM)** is the process of estimating 3D camera poses and sparse 3D points from unordered image sets. It detects and matches keypoints across views and optimizes the solution using bundle adjustment.

**Multi-View Stereo (MVS)** uses known camera poses (from SfM) to compute dense depth maps by triangulating matched pixels across multiple images. MVS is the step that produces the dense geometry required for meshing.

Together, SfM and MVS are the core components of photogrammetry.

---

## The Foundation: Camera Poses 🫭

Everything starts with accurate **camera poses**. Without them, reconstruction fails.

- [**COLMAP**](https://github.com/colmap/colmap) is a widely used SfM tool but struggles with:
  - Reflective surfaces
  - Images without metadata
  - Sparse feature matching between views

- Manual calibration is time-consuming and doesn’t scale.

- Transformer-based estimators like [**DUST3R**](https://github.com/naver/mast3r), [**Mast3r**](https://github.com/naver/mast3r), and retrieval-augmented models can:
  - Estimate poses without EXIF metadata
  - Work with unordered or weakly related views
  - Use attention mechanisms for better matching

These models have been transformative in enabling pose estimation from unstructured image sets.

---

## From Poses to Depth 🕳️

Once camera poses are estimated, the next step is generating **depth maps**, which are essential for building point clouds.

- Tools: MVSNet, COLMAP’s dense stereo, or monocular/stereo depth models
- Example: [**ZoeDepth**](https://huggingface.co/spaces/shariqfarooq/ZoeDepth)
- Depth quality can vary due to occlusions, reflectivity, and lighting

---

## Deep Learning Approaches

Deep learning offers an exciting shift: replacing hand-engineered pipelines with trainable models.

**Why this matters:**
- Scales to unstructured internet images
- Learns features robust to variation
- Works without camera metadata

Deep learning pipelines often mirror classical approaches, with neural models now replacing or augmenting each stage:

| Classical Step            | Deep Learning Equivalent                        |
|---------------------------|--------------------------------------------------|
| SfM                      | Transformers (e.g., DUST3R, Mast3r)              |
| MVS                      | Depth estimation networks, MVSNet                |
| Point Cloud Generation   | Implicit models, NeRFs, Gaussian Splatting       |
| Surface Meshing          | Implicit surface rendering, marching cubes       |

---

## Classic vs Modern Methods 🔧

### 🔹 Traditional Photogrammetry
- Pipeline: Feature Matching → SfM → MVS → Meshing
- Tools: COLMAP, Meshroom, Agisoft
- Weaknesses: Textureless areas, shiny surfaces, inconsistent viewpoints

### 🔸 Neural Rendering
- **NeRF**: Learns volumetric radiance field from image set
- Pros: High visual quality
- Cons: Slow training, limited generalization

### 🔸 Gaussian Splatting ([Repo](https://github.com/graphdeco-inria/gaussian-splatting))
- Fast, real-time alternative to NeRF
- Uses 3D Gaussians to render scenes
- More efficient and robust with known poses

---

## Ongoing Exploration 🔬

I'm actively exploring both:
- **Monocular reconstruction**: DPT, MiDaS, ZoeDepth
- **Multi-view pipelines**: SfM + MVS + NeRF or Gaussian Splatting

Open-source repos worth checking out:
- [COLMAP](https://github.com/colmap/colmap)
- [Mast3r](https://github.com/naver/mast3r)
- [Mast3r SFM](https://github.com/naver/mast3r/tree/mast3r_sfm)
- [Gaussian Splatting](https://github.com/graphdeco-inria/gaussian-splatting)
- [ZoeDepth (Hugging Face)](https://huggingface.co/spaces/shariqfarooq/ZoeDepth)
- [Stability AI: Virtual Camera](https://huggingface.co/spaces/stabilityai/stable-virtual-camera)
- [Tencent Hunyuan3D](https://huggingface.co/spaces/tencent/Hunyuan3D-2mv)

---

## Inspiration 💡

A great read that helped me early on:
- [Photogrammetry Explained: From Multi-View Stereo to SfM](https://pyimagesearch.com/2024/10/14/photogrammetry-explained-from-multi-view-stereo-to-structure-from-motion/)

---

## What’s Next?

I'm continuing to prototype pipelines to test which combinations of pose estimation, depth recovery, and neural rendering yield the best quality.

This work could help unlock scalable, high-fidelity 3D asset pipelines for real-world applications across industries.

Stay tuned for deeper dives into camera pose estimation, depth prediction, and neural rendering. 💎