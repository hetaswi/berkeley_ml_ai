# Project Overview

This repository contains the source code, data processing pipelines, and baseline machine learning evaluations for the Ames Housing dataset. The primary objective is to build a robust real estate valuation engine using regularized linear models (Lasso Regression) on transformed, feature-engineered property data.

---

## Step 1: Loading & Inspection of Dataset

* **Source:** Ames Housing Dataset (`train.csv`).
* **Dimensions:** 1,460 observations and 81 initial features.
* **Feature Breakdown:**
  * **Categorical Features:** 43 attributes (e.g., `Neighborhood`, `HouseStyle`, `SaleCondition`).
  * **Numerical Features:** 37 predictors (e.g., `GrLivArea`, `OverallQual`, `TotalBsmtSF`).
  * **Target Variable:** Continuous `SalePrice`.

---

## Step 2: Data Cleaning & Imputation

Across 1,460 records, 7,829 missing values distributed over 19 features are resolved using domain-specific rules:

* **Structural Absence (MNAR):** Categorical attributes representing missing structures (e.g., `PoolQC`, `Alley`, `Fence`, `FireplaceQu`, `GarageType`, `BsmtQual`) are imputed with `'None'`. Numerical dimensions for non-existent structures (e.g., `MasVnrArea`, `GarageYrBlt`) are imputed with `0`.
* **Conditional Spatial Imputation:** Missing `LotFrontage` values (17.74%) are filled using the median `LotFrontage` calculated within each specific `Neighborhood` group.
* **Categorical Mode:** Low-frequency missing categorical values (e.g., `Electrical` with 0.07% missingness) are filled with the statistical mode (`'SBrkr'`).
* **Outlier Mitigation:** Extreme square footage outliers (`GrLivArea` > 4000 sq ft with `SalePrice` < $300,000) are inspected and filtered.

## Step 3: Exploratory Data Analysis (EDA) & Visualizations

### Target Normalization (log(1+x))
* Raw `SalePrice` shows strong right skewness (+1.88) and high kurtosis (+6.54).
* Applying a log transformation normalizes skewness down to +0.12, stabilizing variance for linear estimation.

### Feature Associations
* `OverallQual` ($r = +0.791$) and `GrLivArea` ($r = +0.709$) show the strongest linear correlation with property value.
* Multicollinearity was identified between paired metrics (e.g., `GarageCars` & `GarageArea` at $r = +0.882$; `TotalBsmtSF` & `1stFlrSF` at $r = +0.820$).

### Spatial Price Stratification
* Sorting median prices by neighborhood reveals significant location-based pricing tiers (e.g., `NridgHt` at >3.5× the median value of `MeadowV`).

---

## Step 4: Feature Engineering

To maximize linear model performance and handle multicollinearity, several combined spatial and temporal metrics were created:

* **TotalSF:** Combines basement (`TotalBsmtSF`) and above-grade living area (`1stFlrSF` + `2ndFlrSF`) into the single strongest physical metric ($r = +0.7823$).
* **TotalBath:** Combines full and half baths across all floors (`FullBath` + 0.5 × `HalfBath`).
* **PropertyAge & RemodelAge:** Calculated as `YrSold - YearBuilt` and `YrSold - YearRemodAdd`.
* **Neighborhood_Group:** Bins 25 neighborhoods into 5 quantile tiers based on median prices, capturing over 70% of the log-target variance.

---

## Step 5: Baseline Model Preparation & Training (LassoCV)

A leak-free Pipeline was built with `scikit-learn` combining standard scaling for continuous predictors, one-hot encoding for categorical variables, and 5-fold cross-validated Lasso Regression (`LassoCV`).

### Model Performance Summary
* **$R^2$ Score (Log / USD Equivalent):** 0.8936 / 0.8905 (Explains $\sim89.36\%$ of price variance)
* **Log RMSE:** 0.1409
* **Mean Absolute Error (MAE):** $18,409.65 USD
* **Optimal Alpha ($\alpha$):** 0.00157

### Key Feature Coefficients (Log Scale)
1. **TotalSF (`+0.1239`):** Primary value driver ($+12.4\%$ valuation gain per standard deviation).
2. **OverallQual (`+0.0981`):** Second most influential driver ($+9.8\%$).
3. **CentralAir_N (`-0.0874`):** Absence of central A/C imposes an automatic $\approx 8.7\%$ price penalty.
4. **Neighborhood_Group (`+0.0830`):** Higher location tier increases valuation by $+8.3\%$.

## Repository Structure
```text
├── CapstoneProjectIdeation.ipynb    # Main notebook
└── README.md                        # Submission documentation
