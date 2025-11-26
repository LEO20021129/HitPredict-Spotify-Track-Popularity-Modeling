## Spotify Track Popularity Prediction

**Course:** STSCI 5740 – Data Mining & Machine Learning  
**Goal:** Predict Spotify track popularity from audio features and metadata, and compare classical and modern ML methods for both regression and classification.

---

### Project Overview

This project uses the **Spotify Tracks Dataset** (≈114K songs, 21 variables) to answer two questions:

1. **Regression:** How accurately can we predict a track’s numerical *popularity score* (0–100) from its audio features and basic metadata?
2. **Classification:** Can we distinguish **high-popularity tracks** from the rest, and which modeling approaches work best under strong class imbalance?

Beyond prediction, the project also explores **which song characteristics are associated with higher popularity**, such as genre, danceability, energy, and acousticness.

---

### Data

- ~114,000 tracks from Spotify with:
  - **Target (regression):** `popularity` (0–100)
  - **Target (classification):** `high_pop` (High if popularity ≥ 60, Low otherwise)
  - **Audio features:** `danceability`, `energy`, `valence`, `speechiness`, `acousticness`, `instrumentalness`, `liveness`, `loudness`, `tempo`, `duration_ms`
  - **Metadata:** `explicit`, `track_genre`, musical `key`, `mode`, `time_signature`
- Preprocessing:
  - Dropped non-informative ID/text fields (`track_name`, `album_name`, `artists`, `track_id`)
  - Encoded `explicit`, `key`, `mode`, `time_signature`, `track_genre` as factors
  - Train–test split: **70% train / 30% test**
  - Centered & scaled numeric predictors where appropriate (e.g., LDA/QDA/kNN/PCR)

---

### Methods

#### Regression Models
- Baselines:
  - Simple linear regression with one predictor (e.g., `popularity ~ danceability`)
- Linear models:
  - Multiple linear regression (full model)
  - Stepwise selection (AIC, BIC)
- Regularization:
  - **Ridge regression**
  - **Lasso regression**
  - **Principal Component Regression (PCR)**
- Nonparametric / tree-based:
  - **k-Nearest Neighbors (kNN) regression**
  - **Regression tree (CART)**
  - **Random forest**
  - **Gradient Boosting (GBM)**

#### Classification Models (High vs. Low Popularity)
- **LDA (Linear Discriminant Analysis)**
- **QDA (Quadratic Discriminant Analysis)**
- **Gaussian Naive Bayes**
- **kNN classification**
- **Classification tree (CART)**
- **GBM classifier**

Evaluation metrics:
- **Regression:** RMSE, R², MAE on the test set  
- **Classification:** Accuracy, Sensitivity, Specificity, AUC (ROC) on the test set

---

### Key Results

#### Regression (Predicting Popularity)

| Model                 | Test RMSE | Test R² | Test MAE |
|-----------------------|-----------|---------|----------|
| LM (single feature)   | 22.26     | 0.001   | 18.83    |
| LM (full)             | 19.11     | 0.263   | 14.04    |
| Ridge / Lasso         | ~19.11–19.13 | ~0.262–0.263 | ~14.0 |
| PCR (128 PCs)         | 19.20     | 0.257   | 14.15    |
| kNN regression        | 19.60     | 0.230   | 14.40    |
| Regression tree       | 19.17     | 0.259   | 14.07    |
| **Random forest**     | **18.27** | **0.334** | **13.89** |
| GBM regression        | 20.30     | 0.204   | 16.65    |

- **Best regression model:** Random forest  
- Even the best model explains only **≈33% of the variance** in popularity → many real-world factors (marketing, artist fame, playlist placement, etc.) are not in the dataset.

#### Classification (High vs. Low Popularity)

| Model         | Accuracy | Sensitivity | Specificity | AUC   |
|--------------|----------|-------------|-------------|-------|
| **LDA**      | 0.869    | 0.259       | 0.958       | 0.826 |
| QDA          | 0.777    | 0.305       | 0.847       | 0.658 |
| Naive Bayes  | 0.741    | 0.356       | 0.798       | 0.637 |
| kNN (k=25)   | 0.876    | 0.149       | 0.983       | 0.804 |
| CART         | 0.879    | 0.098       | 0.994       | 0.552 |
| GBM          | 0.875    | 0.041       | 0.998       | 0.781 |

- **Best classifier overall:** **LDA** (highest AUC with balanced sensitivity/specificity)  
- Many models (kNN, CART, GBM) are **very conservative**, predicting “High” rarely → high specificity but very low sensitivity due to class imbalance (~13% High).

---

### Insights

- **Genre matters:** Pop and dance-pop genres are associated with higher popularity; some rock/metal/niche genres are associated with lower predicted popularity.
- **Energy & loudness:** High-energy, louder tracks tend to be more popular.
- **Danceability & valence:** More danceable, “happier” tracks get a mild popularity boost.
- **Acousticness / instrumentalness:** Highly acoustic or purely instrumental tracks tend to be less popular on average.

Overall, **audio features + basic metadata can help rank tracks and identify patterns**, but they are far from sufficient to fully explain or predict which songs become hits.

---

### Tools & Skills

- **Language:** R  
- **Libraries:** `tidyverse`, `caret`, `glmnet`, `pls`, `rpart`, `randomForest`, `gbm`, `MASS`, `e1071`  
- **Techniques:**  
  - Data cleaning & feature engineering  
  - Train–test split, cross-validation  
  - Linear models, stepwise selection, regularization (ridge, lasso)  
  - PCR, kNN, LDA/QDA, Naive Bayes  
  - Decision trees, random forests, gradient boosting  
  - Model comparison using RMSE, R², MAE, ROC/AUC

---

