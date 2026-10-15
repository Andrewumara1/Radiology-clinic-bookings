# Radiology Clinic Bookings Analysis

## 🏥 Project Overview & Business Problem
Radiology clinics run on a mix of high-volume, low-cost scans (X-ray, Ultrasound) and low-volume, high-value scans (CT, MRI). Booking volume alone doesn't show where the money is made, which branches are underperforming, or where patients wait too long for results.

This project analyses **two years of booking data (2024-2025)** from a radiology clinic with **seven branches across Kenya**: Nairobi, Mombasa, Kisumu, Nakuru, Eldoret, Thika and Kisii. It cleans a messy real-world dataset, explores revenue, turnaround time and patient patterns, and builds a model to predict the price of a booking. The results are turned into practical recommendations for clinic management.

**Questions this project answers:**
- Which scan types and branches bring in the most bookings and the most revenue?
- Is revenue growing, and is there any seasonality?
- Which branches and scan types have the slowest turnaround?
- How do cash and insurance patients differ?
- What actually drives the price of a booking?

## 📊 Dataset & Preparation
The original dataset contained **10,145 bookings** with 11 columns. After cleaning, the final dataset contains **6,931 bookings**.

**Cleaning steps:**
- Replaced hidden errors (`UNKNOWN`, `ERROR`, `N/A`, `?`) with missing values
- Converted `scan_date` to a date type, handling mixed formats
- Set impossible ages (below 0 or above 110) and zero prices to missing
- Standardised text in `branch`, `modality` and `gender` (e.g. `xray`, `X ray` and `x-ray` all became `X-ray`)
- Removed rows with missing values, and removed duplicates, including repeated `booking_id`s

**Key Engineered Features:**
- `scan_year`, `scan_month`, `scan_day_of_week`: Date parts from `scan_date`
- `is_weekend`: Boolean flag for Saturday and Sunday bookings
- `age_group`: Child, Teen, Young Adult, Adult, Senior and Elderly
- `season`: Categorised scan months into Winter, Spring, Summer and Fall
- `price_vs_avg`: How far a booking's price is from its modality average

| Column | Description |
|---|---|
| `booking_id`, `patient_id` | Booking and patient references |
| `branch` | Clinic branch (7 branches) |
| `scan_date` | Date of the scan |
| `modality` | X-ray, Ultrasound, CT Scan, MRI or Mammography |
| `age`, `gender` | Patient details |
| `payment_type` | Cash or Insurance |
| `referral_source` | Doctor Referral, Self, Hospital Transfer or Corporate/Insurance Scheme |
| `price_kes` | Price charged, in Kenya Shillings |
| `turnaround_hours` | Hours between the scan and the result |

## 💡 Key Findings
1. **Volume and revenue tell different stories:** X-ray has the most bookings, but **CT Scan brings in the most revenue**. Average revenue per booking is about **KES 21,980 for MRI** and **KES 13,950 for CT**, compared with about KES 2,040 for X-ray.
2. **Nairobi leads, Kisii lags:** Nairobi has the most bookings and revenue, driven mainly by CT and MRI. Kisii is consistently the weakest branch, with roughly 10-20 bookings a month.
3. **Revenue is slipping:** Yearly revenue fell by about **4.9%**, from about KES 25.6 million in 2024 to KES 24.4 million in 2025. Seasonality is weak and demand is spread evenly across the week.
4. **Insurance pays more, cash is more common:** Insurance contributes **53% of revenue** and cash 47%, because insurance patients book more of the expensive CT and MRI scans.
5. **Turnaround depends on the scan, not the payment type:** CT and MRI take about **24 hours**, while X-ray, Ultrasound and Mammography take about **4 hours**. Nakuru has the slowest average turnaround across branches.
6. **Eldoret earns more per booking:** It has a small volume but the highest average price per booking (about KES 7,670).

