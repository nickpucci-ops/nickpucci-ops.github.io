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
  - url: /assets/images/ml-homework/XY-alpha=25.jpg
    image_path: /assets/images/ml-homework/XY-alpha=25.jpg
    alt: "Supervised Learning Prediction"
  - url: /assets/images/ml-homework/BICpredictA(k=6).jpg
    image_path: /assets/images/ml-homework/BICpredictA(k=6).jpg
    alt: "DatasetA k=6 Clusters"
  - url: /assets/images/ml-homework/scale2.jpg
    image_path: /assets/images/ml-homework/scale2.jpg
    alt: "DatasetB k=5 Clusters"

---

# Introduction

This project covers two machine learning tasks from my AI class assignment: predicting a target variable z using supervised learning (Polynomial Ridge Regression) and identifying clusters in unlabeled datasets using unsupervised learning (Gaussian Mixture Models). With 50 samples for supervised learning and two datasets (A and B) for clustering, my goal was to build effective models and visualize the results.

---

## Gallery
{% include gallery class = "full" %}

---

# How to Run the Project
Link to the repo: [SL-and-UL-machine-learning](https://github.com/nickpucci-ops/SL-and-UL-machine-learning.git)
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
python3 xy.py
```
 
## Unsupervised Learning

Implemented in MATLAB. Requires datasets A and B (not provided here). Run the script to fit GMMs and generate cluster visualizations.

---

# Supervised Learning: Polynomial Ridge Regression
## Task 
Provided three csv files (x, y as input attributes and z as target values) I needed to choose a parametric supervised learning method to train multiple models and choose one to predict its z values on unseen data (output to z-predicted.csv).   
## Methodology
First I wanted to see the data and visualize what I was dealing with. The csv files were float values all in one row. So I combined x and y into a 50x2 matrix and wrote a simple matplotlib script to visualize it
- [xy](assets/images/ml-homework/xy.jpg)
Instantly I could see this was a clear cubic trend, so I knew I could use a Polynomial Regression model to train on this data. I used a degree of 3 to represent a cubic function and help the model best match to the trend. But, I would also need to choose a regressor that would regulate the data.However this is also a non-linear relationship with a little bit of noise present. If I used linear regression as my regressor, it would completely overfit to the data as seen below:
- [XY-alpha=0](assets/images/ml-homework/XY-alpha=0.jpg)
This is where my choice of ridge regression comes in. Ridge regression allows me to regulate along the trend and penalize noise and outliers with an alpha constant. The alpha constant multiples an L2 term and determines how strong the regularization will be. Here are some examples of different alpha values I experimented with:
- XY-alpha=10
- [XY-alpha=10](assets/images/ml-homework/XY-alpha=10.jpg)
- XY-alpha=25
- [XY-alpha=25](assets/images/ml-homework/XY-alpha=25.jpg)
- XY-alpha=1000
- [XY-alpha=1000](assets/images/ml-homework/XY-alpha=1000.jpg)
From the graphs, it's clear that alpha=25 offered the best balance between an overfit risk in alpha=10 and an underfit risk in alpha=1000. This gave me enough for me to now land on a learning method and start training a model. 

## Training
I ran 5 iteraions of PolyRidge regression with a degree of 3 and an alpha value of 25 and calculated the MSE for each model. For each iteration, I trained the model on a different random seed to account for variability. For simplicity, I elected the model with the lowest MSE of that alpha value's training cycle. I proceeded to compare those with more training cycles on the alternative alpha values (0, 1, 100, etc.) and after doing some comparisons, I was still satisfied with my alpha=25 choice as it still offered the best fit. 

Now that I had trained my model, I wanted to verify my training method was valid and would work for any given dataset. So, I wrote a script that would generate samples similar to how the given csv files were. Here is the synthetic data generated along with my trained model being applied on it.
- [synthetic-xy](assets/images/ml-homework/synthetic-XY.jpg)
- [XY-synthetic-alpha=25](assets/images/ml-homework/XY-synthetic-alpha=25.jpg)

---
# Unsupervised Learning: Gaussian Mixture Models
## Task
Provided two datasets (A and B) I needed to use an unsupervised learning method to train a model to identify (k) clusters in the dataset and provide the means and covariances. Some clusters were repeated between both datasets and so I would need to figure out which clusters those were as well.
## Methodology
The assignment stated undergraduates would use K-meamns while graduate students must use Gaussian Mixture Models (GMM). I opted to take the GMM route though I was still an undergrad.  
- DatasetA.jpg
- DatasetB.jpg

When applying the model, I consistently was getting k=5 clusters for DatasetB, meanwhile DatasetA was trickier and would often get either k=6 or k=7

## Justification

Although I was an undergraduate, I wanted to try using GMM to complete this assignment because GMM is better for identifying non-spherical (or ellipsoidal) clusters. The challenge for grad students was that i 

# Conclusion

I really liked this part and found it to be super interesting to solve. It was great first exposure to supervised learning methods and also the thinking process of determining the best course of action in machine learning applications.   


# Methodology
## Supervised Learning: Polynomial Ridge Regression
- Model: Polynomial degree 3 with Ridge regularization (alpha = 25.0).
- Process:
        Combined x and y into a feature matrix, scaled with StandardScaler.
        Trained on 80% of 50 samples, tested on 20%, with 5 random restarts to pick the best model (lowest test MSE).
- Why?: The scatter plots showed a cubic trend. Ridge prevented overfitting on this small, slightly noisy dataset.

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


