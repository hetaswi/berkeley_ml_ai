# Bank Telemarketing Direct Campaign Analysis & Classifier Comparison

A comprehensive data science project comparing four machine learning classifiers (**Logistic Regression**, **K-Nearest Neighbors**, **Decision Trees**, and **Support Vector Machines**) on Portuguese bank direct marketing telemarketing data.

---

## Executive Summary

This report evaluates four machine learning classifiers—**Logistic Regression**, **K-Nearest Neighbors (KNN)**, **Decision Trees**, and **Support Vector Machines (SVM)**—to predict whether bank clients will subscribe to a long-term deposit product following phone marketing outreach.

The primary business objective is to transition from random or exhaustive telemarketing calls to a data-driven, targeted outreach strategy. By identifying high-yield prospective clients, the bank can optimize call center efficiency, cut operational expenses, and minimize customer fatigue.

---

## Dataset Overview & Key Characteristics

* **Data Source:** UCI Machine Learning Repository dataset originating from a Portuguese retail banking institution.
* **Timeframe & Volume:** Encompasses **17 distinct direct telemarketing campaign waves** executed between **May 2008 and November 2010**, totaling **41,188 client records** (`bank-additional-full.csv`).
* **Target Variable (`y`):** Binary outcome indicating deposit subscription (`'yes'` = 1, `'no'` = 0).
* **Severe Class Imbalance:** Only **11.27%** of client contacts resulted in a deposit subscription, while **88.73%** declined.

---

## Business Objective & The "Accuracy Trap"

### The Baseline
A naive **Majority Class Classifier** that blindly predicts `"no"` for every customer achieves a baseline accuracy of **88.73%**.

### Why Accuracy is Misleading
When evaluating default machine learning models, **Accuracy creates a false sense of success**:
* Under default settings, models maximize overall accuracy by predicting `"no"` almost exclusively.
* **K-Nearest Neighbors (KNN)** achieved **88.04% Accuracy** under default settings, but its **Recall was a near-zero 1.29%**—failing to identify actual deposit subscribers.

### Strategic Metric Shift
To solve the business problem, model evaluation was re-oriented toward **ROC-AUC** and **Recall**. In direct marketing, the revenue from securing a term deposit far outweighs the marginal cost of a phone call.

---

## Classifier Performance Summary

Using hyperparameter tuning (`GridSearchCV`) with 5-fold cross-validation and **class weight balancing** (`class_weight='balanced'`), model performance was significantly improved for positive class identification:

| Classifier | Key Settings / Best Hyperparameters | Training Time | Test Accuracy | Test Recall (Class 1) | Test ROC-AUC |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Logistic Regression** | `C=0.1`, `class_weight='balanced'` | ~0.82 s | 62.89% | **61.21%** | **0.6508** |
| **Decision Tree** | `max_depth=5`, `class_weight='balanced'` | ~0.45 s | 63.80% | **59.81%** | **0.6482** |
| **Support Vector Machine** | `C=1.0`, `class_weight='balanced'` | ~68.30 s | 62.88% | 60.78% | 0.6421 |
| **K-Nearest Neighbors** | `n_neighbors=35`, `weights='distance'` | ~2.14 s | 88.04% | 1.29% | 0.5841 |

### Key Takeaways from Model Training
1. **Class Weighting Reclaims Predictive Power:** Incorporating balanced class weights shifted model decision thresholds, jumping Recall from **0% to over 60%** for Logistic Regression, Decision Trees, and SVMs.
2. **Logistic Regression is the Top Performer:** Delivers the highest overall **ROC-AUC (0.6508)** and **Recall (61.21%)** while fitting in less than one second.
3. **Decision Trees Offer Transparency:** Performs nearly on par with Logistic Regression while providing intuitive, rule-based logic easily interpretable by non-technical marketing teams.
4. **SVM Creates a Computational Bottleneck:** Requires significantly longer training time (~68 seconds) without delivering meaningful performance gains over Logistic Regression.

---

## Demographic & Statistical Insights

* **Target Profiles:**
  * **Retirees** and **Students** demonstrate the highest relative subscription conversion rates among job categories.
  * Clients with **no credit default record** (`default = 'no'`) account for virtually all successful conversions.
* **Statistical Limits of Demographics:** Demographic features alone (`age`, `job`, `marital`, `education`, `default`, `housing`, `loan`) set a performance ceiling near **0.65 ROC-AUC**.

---

## Strategic Business Recommendations & Next Steps

1. **Deploy Logistic Regression in Production:** Implement Logistic Regression or a tuned Decision Tree for call list generation. Their fast execution time and clear feature weighting make them ideal for operational deployment.
2. **Adopt Tiered Probability Outreach:** Instead of a strict binary threshold, rank clients by predicted subscription probability:
   * **Tier 1 (High Probability > 70%):** Direct phone outreach assigned to senior sales staff.
   * **Tier 2 (Medium Probability 30%–70%):** Secondary outreach via digital marketing or email campaigns.
   * **Tier 3 (Low Probability < 30%):** Suppress from direct phone calling to protect brand sentiment and eliminate unnecessary call center spending.
3. **Expand Feature Inputs to Boost Predictive Capacity:** Incorporate campaign contact history (`poutcome`, `pdays`, `previous`) and macroeconomic indicators (`euribor3m`, `emp.var.rate`, `cons.price.idx`) into future model iterations. Prior research indicates including these external signals elevates model ROC-AUC from **~0.65 to > 0.90**.

---

## Repository Structure

```text
.
├── Required_Assignment_17_1.ipynb       # Primary Jupyter Notebook containing data preprocessing, EDA, model training, and evaluation
├── bank-additional-full.csv             # Full benchmark dataset (41,188 rows x 21 columns)
├── bank-additional.csv                  # 10% sample dataset for fast algorithm prototyping
├── bank-additional-names.txt            # Data dictionary and UCI attribute descriptions
├── bank_marketing_classifier_plots.png  # Generated evaluation visual (Model metrics & Class imbalance pie chart)
└── README.md
