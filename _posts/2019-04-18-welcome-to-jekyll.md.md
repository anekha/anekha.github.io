---
title: "Welcome to Jekyll!"
date: 2019-04-18T15:34:30-04:00
categories:
  - blog
tags:
  - Jekyll
  - update
---


[//]: # (---)

[//]: # (title: "The Student Forecaster Model")

[//]: # (date: 2024-08-15)

[//]: # (categories:)

[//]: # (  - machine-learning)

[//]: # (tags:)

[//]: # (  - education)

[//]: # (  - streamlit)

[//]: # (  - data-analysis)

[//]: # (  - visualization)

[//]: # (---)

As part of the **Le Wagon Bootcamp**, this was my final group project:

> 🧠 **Proven Hypothesis**  
> *The academic performance of a student depends not only on their academic capabilities but also their socio-economic status.*

### 🏫 Imagine this...

You're a headteacher managing a school of thousands of students. How do you create an ideal environment where every student can thrive?

<p align="center">
  <img src="https://images.pexels.com/photos/5212700/pexels-photo-5212700.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=2" width="80%">
</p>

---

## 🎯 Project Goal

To build a **machine learning model** that forecasts academic outcomes using a dataset of Portuguese students.  
The objective was to uncover **socio-economic drivers** influencing final grades (G3) and offer a predictive dashboard for education leaders.

🔗 **Try the app**: [The Student Forecaster Model](https://student-forecaster.streamlit.app/)

---

## 🔍 Data Exploration

We started with a raw dataset and trimmed irrelevant or misleading variables like:

- Home address
- Parent jobs (too many "other" values)
- Nursery attendance
- G1/G2 (used G3 only as target)

We then grouped related features:
- Alcohol intake (weekday/weekend)
- Family dynamics
- Time management
- Educational support
- Reason for school choice
- Parental education

Tools used:
- **Heatmaps**
- **Boxplots**
- **Group binning**
- **Value counts**
- **Correlation analysis**

---

## ⚙️ Model Training

After feature engineering and encoding:
- Split data into train/test sets
- Used **Gradient Boosting Classifier**
- Preprocessing: scaling + one-hot encoding

We tested multiple models for comparison:

| Model                     | Precision | Test vs Train | Overfitting |
|--------------------------|-----------|---------------|-------------|
| Logistic Regression      | 0.75      | 0.72 vs 0.72  | ❌          |
| KNN                      | 0.76      | 0.67 vs 0.83  | ✅          |
| Random Forest            | 0.81      | 0.70 vs 0.98  | ✅          |
| XGBoost                  | 0.82      | 0.68 vs 0.95  | ✅          |
| **Gradient Boosting**    | **0.76**  | **0.70 vs 0.78** | ✅ Slight |

---

## 🧪 Final Metrics (on test data)

- 🎯 **Accuracy**: `0.96`
- 📊 **Precision**: `0.95`
- 🔍 **Recall**: `0.99`
- 🧮 **F1 Score**: `0.97`

These metrics show strong performance on the binary classification task: predicting whether a student would pass or fail based on inputs.

---

## 🎓 Insights

- Students who had failed before are more likely to receive support and improve.
- Mother's education had stronger correlation than father's.
- Study time and school choice were significant drivers.
- Most students want higher education, causing class imbalance.

---

## 🧠 Tools Used

- Python: `pandas`, `seaborn`, `scikit-learn`, `joblib`
- Web app: **Streamlit**
- Deployment: Streamlit Cloud

---

## 🌐 App Link

👉 [student-forecaster.streamlit.app](https://student-forecaster.streamlit.app/)

---

## Additional Resources 📎

For a detailed explanation:

1. 📊 **Data Analysis & Insights** — View the [presentation slides](/assets/projects/StudentForecaster/presentation.slides.html)
2. 🧠 **Model Training & Evaluation** — Explore the [Jupyter Notebook](/assets/projects/StudentForecaster/presentation.ipynb)
3. 💻 **Codebase & App** — Visit the [GitHub repository](https://github.com/anekha/streamlit_ml_for_learners)

---

## 💬 Final Thoughts

This project brought together all the fundamentals of data science — from data wrangling and EDA to machine learning and user deployment — to solve a real-world education problem.

> Let’s build data tools that *truly support* educators and learners alike. 🌍📚