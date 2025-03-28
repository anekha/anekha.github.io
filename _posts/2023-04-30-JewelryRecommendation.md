---
title: "Jewelry Recommendation System"
date: 2025-03-28
categories:
  - machine-learning
tags:
  - computer-vision
  - streamlit
  - fashion
  - personalization
class: wide
excerpt: "A Streamlit-powered app that recommends jewelry using facial features and color analysis."
---

## Background

This project was built as part of my **Machine Learning course** at Rice University. I developed a **Jewelry Recommendation System** that personalizes jewelry design suggestions based on a user's face shape and skin undertone using **computer vision** and a **Streamlit app**.

## 🧠 Problem Statement
How can we use machine learning and color theory to suggest jewelry that complements an individual’s unique facial features?

## 📊 Dataset
We used a publicly available dataset from Kaggle with over **5,000 labeled celebrity images** across 5 face shapes: Oval, Round, Square, Heart, and Oblong.  
[Kaggle Dataset →](https://www.kaggle.com/datasets/niten19/face-shape-dataset)

<p align="center">
  <img src="/assets/projects/jewelryrecommendation/dataset-structure.png" alt="Dataset Structure" width="40%">
</p>

## 💻 Tech Stack
- **Face Detection:** MTCNN for facial landmarks
- **Face Shape Classification:** Custom CNN using TensorFlow/Keras
- **Skin Tone Extraction:** Dominant color detection with OpenCV
- **App Interface:** Streamlit

## 🧬 Model Pipeline
1. Upload image
2. Detect facial landmarks and extract dominant colors
3. Classify face shape (CNN)
4. Perform color clustering to infer undertone
5. Generate jewelry, gemstone, and metal color recommendations

Here’s a snapshot of the app in action:

## 🖼️ Application Demo

Below are screenshots of the application UI and results:

![App Screenshot](/assets/projects/jewelryrecommendation/app_screenshot.png)
<div style="margin-top: 40px;"></div>
![App UI Example](/assets/projects/jewelryrecommendation/app_ex.png)

## 🧠 Recommendation Logic

We hardcoded the jewelry logic based on domain knowledge:

```python
def recommend_jewelry_face_shape(face_shape):
    recommendations = {
        "oval": ["Drop earrings, hoops...", "Layered necklaces..."],
        ...
    }
    return recommendations.get(face_shape.lower(), "No recommendation")
```

## 💡 Future Improvements

1. **Learning from user feedback:**  
   Incorporate aesthetic ratings to fine-tune recommendations using reinforcement learning or similarity metrics.

2. **Multimodal modeling:**  
   Explore neural networks that take both face shape and undertone embeddings for holistic recommendations.

3. **Jewelry embeddings:**  
   Fine-tune CLIP-style models to learn representations of jewelry pieces and generate recommendations based on visual similarity.

---

## 📎 Additional Resources

- 📄 [Final Poster Presentation (PDF)](/assets/projects/jewelryrecommendation/project_presentation.pdf)  
- 🌐 [Streamlit App (Demo)](https://jewelrecs.streamlit.app/)  
- 📁 [GitHub Repository](https://github.com/anekha/jewelry_recommendations)

---

*Built with 💎 and Python.*