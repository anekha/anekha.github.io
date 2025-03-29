---
title: "Exploring GaussianObject: 3D Reconstruction from Sparse Views"
date: 2025-01-31
layout: single
classes: wide
categories:
  - research
  - computer vision
  - 3D
excerpt: "Testing the GaussianObject pipeline for sparse-view 3D object reconstruction using Gaussian Splatting."
tags:
  - 3D reconstruction
  - Gaussian Splatting
  - NeRF
  - Camera Poses
  - Self-supervised learning
  - COLMAP
---

## Introduction 🔍

I became interested in **Gaussian Splatting** after seeing its promising results in rendering high-quality 3D scenes in real-time. One consistent requirement, though, is the need for **accurate camera poses** — often derived from COLMAP — which isn’t always reliable or feasible, especially with casual, real-world imagery.

That’s when I came across the paper:

> **GaussianObject: High-Quality 3D Object Reconstruction from Four Views with Gaussian Splatting**  
> **SIGGRAPH Asia 2024 (ACM TOG)**  
> [Project page](https://gaussianobject.github.io/) | [GitHub](https://github.com/chensjtu/GaussianObject)

The idea that stood out most was:  
> *"COLMAP-free 3D object reconstruction with only **4 input images**, using Gaussian Splatting + diffusion repair."*

## Why This Was Exciting

- No need for accurate poses (optional COLMAP-free mode!)
- Works with as few as **4 images**
- Uses a clever **visual hull + repair** strategy
- Introduces diffusion-based Gaussian repair with LoRA

So I gave it a try — running the model across a variety of object image sets (from 4 to 40 images), and setting it up on **Modal** for GPU access.

---

## What I Found 🧪

- **SAM (Segment Anything)** was critical to generate good masks — otherwise performance dropped.
- **Reflective or metallic objects** were particularly hard to reconstruct.
- **Smaller objects** were challenging — due to scale and mask precision.
- Despite being "COLMAP-free", having **better pose estimates** significantly helped reconstruction.
- The input images need **some view consistency** — widely differing angles didn’t work well.
- **Processing more than 12 images** dramatically increased rendering time — 40 images took 12+ hours!

They currently use **MASt3R** for pose estimation, but a new **MASt3R-SfM** (retrieval-based) version is out — I’d love to test how that affects results.

---

## Pipeline Overview 🧵

GaussianObject consists of multiple stages:

<img src="/assets/projects/gaussiansplatting/gaussianobjectpipeline.png" alt="GaussianObject Pipeline" style="width:100%; border-radius: 8px;">

> 📌 *Diagram from the official [project page](https://gaussianobject.github.io/). All credits to the original authors.*

### (a) Visual Hull & Optimization
- Create an initial 3D Gaussian representation using masks + camera parameters
- Refine using floater elimination

### (b) Leave-One-Out Training
- Add 3D noise to Gaussians to create corrupted renderings
- Use these to train the **repair model** via paired clean-corrupted image supervision

### (c) Gaussian Repair
- Identify views that need repairing (distance-aware sampling)
- Use diffusion LoRA to refine + repair the Gaussians

---

## Setup Instructions ⚙️

I followed the steps on the [GitHub repo](https://github.com/chensjtu/GaussianObject), summarized below.

### 🧬 Environment
```bash
# Clone repo with submodules
git clone https://github.com/chensjtu/GaussianObject.git --recursive

# Install PyTorch (CUDA 11.8)
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu118

# Install Python deps
pip install -r requirements.txt
```

### 🖼️ Prepare Your Data
```bash
# Download SAM + DUST3R/MAST3R
cd models
sh download_preprocess_models.sh
cd ..

# Structure:
GaussianObject/
└── data/<your_dataset>/
    └── images/
        ├── 0001.png
        ├── 0002.png
        ├── ...
    └── sparse_4.txt
    └── sparse_test.txt
```

Use SAM to generate masks, and DUST3R/MAST3R to estimate poses:
```bash
python pred_poses.py -s data/<your_dataset> --sparse_num 4
```

---

## Training Stages 📈

### 1. Visual Hull Initialization
```bash
python visual_hull.py --sparse_id 4 --data_dir data/<your_dataset> --reso 2 --not_vis
```

### 2. Coarse Gaussian Splatting
```bash
python train_gs.py -s data/<your_dataset> -m output/gs_init/<your_dataset> \
    -r 4 --sparse_view_num 4 --init_pcd_name visual_hull_4 \
    --white_background --random_background
```

### 3. Leave-One-Out Training
```bash
python leave_one_out_stage1.py ...
python leave_one_out_stage2.py ...
```

### 4. LoRA Fine-Tuning + Repair
```bash
python train_lora.py ...
python train_repair.py ...
```

### 5. Rendering
```bash
python render.py -m output/gs_init/<your_dataset> \
  --white_background --render_path --use_dust3r \
  --load_ply output/gaussian_object/<your_dataset>/save/last.ply
```

> 🧩 See [full GitHub repo](https://github.com/chensjtu/GaussianObject) for command options.

---

## Final Thoughts 🧠

Despite the complexity, I was genuinely impressed by the results given so little input (just 4 images!). That said, performance on **reflective, fine-grained, or small-scale objects** still leaves room for improvement.

The reliance on good segmentation and view coverage also means **pose quality still matters**, even in COLMAP-free mode. I’m curious how it’ll perform with the new [**MASt3R-SfM** retrieval model](https://github.com/naver/mast3r/tree/mast3r_sfm).

---

## References & Links 📚

- [Paper](https://gaussianobject.github.io/)
- [GitHub Repository](https://github.com/chensjtu/GaussianObject)
- [MASt3R-SfM](https://github.com/naver/mast3r/tree/mast3r_sfm)
- [SAM (Segment Anything)](https://github.com/facebookresearch/segment-anything)
- [Modal](https://modal.com) for GPU cloud execution

> Reference: Yang et al., *GaussianObject: High-Quality 3D Object Reconstruction from Four Views with Gaussian Splatting*, ACM TOG 2024.
