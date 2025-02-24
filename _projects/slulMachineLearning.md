---
title: "Machine Learning: SL and UL"
excerpt: "An analysis of supervised learning and unsupervised learning processes"
date: 2025-02-23
collection: projects
layout: single
header:
  image: /assets/images/slulml/ML11.jpg
  teaser: /assets/images/slulml/ML11.jpg
toc: true
toc_label: "Table of Contents"
sidebar:
  - title: "Tools Used"
    text: "Python, scikit-learn, matplotlib, numpy"
gallery:
  - url: /assets/images/slulml/ML1.jpg
    image_path: /assets/images/slulml/ML1.jpg
    alt: "Screenshots"
  - url: /assets/images/slulml/ML2.jpg
    image_path: /assets/images/slulml/ML2.jpg
    alt: "Screenshots"
  - url: /assets/images/slulml/ML3.jpg
    image_path: /assets/images/slulml/ML3.jpg
    alt: "Screenshots"
  - url: /assets/images/slulml/ML4.jpg
    image_path: /assets/images/slulml/ML4.jpg
    alt: "Screenshots"
  - url: /assets/images/slulml/ML5.jpg
    image_path: /assets/images/slulml/ML5.jpg
    alt: "Screenshots"
---

# Introduction

This project explores supervised learning using polynomial regression with ridge regularization to predict a target variable \(z\) based on two input attributes \(x\) and \(y\). The dataset consists of 50 noisy samples, and the objective is to develop a model that captures nonlinear relationships while maintaining generalizability. This report details my thought process in model selection, feature engineering, regularization, and evaluation. This was a neat assignment for my CS465 AI class.

---

# How to Run the Project

## Prerequisites

Ensure you have Python 3 installed along with the following dependencies:

- pandas
- numpy
- scikit-learn
- matplotlib

## Running the Code

1. Clone this repository and navigate to the `supervised-learning` directory.
2. Place `x.csv`, `y.csv`, and `z.csv` in the directory.
3. Run the script using:
   ```sh
   python3 script.py
   ```
4. The predicted values will be output to `z-predicted.csv` upon completion.

---

# Methodology

## Choosing the Model: Polynomial Regression

From an initial analysis of the dataset, scatter plots of \(x\) vs \(z\) and \(y\) vs \(z\) indicated a nonlinear relationship. To model this relationship, I chose polynomial regression with a degree of 2, implemented using `PolynomialFeatures` from scikit-learn.

## Feature Expansion

To transform the dataset for polynomial regression, I constructed a feature matrix from \(x\) and \(y\), which included:

- \(1\) (intercept)
- \(x\), \(y\) (linear terms)
- \(x^2\), \(xy\), \(y^2\) (quadratic terms)

{% include gallery class = "full" %}

## Normalization

Given the large scale of \(z\) (in billions), normalization was necessary to prevent numerical instability and ensure effective learning. I used `StandardScaler` to standardize \(x\), \(y\), and \(z\), transforming them to have zero mean and unit variance.

## Regularization: Ridge Regression

Instead of ordinary least squares regression, I opted for ridge regression to prevent overfitting. Ridge regression adds an L2 penalty to large coefficient values, improving model generalization. I set \(\alpha = 1.0\) to balance fit and complexity.

---

# Justification for Model Choices

## Degree Selection

- **Degree 2 (Preferred Model)**: Captures nonlinearity while remaining simple and interpretable. It provides a good balance between bias and variance.
- **Degree 3 (Alternative Model)**: Includes additional cubic terms, increasing flexibility but also the risk of overfitting, particularly with limited data.

I tested degree 3, but it resulted in a tighter fit with excessive sensitivity to noise, suggesting overfitting in sparse regions. Thus, I prioritized degree 2 to ensure robustness.

## Ridge Regularization

- Without regularization, polynomial regression is highly susceptible to overfitting.
- Ridge regression controls model complexity by penalizing large coefficients, leading to better generalization.
- I experimented with different \(\alpha\) values and found that \(\alpha = 1.0\) for degree 2 provided the best trade-off between bias and variance.

---

# Implementation

## Data Preprocessing

1. Load \(x\), \(y\), and \(z\) values from CSV files.
2. Standardize \(x\), \(y\), and \(z\) for numerical stability.

## Model Training and Prediction

1. **Degree 2 Model**:
   - Transform features using `PolynomialFeatures(degree=2)`.
   - Train a ridge regression model with \(\alpha = 1.0\).
   - Predict \(z\) and unscale it to match the original scale.
2. **Degree 3 Model (for comparison)**:
   - Use `PolynomialFeatures(degree=3)` to add cubic terms.
   - Train a ridge regression model with a higher regularization strength (\(\alpha = 100.0\)) to mitigate overfitting.

# Visualization

Four scatter plots are generated:

1. \(x\) vs \(z\) (raw data)
2. \(y\) vs \(z\) (raw data)
3. **Degree 2**: Actual vs. Predicted \(z\) (Preferred Model)
4. **Degree 3**: Actual vs. Predicted \(z\) (Overfit Model)

---

## Trade-offs Between Model Complexity and Empirical Loss

| Model        | Parameters                  | Overfitting Risk | Fit Quality              |
| ------------ | --------------------------- | ---------------- | ------------------------ |
| **Degree 2** | 6 (1, x, y, x², xy, y²)     | Low              | Good generalization      |
| **Degree 3** | 10 (additional cubic terms) | High             | Risk of memorizing noise |

- **Empirical Fit:** Degree 2 model predictions cluster around the 45-degree line with moderate spread, suggesting good generalization.
- **Overfitting Risk:** Degree 3 model provides a tighter fit but overfits sparse regions, decreasing reliability on unseen data.

I finalized degree 2 as the optimal balance of complexity and predictive performance.

---

# Results and Conclusion

- **Degree 2 model** successfully captures nonlinear relationships while avoiding overfitting.
- **Degree 3 model** demonstrates overfitting tendencies, emphasizing the importance of careful complexity control.
- **Ridge regression** effectively balances fit and complexity by penalizing excessive coefficient magnitudes.

### Next Steps

- **Part 2** Fit gaussians to HW2 dataset to identify the number of parameters of clusters of similar data.

---

This project highlights the practical considerations in polynomial regression, feature engineering, and regularization, demonstrating how thoughtful model selection improves predictive accuracy and generalization.