## 🤖 Price Prediction Model
A **linear regression** model predicts `price_kes` from modality, branch, payment type, referral source, gender, age and weekend, using an 80/20 train/test split.

| Metric | Result |
|---|---|
| R² | 0.89 |
| MAE | KES 1,590 |
| RMSE | KES 2,539 |

**What the model shows:**
- **Modality drives price.** Compared with CT Scan, MRI is about KES 7,900 higher, while X-ray, Ultrasound and Mammography are about KES 12,100, 10,900 and 9,800 lower.
- **Everything else adds very little.** A modality-only model gives almost the same R², so branch, referral source, gender, age and weekend make no meaningful difference to price.
- **Insurance has one small effect:** insurance bookings are about KES 670 cheaper than cash, holding everything else equal.
- Branch revenue differences come from the **mix of scans**, not from pricing.

## Visualization

### 1. Monthly Revenue Trend

![Monthly Revenue Trend](https://github.com/Andrewumara1/Radiology-clinic-bookings/blob/main/images/monthly_revenue_trend.png)

### 2. Bookings per Modality

![Count of Bookings per Modality](https://github.com/Andrewumara1/Radiology-clinic-bookings/blob/main/images/count_of_bookings_per_modality.png)

### 3. Revenue Mix by Branch

![Revenue Mix by Branch](https://github.com/Andrewumara1/Radiology-clinic-bookings/blob/main/images/revenue_mix_by_branch.png)

### 4. Booking Count by Branch and Month

![Booking Count Heatmap](https://github.com/Andrewumara1/Radiology-clinic-bookings/blob/main/images/booking_count_heatmap.png)

### 5. Correlation Matrix

![Correlation Heatmap](https://github.com/Andrewumara1/Radiology-clinic-bookings/blob/main/images/correlation_heatmap.png)

## Strategic Recommendations
Based directly on the findings in the analysis, the following actions are recommended:

* **Protect and grow CT and MRI, especially at Nairobi:** These scans drive revenue. Confirm that machine capacity and radiologist availability can absorb more demand, and promote them to doctor-referral and insurance-scheme sources, which skew toward them.
* **Review the Kisii and Thika branches:** Check whether low volume comes from limited catchment, low awareness or a lack of doctor referrals, and consider referral partnerships with nearby clinics and hospitals.
* **Learn from Eldoret:** Find out why its revenue per booking is high (a richer CT/MRI mix or more insurance patients) and whether other branches can copy the model.
* **Fix turnaround time at Nakuru and for CT/MRI:** Check whether the delay is a staffing or reporting bottleneck.
* **Balance the payment mix:** Watch insurance claim delays and keep strong insurer relationships, while keeping cash-friendly pricing for X-ray and Ultrasound.

## ⚠️ Limitations
- Removing every row with a missing value cut the data by about 32% (10,145 to 6,931 rows). A more targeted approach would keep more bookings.
- The dataset covers only two years, so seasonal patterns are hard to confirm.
- Price is almost entirely set by modality, so the regression model adds little beyond a price list.

## 🛠 Tools & Skills Demonstrated
* **Languages & Libraries:** Python (Pandas, NumPy, Matplotlib, Seaborn, Plotly, Scikit-learn)
* **Skills:** Data Cleaning (hidden errors, mixed formats, duplicates, outliers), Feature Engineering, Exploratory Data Analysis (EDA), Interactive Data Visualization, Linear Regression, and Business Analytics.

## 🚀 How to Run the Project
Clone the repository:

```bash
git clone https://github.com/Andrewumara1/radiology-clinic-bookings-analysis.git
cd radiology-clinic-bookings-analysis
```

Install dependencies:

```bash
pip install pandas numpy matplotlib seaborn plotly scikit-learn jupyter
```

Place the dataset at `data/radiology_clinic_bookings.csv`, then open the notebook:

```bash
jupyter notebook project.ipynb
```

## 👤 Author
**Andrew Umara**
GitHub: [Andrewumara1](https://github.com/Andrewumara1)
