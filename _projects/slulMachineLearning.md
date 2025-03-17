---
title: "Machine Learning: Supervised and Unsupervised Learning"
excerpt: "Predicting values with Polynomial Ridge Regression and clustering with Gaussian Mixture Models"
date: 2025-03-17
collection: projects
layout: single
header:
  image: /assets/images/ml-homework/XY-synthetic-alpha=25.jpg
  teaser: /assets/images/ml-homework/scale2.jpg
toc: true
toc_label: "Table of Contents"
sidebar:
  - title: "Tools Used"
    text: "Python, scikit-learn, MATLAB, numpy, pandas, matplotlib"
gallery:
  - url: /assets/images/ml-homework/xy-alpha-25.jpg
    image_path: /assets/images/ml-homework/xy-alpha-25.jpg
    alt: "Supervised Learning Prediction"
  - url: /assets/images/ml-homework/datasetA-k6.jpg
    image_path: /assets/images/ml-homework/datasetA-k6.jpg
    alt: "DatasetA k=6 Clusters"
  - url: /assets/images/ml-homework/datasetB-k5.jpg
    image_path: /assets/images/ml-homework/datasetB-k5.jpg
    alt: "DatasetB k=5 Clusters"
---

# Introduction

This project covers two machine learning tasks from my AI class assignment: predicting a target variable z using supervised learning (Polynomial Ridge Regression) and identifying clusters in unlabeled datasets using unsupervised learning (Gaussian Mixture Models). With 50 samples for supervised learning and two datasets (A and B) for clustering, my goal was to build effective models and visualize the results.

---

# How to Run the Project

## Supervised Learning
### Dependecies

```bash
pip install scikit-learn numpy pandas
```
### Scripts
- Clone the repo to your root directory.
- To train: 
```bash
python3 train_model.py
```
- To visualize:
```bash
python3 visualize_model.py
```
- To generate synthetic data:
```bash
python3 generate_samples.py
```
- To visualize x.csv and y.csv against z.csv
```bash
python2 xy.py
```
 
## Unsupervised Learning

Implemented in MATLAB. Requires datasets A and B (not provided here). Run the script to fit GMMs and generate cluster visualizations.

---

# Methodology
## Supervised Learning: Polynomial Ridge Regression
- Model: Polynomial degree 3 with Ridge regularization (alpha = 25.0).
- Process:
        Combined x and y into a feature matrix, scaled with StandardScaler.
        Trained on 80% of 50 samples, tested on 20%, with 5 random restarts to pick the best model (lowest test MSE).
- Why?: Scatter plots showed a cubic trend. Ridge prevented overfitting on this small, slightly noisy dataset.

### Normalization

Given the large scale of z (in billions), normalization was necessary to prevent numerical instability and ensure effective learning. I used `StandardScaler` to standardize x, y, and z, transforming them to have zero mean and unit variance.

## Unsupervised Learning: Gaussian Mixture Models (GMM)
- Model: GMM with Bayesian Information Criterion (BIC) to select cluster count k.
- Process:
        DatasetA: Tested k=1 to 10, chose k=6 for balanced fit.
        DatasetB: BIC consistently picked k=5.
        Visualized clusters with means, covariances, and ellipses.
- Why?: GMM handles non-spherical clusters better than K-means, fitting the ellipsoidal data shapes.

# Results
## Supervised Learning
- Prediction: Captured the cubic relationship well with minimal overfitting.
- Visualization: Predicted z vs. actual z shows a tight fit with alpha = 25.0 balancing noise.
show picture here
XY-alpha=25
## Unsupervised Learning
- DatasetA (k=6): Six clusters captured the main groups without over-segmenting
pic
DatasetAk=6
- DatasetB (k=5): Five stable clusters matched BIC and visuals.
pic
DatasetBk=5

## Gallery
{% include gallery class = "full" %}

# Justification
## Supversied Learning
- Why PolyRidge?: Simpler than SVR or neural networks for 50 samples; cubic fit matched the data; Ridge handled noise.
- Trade-offs: Degree 3 with alpha = 25.0 avoided overfitting (alpha = 0) and underfitting (alpha = 1000).
### Regularization: Ridge Regression
Instead of ordinary least squares regression, I opted for ridge regression to prevent overfitting. Ridge regression adds an L2 penalty to large coefficient values and can improve model generalization. I set alpha = 1.0 to balance fit and complexity.

## Unsupervised Learning
- Why GMM?: Probabilistic clustering suited the overlapping, ellipsoidal data over K-means’ spherical assumption.
- Trade-offs: k=6 (DatasetA) was safer than k=7 (over-segmented); k=5 (DatasetB) was consistent.

---
This project highlights the practical considerations in polynomial regression, feature engineering, and regularization, demonstrating how thoughtful model selection improves predictive accuracy and generalization.


