# In-Vehicle Coupon Recommendation Analysis

![Python](https://img.shields.io/badge/Python-3.9+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-2.0+-150458?style=for-the-badge&logo=pandas&logoColor=white)
![Seaborn](https://img.shields.io/badge/Seaborn-0.13+-3776AB?style=for-the-badge)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?style=for-the-badge&logo=jupyter&logoColor=white)

---

## 📌 Project Overview

This project analyzes data from the **UCI Machine Learning Repository** (collected via Amazon Mechanical Turk) to answer a fundamental question in contextual marketing: **“Will a customer accept a driving coupon?”**

Using exploratory data analysis (EDA), statistical aggregation, and data visualizations, this project explores how user attributes (e.g., age, income, venue visit habits) and contextual factors (e.g., time of day, passenger type, destination urgency) influence coupon acceptance across five coupon types:
1. **Carry out & Take away**
2. **Restaurant (<$20)**        

3. **Coffee House**
4. **Restaurant ($20–$50)**
5. **Bar**

---

## 🔗 Project Deliverables & Notebook Link

* 📓 **Jupyter Notebook:** [View the Practical Application 1 Notebook](./Practical_Application_1.ipynb)
* 📁 **Dataset:** [coupons.csv](./coupons.csv)

---

## 📊 Dataset & Cleaning Methodology

The original dataset contains **12,684 records** and **26 features**. Prior to analysis, data cleaning was conducted:

1. **Sparsity & Column Removal:**
   * **`car`:** Dropped due to **99.15% missing values** (12,576 missing entries).
   * **`toCoupon_GEQ5min`:** Dropped due to **zero variance** (all entries equal `1`).
   * **`direction_opp`:** Dropped to eliminate perfect multicollinearity with `direction_same`.
2. **Duplicate Removal:** Removed **74 duplicate rows** (0.58% of the dataset).
3. **Typo Correction:** Renamed `passanger` to `passenger`.
4. **Imputation:** Missing values in categorical venue frequency columns (`Bar`, `CoffeeHouse`, `CarryAway`, `RestaurantLessThan20`, `Restaurant20To50`) were imputed using column modes to maintain modern Pandas 3.0+ compliance without chained assignment warnings.

---

## 🔑 Key Findings & Summary of Observations

### 1. Overall Acceptance Baseline
* **Overall Coupon Acceptance:** **56.84%** across all driving scenarios ($7,210$ accepted / $12,684$ total).
* **Acceptance by Coupon Type:**
  * **Carry out & Take away:** **73.55%**

  * **Restaurant (<$20):** **70.71%**            

  * **Coffee House:** **49.92%**
  * **Restaurant ($20–$50):** **44.10%**
  * **Bar:** **41.00%**

---

### 2. Bar Coupon Deep-Dive
While Bar coupons have a lower baseline acceptance rate (**41.00%**), targeted segmentation reveals dramatic performance shifts:

* **Prior Habit is Paramount:** Drivers who visit a bar **more than once a month** accept bar coupons at **68.79%**, compared to **29.35%** for rare or non-visitors. Drivers going $>3$ times/month reach **76.88%** acceptance.
* **Age & Social Context:** Drivers **over age 25** who visit bars $>1$/month accept at **69.52%** (vs. **33.50%** for all others).
* **Passenger Filtering:** Frequent bar-goers without child passengers and not working in agriculture accept bar coupons at **71.32%** (vs. **29.60%** for all others).
* **Young Adults:** Drivers **under age 30** who visit bars $>1$/month achieve the highest bar segment acceptance rate at **72.17%**.

---

### 3. Coffee House Coupon Independent Investigation
An independent investigation of **Coffee House** coupons ($N = 3,996$, baseline **49.92%**) identified clear temporal and social patterns:

* **Habit Match:** Frequent coffee goers ($>1$ visit/month) accept at **66.02%**, versus **18.88%** for drivers who "never" visit coffee houses.
* **Optimal Time Window:** **10 AM** is the peak conversion time (**64.07%** acceptance), followed by **2 PM** (**54.79%**). Early morning commuters (**7 AM**) accept at lower rates (**44.58%**) due to schedule urgency.
* **Social Companionship:** Drivers accompanied by **friends** (**59.69%**) or a **partner** (**57.05%**) convert significantly higher than solo drivers (**43.79%**).
* **Time Sensitivity:** Coupons with a **1-day expiration** accept at **58.39%**, outperforming **2-hour expiration** coupons (**43.20%**).
* **High-Converting Composite Segment:** Frequent coffee goers offered coupons during **10 AM / 2 PM** or while accompanied by **friends/partners** convert at **76.66% – 77.44%**.

---

## 📈 Strategic Business Recommendations

1. **Prioritize Behavioral Alignment:**  
   * Suppress **Bar** and **Coffee House** coupons for users reporting "never" visiting those venues. Reallocate promo spend toward high-converting general categories (**Carry out** and **Cheap Restaurants**).
2. **Context-Aware Dynamic Pushing:**  
   * Push **Coffee House** coupons mid-morning (**10 AM**) or early afternoon (**2 PM**), especially when the driver’s destination is unhurried ("No Urgent Place") and social passengers are present.
3. **Strict Demographic & Passenger Rules for Alcohol/Nightlife Offers:**  
   * Target **Bar** coupons exclusively to users with a documented history of bar visits ($>1$/month), aged under 30 or over 25 with adult companions, and **never** when child passengers are detected in the vehicle.

---

## 🛠️ Repository Structure

```text
├── README.md                     <- Executive summary and portfolio documentation
├── Practical_Application_1.ipynb <- Main Jupyter Notebook containing analysis & charts
├── coupons.csv                   <- UCI dataset
├── coupon_distribution.png       <- Distribution chart of coupon offers
├── temperature_histogram.png     <- Histogram of driving scenario temperatures
└── coffee_house_investigation.png<- 4-panel dashboard of Coffee House insights
