🛍️ CLV Prediction — E-Commerce Customer Lifetime Value Engine

Can a regression model estimate which customers are worth retaining before the marketing budget is spent?

## Problem Statement

E-commerce teams often treat every customer equally: the same discounts, the same retargeting spend, and the same retention campaigns. But customer value is highly concentrated. A small group of repeat buyers can drive a large share of revenue, while inactive or low-value customers may not justify expensive outreach.

This project predicts Customer Lifetime Value (CLV) from transaction history using RFM features: Recency, Frequency, and Monetary value. The goal is not just to fit a regression model, but to turn customer behaviour into an actionable retention playbook.

## Dataset

Source: UCI Online Retail II, also available on Kaggle  
Size: Online retail transactions from December 2009 to December 2011  
Target: CLV, defined as each customer's historical monetary value  
File expected: `online_retail_II.csv`

Note: The dataset is large and may not always be committed to GitHub. Download `online_retail_II.csv` from Kaggle or UCI and place it in the project root before running the script.

## Approach

### Act 1 — Revenue Landscape

- Load and validate the raw transaction dataset
- Remove cancelled invoices, anonymous customers, non-positive quantities, and invalid prices
- Compute line-item revenue as `Quantity * Price`
- Visualise monthly revenue, international revenue mix, and revenue concentration

### Act 2 — Customer DNA

- Engineer RFM features per customer:
  - Recency: days since last purchase
  - Frequency: number of distinct invoices
  - Monetary: total historical spend
- Create quintile-based RFM scores
- Segment customers into four business archetypes:
  - Champion
  - Loyal
  - At Risk
  - Lost

### Act 3 — CLV Prediction

- Use historical monetary value as the CLV target
- Log-transform skewed variables so the model handles high-value customers without ignoring the long tail
- Compare interpretable regression models with 5-fold cross-validation:
  - Linear Regression
  - Ridge Regression
  - Lasso Regression
  - ElasticNet
- Select the best model by cross-validated RMSE
- Back-transform predictions into GBP for business interpretation

### Act 4 — Marketing Verdict

- Project predicted CLV by customer segment
- Compare actual vs predicted CLV across segments
- Translate model output into campaign strategy:
  - Champions: VIP rewards, early access, referral campaigns
  - Loyal: cross-sell and upsell campaigns
  - At Risk: personalised win-back offers
  - Lost: low-cost automated email or suppression from paid retargeting

## Key Results

The script prints model results after running `clv_customer_lifetime_value.py`.

| Model | CV RMSE | CV MAE | CV R2 |
|---|---:|---:|---:|
| Linear Regression | 0.0000 | 0.0000 | 1.0000 |
| Ridge (alpha=1.0) | 0.0017 | 0.0011 | 1.0000 |
| Lasso (alpha=0.01) | 0.0100 | 0.0077 | 0.9999 |
| ElasticNet (alpha=0.01) | 0.0153 | 0.0089 | 0.9998 |

Business impact: The final segment projection table identifies where retention spend should be concentrated by comparing predicted CLV and revenue share across Champion, Loyal, At Risk, and Lost customers.

## Outputs

When the script runs successfully, it saves these visualisations to `outputs/`:

- `act1_revenue_landscape.png`
- `act2_customer_dna.png`
- `act3_clv_model_showdown.png`
- `segment_clv_projection.png`

## How to Run

```bash
git clone https://github.com/PrajwalKapnoor/clv-predection
cd clv-predection
pip install pandas numpy matplotlib seaborn scikit-learn
```

Download `online_retail_II.csv` from one of these sources:

- Kaggle: https://www.kaggle.com/datasets/mashlyn/online-retail-ii-uci
- UCI: https://archive.ics.uci.edu/dataset/502/online+retail+ii

Place the CSV in the project root, then run:

```bash
python clv_customer_lifetime_value.py
```

## Tech Stack

Python  
Pandas  
NumPy  
Scikit-learn  
Matplotlib  
Seaborn

## LinkedIn Posts

Completion post —[ CLV prediction results and marketing playbook](https://www.linkedin.com/posts/prajwal-kapnoor-50042730a_datascience-machinelearning-customerlifetimevalue-activity-7456896639824457728-dos1)
