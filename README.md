# Healthcare-claims-fraud-detection-analysis

## Overview
This project identifies healthcare providers with anomalous claims patterns that may indicate potential fraud, using SQL for data aggregation and Python for statistical and machine learning-based anomaly detection. The analysis is built on the Kaggle "Healthcare Provider Fraud Detection Analysis" dataset, which includes inpatient, outpatient, and beneficiary claims data along with ground-truth fraud labels for validation.

The goal was to simulate a real-world compliance analytics workflow: extract and aggregate claims data, flag statistically unusual providers, and validate those flags against known outcomes — the same core process used in healthcare and insurance fraud/compliance monitoring.

## Tech Stack
- **SQL (SQLite)** — data loading and provider-level aggregation
- **Python** — pandas, scipy, scikit-learn, matplotlib
- **Methods** — z-score outlier detection, Isolation Forest (unsupervised anomaly detection)

## Dataset
[Healthcare Provider Fraud Detection Analysis](https://www.kaggle.com/datasets/rohitrox/healthcare-provider-fraud-detection-analysis) (Kaggle)
- `Train_Inpatientdata.csv`, `Train_Outpatientdata.csv` — claims data
- `Train_Beneficiarydata.csv` — patient demographic/chronic condition data
- `Train.csv` — provider-level fraud labels (`PotentialFraud`: Yes/No)

## Methodology
1. **Load data into SQLite** — all claims tables loaded into a local relational database.
2. **Aggregate claims by provider (SQL)** — computed total claims, average reimbursement, claim volume, and distinct beneficiary count per provider.
3. **Flag anomalies (Python)**:
   - **Z-score method**: flagged providers whose total reimbursement, average reimbursement, or claim volume exceeded 2 standard deviations from the mean.
   - **Isolation Forest**: an unsupervised machine learning model trained on the same aggregated features, flagging the top ~5% most anomalous providers.
4. **Validate against ground truth** — compared flagged providers against the dataset's actual `PotentialFraud` labels to measure how well each method performed.
5. **Visualize results** — compared fraud rates between flagged and non-flagged providers.

## Results
| Method | Overall Fraud Rate | Fraud Rate Among Flagged | Lift |
|---|---|---|---|
| Z-score | 9.35% | 46.44% | **4.96x** |
| Isolation Forest | 9.35% | 52.1% | **5.2x** |

The z-score method alone identified a set of providers nearly **5x more likely** to be confirmed fraudulent than the dataset average — demonstrating that even simple statistical thresholds can meaningfully narrow down where compliance review effort should focus.
