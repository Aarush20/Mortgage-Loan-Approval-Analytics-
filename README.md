# Mortgage Loan Approval Analytics: Rocket Mortgage (HMDA 2025)

An end-to-end business analytics project that cleans, explores, and models 663,472 real 2025 mortgage applications from Rocket Mortgage to find what drives loan approval decisions. Built as an MBA (Business Analytics) master thesis.

## Data

- **Source:** Home Mortgage Disclosure Act (HMDA) Loan Application Register, 2025, filed by Rocket Mortgage (LEI `549300FGXN1K3HLB1R50`)
- **Download:** [FFIEC HMDA Data Browser](https://ffiec.cfpb.gov/data-browser/) (filter: year 2025, institution Rocket Mortgage, export CSV)
- **Size:** 663,472 records, 99 columns. 539,966 have a clear decision (originated or denied); 514,253 remain after cleaning.
- The raw file is not included in this repo. Download it and save it as `lei_549300FGXN1K3HLB1R50.csv` next to the script.

## What the project does

1. **Clean:** converts numeric fields, turns banded debt-to-income values into numbers, handles outliers and missing values
2. **Explore:** approval rates by loan product, purpose, occupancy, income, DTI, LTV, state, and applicant demographics
3. **Test:** t-tests and chi-square tests, with effect sizes (Cohen's d, Cramér's V)
4. **Model:** Logistic Regression vs Random Forest, with and without demographic variables
5. **Fair-lending check:** compares approval rates between matched groups with similar DTI and LTV

## Key results

| Model | Accuracy | AUC |
|---|---|---|
| Logistic Regression | 80.2% | 0.691 |
| Random Forest (financial variables only) | 85.5% | 0.785 |
| Random Forest (+ race, sex, age) | 85.4% | 0.784 |

- Overall approval rate: **79.5%**
- Top predictors: debt-to-income ratio (39.5% importance), loan-to-value ratio (20.7%), income (11.4%)
- Applications with LTV above 100% were approved only **2.7%** of the time
- Demographic variables add only **3.4%** of model importance, but the approval gap between Black and White applicants narrows only from 8.9 to 7.8 points after matching on DTI and LTV, so a formal fair-lending audit is still warranted

## Run it

```bash
pip install pandas numpy scipy scikit-learn matplotlib seaborn jupyter
jupyter notebook Analysis.ipynb
```

Outputs:
- `charts/`: 21 PNG charts (distributions, approval rates, state maps, correlation heatmap, ROC curve, feature importance, confusion matrix)
- `model_ready_data_powerbi.csv`: cleaned dataset for Power BI

## Repo structure

```
full_analysis.py              # full pipeline: cleaning, tests, models, charts
charts/                       # generated figures
model_ready_data_powerbi.csv  # cleaned data for dashboards
```

## Limitations

- HMDA records the lender's decision, not repayment, so default rate cannot be measured
- Credit scores are not available at full precision in the public file
- One lender, one year, US mortgages only
- The demographic model is exploratory analysis, not a regulatory fair-lending test. Using race, sex, or age to decide real loans is prohibited under ECOA / Regulation B.

## Tech

Python, pandas, NumPy, SciPy, scikit-learn, matplotlib, seaborn. Power BI (dashboard design).
