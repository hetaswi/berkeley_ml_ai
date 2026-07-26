# Used Car Inventory Optimization Analysis

## Overview
This repository presents an analysis of a Kaggle used car dataset containing information on over 400,000 vehicle listings. Using the industry-standard **CRISP-DM** (Cross-Industry Standard Process for Data Mining) framework, this project identifies the primary factors driving used car pricing to help dealerships fine-tune inventory acquisition and trade-in pricing strategies.

---

## Executive Summary & Key Takeaways

Consumers pay premium prices primarily for **utility, capability, and vehicle condition integrity**. To maximize profit margins and inventory turnover, dealerships should prioritize acquiring vehicles with **4-Wheel Drive (4WD)**, **truck/pickup body styles**, and **diesel engines**, while enforcing a strict **clean-title-only** sourcing policy.

### Core Insights for Inventory Sourcing

1. **Utility Body Styles Command Premium Value**
   * **Pickups and Trucks** command a median price premium of **\$6,900 to \$7,200** over standard sedans.
   * Sedans (median price ~$11,500) and mini-vans (median price ~$7,500) sit at the lowest valuation tiers and experience slower inventory turnover.

2. **Drivetrain & Fuel Type Matter Significantly**
   * **4-Wheel Drive (4WD)** vehicles achieve a median price of **\$21,100**, compared to **\$10,750** for Front-Wheel Drive (FWD) models.
   * **Diesel engines** average a median price of **\$32,999**, vastly outperforming gasoline (\$13,995) and hybrid (\$12,998) options due to commercial towing and durability demand.

3. **Strategic Acquisition Sweet Spot: 3 to 6 Years Old (< 80,000 Miles)**
   * Depreciation drops most steeply during years 1 through 3 (8%–12% per year). 
   * Acquiring 3-to-6-year-old vehicles bypasses initial steep depreciation while avoiding high reconditioning costs associated with 100,000+ mile cars.

4. **Title Integrity is Non-Negotiable**
   * Vehicles with a **clean title** hold a **\$5,500 to $8,500** premium over salvage, rebuilt, or missing titles.
   * Non-clean titles face heavy markdowns (>40% penalty) and higher legal/warranty liability.

---

## Summary of Analytical Modeling

Using regularized linear regression algorithms (Lasso & Ridge) trained on log-transformed vehicle prices, our predictive models explained **~66.8% of the total variance** ($R^2 = 0.6675$) in resale pricing:

| Model Architecture | Parameter Tuning | Test $R^2$ Score | Test MAE (\$) |
| :--- | :---: | :---: | :---: |
| **Linear Regression (OLS)** | None | 0.6660 | \$5,207 |
| **Ridge Regression** | $\alpha = 0.1$ | 0.6672 | \$5,192 |
| **Lasso Regression** | $\alpha = 0.0001$ | 0.6675 | \$5,188 |

*Note: Regularization ($L_1$ / $L_2$) was required to resolve cross-validation instability caused by multicollinearity among categorical attributes.*

---

## Dealership Action Plan

| Strategy Area | Recommended Action | Operational Impact |
| :--- | :--- | :--- |
| **1. Auction Bidding** | Shift inventory allocation toward **Pickups, Trucks, and 4WD SUVs**. | Higher gross margin per vehicle unit. |
| **2. Powertrain Focus** | Prioritize **Diesel powertrains** when sourcing light work trucks. | Stronger buyer demand and higher resale values. |
| **3. Trade-In Pricing** | Apply stricter depreciation discounts for FWD sedans and high-mileage cars (>120k miles). | Protects dealership margin on trade-in resale. |
| **4. Risk Management** | Enforce a strict **Clean-Title-Only** inventory sourcing policy. | Reduces days-on-lot and protects brand reputation. |

---

## Key Visualizations

The dataset analysis includes four core visual outputs saved in `used_car_plots.png`:
* **Vehicle Depreciation Curve:** Maps median listing price against vehicle age to illustrate non-linear value loss.
* **Price Comparison by Fuel & Drivetrain:** Demonstrates 4WD and Diesel pricing premiums.
* **Body Type Resale Ranking:** Ranks vehicle body styles from highest to lowest median price.
* **Actual vs. Predicted Prices:** Illustrates regression model fit and residual accuracy.

---

## Repository Structure

```text
├── Required_Assignment_11_1.ipynb   # Jupyter Notebook containing full CRISP-DM code and report
├── vehicles.csv.gz                  # Raw Kaggle used car dataset (~426K rows). Unzip this prior to running the code
├── used_car_plots.png               # Summary visualizations of EDA and model performance
└── README.md                        # Executive report and project overview
