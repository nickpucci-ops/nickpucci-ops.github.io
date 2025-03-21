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
    alt: "Supervised Learning Prediction (alpha=25)"
  - url: /assets/images/ml-homework/BICpredictA(k=6).jpg
    image_path: /assets/images/ml-homework/BICpredictA(k=6).jpg
    alt: "DatasetA k=6 Clusters"
  - url: /assets/images/ml-homework/scale2.jpg
    image_path: /assets/images/ml-homework/scale2.jpg
    alt: "DatasetB k=5 Clusters"
---

# Introduction

This project is from my CS465 AI class, where I tackled two machine learning tasks. For supervised learning, I predicted a target variable `z` from `x` and `y` using Polynomial Ridge Regression. For unsupervised learning, I clustered two datasets (A and B) with Gaussian Mixture Models (GMM). With just 50 samples for supervised and two mystery datasets for unsupervised, I wanted to figure out solid models and show my results with visuals. Here’s how I did it!

---

# How to Run It

Repo: [SL-and-UL-machine-learning](https://github.com/nickpucci-ops/SL-and-UL-machine-learning.git)

## Supervised Learning
- **Dependencies**:
  ```bash
  pip install scikit-learn numpy pandas
```

- **Steps**:
  1. Clone the repo.
  2. Train the model: `python3 train_model.py`
  3. Visualize results: `python3 visualize_model.py`

- **Other useful scripts**
  1. Generate synthetic data: `python3 generate_samples.py`
  2. Plot x and y vs. z: `python3 xy.py`

## Unsupervised Learning
- Done in MATLAB. Needs datasets A and B (not shared here). Run the script to fit GMMs and see cluster plots.

---

# Supervised Learning: Polynomial Ridge Regression

## The Task
## Objective
I was provided with three CSV files: x and y as input attributes and z as the target variable. Each contains 50 float values in a single row. The task was to select a supervised learning method, train multiple models, and predict z for unseen data, outputting the results to z-predicted.csv.

## Approach
I first wanted to visualize what I was working with to see if there were any trends or patterns I could identify. I stacked x and y into a 50x2 matrix and plotted them against z with a quick matplotlib script:
- [xy.jpg](/assets/images/ml-homework/xy.jpg)

Immediately what stood out to me was that this data was non-linear and followed a cubic trend. This meant I could use Polynomial Regression to follow the polynomial function. Because it's cubic, that means I would use a degree 3 to match the curve. But there’s also slight noise in the data, and a plain linear regressor would risk overfitting. So we need a way to regularize against this. This is when I discovered Ridge Regression, but first to demonstrate, here is an example of a trained model that utilizes a linear regressor (alpha=0, no regularization):
- [XY-alpha=0.jpg](/assets/images/ml-homework/XY-alpha=0.jpg)

The predicted z values (red) align exactly with the original data which is a clear assign of severe overfitting. So, I combined polynomial regresssion with ridge regression to smooth out the predictions to more closely follow an underlining trend. 
Ridge adds an L2 penalty via an alpha constant. A higher alpha means more noise control. I ran a couple more tests to find an optimal alpha constant [10, 25, 1000]:
- [XY-alpha=10.jpg](/assets/images/ml-homework/XY-alpha=10.jpg)
- [XY-alpha=25.jpg](/assets/images/ml-homework/XY-alpha=25.jpg)
- [XY-alpha=1000.jpg](/assets/images/ml-homework/XY-alpha=1000.jpg)

Alpha=25 looked best—followed the trend without freaking out over noise.

## Training and Testing
I trained 5 models with degree 3 and alpha=25, each with a different random seed (80% train, 20% test). I picked the one with the lowest test MSE. To double-check, I compared it against alpha=0, 10, 100, etc., and 25 still ruled. Since `z` was huge (billions!), I used `StandardScaler` to normalize everything, then unscaled predictions to match the original scale.

To prove it wasn’t a fluke, I made synthetic data mimicking the CSV files—100 samples with some extra large noise:
- [synthetic-xy.jpg](/assets/images/ml-homework/synthetic-xy.jpg)
- [XY-synthetic-alpha=25.jpg](/assets/images/ml-homework/XY-synthetic-alpha=25.jpg)

The model nailed it, balancing fit and generalization.

---

# Unsupervised Learning: Gaussian Mixture Models

## The Task
I got two datasets, A and B, and had to find clusters (k), their means, and covariances using an unsupervised method. Some clusters might repeat between datasets, so I had to spot those too.

## My Process
The assignment said undergrads use K-means, but grad students use GMM. I’m an undergrad, but I went GMM anyway—heard it’s big in data science and handles funky, ellipsoidal clusters better than K-means’ boring spheres.

I used MATLAB and the Bayesian Information Criterion (BIC) to pick `k`, testing 1 to 10. DatasetB was easy—BIC almost always said `k=5`, and the scatter plots agreed:
- [scale2.jpg](/assets/images/ml-homework/scale2.jpg)

DatasetA was trickier. BIC flipped between `k=6` and `k=7`. I eyeballed the plots and saw overlap around `x:-5 to 10, y:-5 to 10`. Here’s `k=6`:
- [BICpredictA(k=6).jpg](/assets/images/ml-homework/BICpredictA(k=6).jpg)

And `k=7` sometimes split clusters too much, so I stuck with `k=6`—it felt safer and matched the data’s vibe.

## Finding Repeated Clusters
I compared means and covariances across datasets. Two stood out:
- DatasetA Cluster 3 (mean: 2.92, 4.42) and DatasetB Cluster 4 (mean: 2.04, 4.23)—close means, same spot, but different shapes.
- DatasetA Cluster 1 (mean: 12.36, 5.97) and DatasetB Cluster 1 (mean: 12.96, 3.95)—similar `x`, but covariances shifted their look.

Means say “repeated,” but covariances say “not quite identical.” Cool twist!

---

# Gallery
{% include gallery class="full" %}

---

# Why I Chose These Methods

## Supervised: Why PolyRidge?
The cubic trend was obvious, and 50 samples ruled out fancy stuff like SVR or neural networks—too much tuning, not enough data. Polynomial Regression was simple, and Ridge kept the noise in check. Alpha=25 avoided overfitting (alpha=0) and underfitting (alpha=1000).

## Unsupervised: Why GMM?
GMM’s probabilistic vibe fit the ellipsoidal clusters I saw. K-means assumes spheres—lame for this data. `k=6` for DatasetA avoided over-segmenting (unlike `k=7`), and `k=5` for DatasetB was a no-brainer.

---

# Conclusion

This was my first real swing at machine learning, and I loved it! For supervised, PolyRidge with alpha=25 nailed the cubic trend without overfitting. For unsupervised, GMM uncovered `k=6` and `k=5` clusters and taught me how means and covariances tell different stories. Figuring out the “why” behind my choices—like regularization or BIC—was the best part. Solid intro to ML!
