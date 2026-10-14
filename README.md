# Radiology Clinic Bookings Analysis

A data analytics project exploring booking records from a network of radiology clinics in Kenya. The analysis cleans a messy bookings dataset, uncovers patterns in demand, pricing and turnaround time, and builds regression models to predict price and turnaround hours.

## Project Goals

- Clean and validate raw booking data (missing values, duplicates, inconsistent dates)
- Understand booking demand across branches, scan types and time
- Analyse pricing (KES) and turnaround times
- Predict `price_kes` and `turnaround_hours` with linear regression
- Turn the findings into practical business recommendations

## Repository Structure

```
Radiology-clinic-bookings/
├── data/
│   └── radiology_clinic_bookings.csv   # Raw bookings dataset
├── project.ipynb                       # Main analysis notebook
└── README.md
```

## Dataset

The dataset (`data/radiology_clinic_bookings.csv`) contains one row per clinic booking.

| Column | Description |
|---|---|
| `scan_date` | Date of the scan |
| `price_kes` | Price charged, in Kenyan shillings |
| `turnaround_hours` | Hours taken to deliver the scan results |

> Add the remaining columns here (for example branch, scan type, patient age group, booking status) to match your CSV.

## Methodology

1. **Data cleaning**
   - Remove duplicate bookings
   - Handle missing values column by column instead of dropping whole rows, to avoid losing large amounts of data
   - Parse `scan_date` with `pd.to_datetime(..., errors="coerce")` and handle invalid dates
2. **Exploratory data analysis**
   - Bookings by branch, scan type and period
   - Price and turnaround distributions
   - Interactive Plotly charts for exploring trends
3. **Modelling**
   - Linear regression predicting `price_kes` and `turnaround_hours`
   - Train/test split, with evaluation metrics (R², MAE, RMSE)
   - Actual vs predicted plots and a coefficients table
4. **Recommendations**
   - Actionable suggestions for the clinics based on the findings

## Key Findings

> Fill these in after your final run, for example:
> - Which branches and scan types drive the most bookings
> - Where turnaround times are slowest
> - Which factors influence price the most

## Tech Stack

- Python 3
- pandas, NumPy
- Matplotlib, Seaborn
- Plotly
- scikit-learn
- Jupyter Notebook

## Getting Started

```bash
# Clone the repository
git clone https://github.com/Andrewumara1/Radiology-clinic-bookings.git
cd Radiology-clinic-bookings

# Install dependencies
pip install pandas numpy matplotlib seaborn plotly scikit-learn jupyter

# Launch the notebook
jupyter notebook project.ipynb
```

## Future Improvements

- Interactive Streamlit dashboard with branch and date filters
- Try non-linear models (Random Forest, Gradient Boosting) and compare with linear regression
- Demand forecasting by branch

## Author

**Andrew Umara**
Organisation: KeDatalab
