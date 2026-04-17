# Predicting Hotel Booking Cancellations

> A machine learning project by: — Chip · Dajana · Kerry-Ann · Mary · Shivana · Tenchi

---

## Overview

Hotel booking cancellations cost the hospitality industry billions each year. Project Reserve builds a classification model that predicts whether a hotel booking will be cancelled — before it happens — so hotels can take proactive action to protect revenue and improve guest experience.

**The result:** An XGBoost model with **83% accuracy** and an estimated **~€41,000 in additional monthly profit** per hotel through smarter inventory and retention decisions.

---

## Problem Statement

Predict booking cancellations based on customer characteristics and booking details to help hotels:

- Make smarter **operational and pricing decisions**
- Reduce **unoccupied rooms** and recover lost revenue
- Improve **guest experience** through targeted outreach

**Target variable:** `booking_status` — Cancelled (1) or Not Cancelled (0)

---

## Dataset

| Attribute | Detail |
|---|---|
| Source | [Kaggle — Hotel Booking Demand](https://www.kaggle.com/datasets/jessemostipak/hotel-booking-demand) |
| Hotels | 2 hotels in Portugal (City Hotel & Resort Hotel) |
| Period | July 2015 – August 2017 |
| Raw rows | 119,390 |
| Final rows | 85,012 |
| Features | 20 |

**Feature categories:**

- **Booking Attributes** — lead time, arrival date, average daily rate, length of stay, reservation status, days on waitlist, deposit type
- **Booking Behavior** — previous cancellations, repeated guest status, booking changes
- **Customer Attributes** — number of adults/children, customer type, market segment
- **Hotel/Room Details** — hotel type, meal preference, reserved room, parking spaces, special requests

---

## Data Preparation

Starting from 119,390 raw rows, the cleaning pipeline produced 85,012 final records — **97.27% of true unique records**.

| Step | Action | Impact |
|---|---|---|
| Remove duplicates | Kept real values only | −31,994 rows → 87,396 |
| Handle missing values | Mode imputation for `children` (4 rows) and `country` (488 rows) | −0 rows |
| Drop sparse features | Removed `agent` (12,193 missing) and `company` (82,137 missing) | Cleaner feature space |
| Handle outliers | Transformed extreme values, binning, removed impossible values | −2,384 rows |
| Multicollinearity | Dropped features causing high correlation | Reduced redundancy |

**Additional considerations:**
- No PII — customer identification data was not included in the dataset
- Dates that were not the latest version were managed to retain real values

---

## Exploratory Data Analysis

Key findings from the data:

- **~1 in 4 bookings** is cancelled overall (27.8% cancellation rate)
- **City hotels cancel more** than resort hotels (30% vs. 24%)
- **Peak summer months** (June–August) drive the most bookings *and* the most cancellations
- Cancellation risk is highly concentrated among specific customer segments

---

## Model Development

Three classification models were trained and compared:

| Model | Accuracy | Precision | Recall | AUC |
|---|---|---|---|---|
| Logistic Regression | 79.7% | 69% | 50% | 0.846 |
| Random Forest | 81.9% | 71% | 59% | 0.877 |
| **XGBoost (Tuned)** ✅ | **83.0%** | **73%** | **62%** | **0.893** |

**Why XGBoost?**
- Best performance across all metrics
- Strongest early targeting in cumulative lift chart — identifies high-risk cases far better than alternatives
- No overfitting — results hold true for both historical and new data

---

## Results

The final tuned XGBoost model achieves:

| Metric | Value |
|---|---|
| Accuracy | 83% |
| Precision | 73% |
| Recall (Cancellation Capture) | 62% |
| AUC | 0.893 |
| True Positives (Caught Cancels) | 2,927 |
| False Negatives (Missed Cancels) | 1,801 |
| False Positives (False Alarms) | 1,061 |
| True Negatives (Correct Stays) | 11,214 |
| Estimated Profit from Predictions | $874,991 |
| **Additional Monthly Profit** | **~€41,000** |

*Profit estimate assumes: ADR = €110, avg. stay = 4 days, resell rate = 70%*

---

## Key Drivers

### 🚨 Cancellation Drivers

| Driver | Impact |
|---|---|
| **Longer lead time** | Bookings made 3+ months in advance are **15× more likely** to be cancelled |
| **Previous cancellation** | A guest who cancelled before is **21× more likely** to cancel again |
| **Online Travel Agency** | OTA bookings are **2.3× more likely** to cancel |
| **Higher average daily rate** | Premium rates **double** cancellation risk; the highest-priced rooms **triple** it |

### ✅ Retention Drivers

| Driver | Impact |
|---|---|
| **International guest** | 80% less likely to cancel than domestic guests |
| **Needs parking** | 99% less likely to cancel — virtually guaranteed to show up |
| **Special requests** | 47% less likely to cancel |
| **Repeat guest** | Significantly lower cancellation likelihood |

---

## Business Applications

### Customer Profiling

Segment guests by predicted cancellation risk:

- **High-Risk** — past cancellations, OTA bookings, long lead time + high rates
- **Moderate-Risk** — mixed indicators
- **Low-Risk** — parking needs, repeat stays, international travelers

### Strategies

| Strategy | Actions |
|---|---|
| **Proactive Retention** | Automated reminders & reconfirmations; limit date change flexibility |
| **Targeted Offers** | Risk-based cancellation windows; price incentives by channel (e.g., OTA surcharges) |
| **Inventory & Revenue Management** | Intelligent overbooking for high-risk segments; early room release when cancellation is likely |
| **Loyalty & Customer Experience** | Loyalty invitations for low-risk guests; personalized offers based on past behavior |
| **Platform / Dashboard Integration** | Auto-tagging of guest risk profiles; daily/weekly/monthly cancellation prediction dashboard |
| **Guest Engagement** | Targeted messaging for high-risk guests; flexible check-in or small add-ons to reduce likelihood |

---

## Project Structure

```
project-reserve/
│
├── data/
│   └── README.md               # Dataset source and download instructions
│
├── notebooks/
│   ├── 01_data_preparation.ipynb
│   ├── 02_eda.ipynb
│   └── 03_model_selection.ipynb
│
├── presentations/
│   ├── 1-ProjectReserve_DataPreparation.pptx
│   ├── 2-InsightOut_DataPreparationReport.pptx
│   ├── 3-Model_Selection.pptx
│   └── 4-InsightOut_FinalPresentation.pptx
│
├── requirements.txt
└── README.md
```

> **Note:** The raw dataset is not included in this repo. Download it from [Kaggle](https://www.kaggle.com/datasets/jessemostipak/hotel-booking-demand) and place it in the `data/` folder.

---

## Tech Stack

- **Python** — pandas, numpy, scikit-learn, xgboost, matplotlib, seaborn
- **Models** — Logistic Regression, Random Forest, XGBoost
- **Evaluation** — Confusion matrix, ROC-AUC, cumulative lift chart, profit estimation

---

## Team

**InsightOut** — a data science consulting group.

Chip · Dajana · Kerry-Ann · Mary · Shivana · Tenchi
