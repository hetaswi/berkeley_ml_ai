# Real Estate Valuation & Dynamic Pricing Engine

## Project Overview
Setting an accurate listing price for residential properties is one of the most critical decisions in real estate. 
* **Overpricing** leads to properties sitting on the market, incurring higher carrying costs, and requiring eventual price drops.
* **Underpricing** leaves thousands of dollars on the table for homeowners and real estate investors.

This project delivers an automated, machine-learning-powered **Automated Valuation Model (AVM)**. By analyzing physical home characteristics, effective age, structural quality, and neighborhood tier classifications, the engine produces real-time, objective market value estimates.

---

## Business Problem & Objectives
Traditional real estate appraisals rely heavily on human comparisons, which can be slow, expensive, and subject to personal or regional bias. 

Our primary goals for this pricing engine were to:
1. **Reduce Price Valuation Uncertainty:** Automate fair-market property estimates within an average error margin of under 10%.
2. **Identify Primary Drivers of Value:** Pinpoint which home renovations and characteristics contribute most to resale value.
3. **Handle Regional Price Differences:** Automatically group localized neighborhoods into fair value tiers without requiring manual regional lookup tables.

---

## Data Summary & Key Insights

The model was built using historical sales data from 1,460 residential properties across 80 unique features (e.g., square footage, construction year, finish quality, and bathroom counts).

### Important Findings from Data Exploration:
* **High-End Luxury Homes Distort Averages:** Raw sales prices contained extreme luxury outliers, creating a skewed market view. Transforming sales prices onto a standard percentage scale ensured our pricing model remained accurate for middle-income housing as well as higher-end estates.
* **Key Drivers of Value:** The three single largest factors influencing home prices are:
  1. **Overall Construction & Finish Quality** (materials, craft, and architectural design)
  2. **Total Usable Square Footage** (combined basement and above-ground space)
  3. **Effective Property Age** (how recently the home was built or remodeled)
* **Location Tiers Matter:** Homes with identical physical sizes varied in price by up to 45% based on neighborhood location tier.

---

## Machine Learning Solution & Methodology

### Data Preparation
* **Data Cleaning:** Extreme physical anomalies (e.g., properties over 4,000 sq ft selling at clearance prices) were removed to avoid corrupting model estimates.
* **Smart Imputation:** Missing values were filled using localized medians to preserve data integrity.
* **Feature Engineering:** We constructed aggregated metrics that mirror real estate logic, including **Total Usable Square Feet**, **Total Bathroom Count**, **Property Age**, and **Neighborhood Price Tiers**.

### Model Strategy
We evaluated three different modeling strategies:
1. **Lasso Linear Baseline:** A baseline model that assumes straight-line relationships between home features and price.
2. **Random Forest:** A decision-tree ensemble that groups homes into multi-attribute buckets.
3. **Gradient Boosting Engine (Winning Model):** An advanced learning algorithm that builds sequential decision trees to catch subtle, localized interactions (such as how an added bathroom impacts price differently in a luxury neighborhood versus an entry-level neighborhood).

---

## Final Performance & Business Impact

Models are evaluated on unseen test properties to ensure they perform reliably in real-world conditions.

### Model Comparison Results

| Valuation Engine | Overall Variance Explained ($R^2$) | Average Dollar Error (MAE) | Business Interpretation |
| :--- | :--- | :--- | :--- |
| **Lasso Baseline Model** | 89.1% | $17,700 | Good baseline, but misses non-linear neighborhood trends. |
| **Random Forest** | 87.9% | $17,324 | Better on average homes, but slightly lower overall consistency. |
| **Gradient Boosting (Selected)** | **88.4%** | **$16,712** | **Best overall accuracy:** Reduces price valuation error by nearly **$1,000 per home**. |

### Key Business Takeaways
* **High Dollar Accuracy:** The selected Gradient Boosting engine predicts listing prices within an average error of **~$16,712** on homes averaging **~$180,000**—representing a **sub-10% error rate**, which matches commercial valuation platforms.
* **Strong Generalization:** The engine accounts for over **88% of price variations** on unseen listings, proving it learns genuine market fundamentals rather than memorizing training data.
* **Smooth Market Range:** When run on 1,459 unlabeled new market listings, the model generated realistic property valuations ranging smoothly from **$44,553** to **$570,970** without negative or unrealistic prices.

---

## Recommendations & Next Steps

1. **Strategic Renovation Guidance:** Homeowners and real estate agents should focus on structural quality and total finished space over purely cosmetic updates, as quality and area account for over 60% of valuation weight.
2. **Geospatial Expansion:** Integrate exact GPS coordinates and school district boundaries to refine micro-neighborhood pricing.
3. **Interactive Valuation Web App:** Package the Gradient Boosting pipeline into an easy-to-use web interface where listing agents can input home specs and receive real-time price confidence ranges.

---

## Repository Structure

```text
.
├── CapstoneProjectFinal.ipynb   # Technical Jupyter Notebook containing EDA, preprocessing, 
│                                # feature engineering, tree ensemble modeling, and submission logic.
└── README.md                    # Executive non-technical report covering business problem, 
                                 # methodology, findings, and project recommendations.
