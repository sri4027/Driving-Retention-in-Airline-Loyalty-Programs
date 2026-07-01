# Unlocking Behavioral Intelligence in Airline Loyalty Programs

**Churn Prediction · Customer Segmentation · Smart Retention**
Consulting & Analytics Club, IIT Guwahati — June 2026

An end-to-end churn prediction and segmentation system built on 16,737 airline loyalty
records (2012–2018), designed to move a mid-sized Canadian airline's loyalty program
from reactive points-and-rewards thinking to proactive, targeted retention.

---

## Key Results

| Metric | Value |
|---|---|
| Members analyzed | 16,737 |
| Churn rate | 7.8% (1,310 members) |
| Model | Random Forest, 5-fold Stratified CV |
| Model performance | OOF AUC = 0.840 ± 0.003 (leakage-free) |
| High-Risk members identified | 1,602 (top 10% by OOF churn probability) |
| Behavioral segments | 4 (via K-Means on RFM + redemption rate) |
| Highest-value segment | Premium Loyalists — 4.4× average CLV (£27,729 vs £8,000 mean) |
| Highest-risk segment | Dormant Members — 11.5% churn rate, 628 High-Risk members |
| Retention framework | 12-cell action matrix (4 segments × 3 risk tiers) — who, when, what offer, which channel, why |

---

## Project Structure

```
├── airline__9___3_.ipynb        # Full analysis: cleaning, feature engineering, modeling, segmentation
├── Technical_Report.pdf         # Full write-up: methodology, findings, business recommendations
├── WORKING_PROTOTYPE.html       # Interactive "Member Retention Manifest" dashboard
│                                 #   (KPI cards, segment breakdown, prioritized action manifest, playbook)
└── README.md                    # This file
```

---

## Methodology

### 1. Churn Definition (3 signals, 2018 data used only for labeling — never as a feature)
- **Formal Churn** — cancelled in 2017/2018 (1,147 members)
- **Silent Disengagement** — enrolled pre-2017, zero 2017 flights, above-median CLV, no formal cancel (130 members)
- **Late-Stage Vanishers** — active in 2017, completely silent in 2018 (33 members)

### 2. Data Cleaning
Negative salaries imputed via group median (Education × Marital Status); duplicate activity
rows deduplicated/aggregated; impossible cancellation records nullified; negative activity
values clipped to zero; CLV outliers flagged (not dropped); ordinal encoding applied per
data dictionary; daily calendar merged in to derive quarter/season features.

### 3. Feature Engineering (25+ features, all from 2017-and-earlier data)
Flight volume, recency & momentum, points economics, tenure/lifecycle, seasonal
decomposition, and interaction terms (e.g. `tier_x_recent_silence`, `clv_x_negative_trend`).

### 4. Modeling
Random Forest (200 trees, `class_weight="balanced"`), validated with 5-fold Stratified
KFold. All churn probabilities are out-of-fold. Risk tiers assigned via quantile cuts:
Low (bottom 70%), Medium (70th–90th percentile), High (top 10%).

### 5. Segmentation
K-Means (k=4) on standardized Recency, Frequency, Monetary (CLV), and Redemption Rate.
Segments are named by empirical profile, not arbitrary cluster ID, for stability across re-runs.

| Segment | Members | Avg CLV | Churn Rate | High-Risk |
|---|---|---|---|---|
| Premium Loyalists | 1,109 | £27,729 | 7.7% | 82 |
| Frequent Flyers | 9,755 | £6,380 | 6.0% | 662 |
| Active Engagers | 1,564 | £6,638 | 9.2% | 230 |
| Dormant Members | 4,309 | £7,041 | 11.5% | 628 |

### 6. Retention Strategy
A 12-cell matrix (4 segments × 3 risk tiers) prescribes a specific action, offer, timing,
and channel per cell — from personal phone outreach for High-Risk Premium Loyalists to
low-cost newsletters for healthy segments.

---

## Business Recommendations

1. **Silent High-Value Rescue Protocol** — target 130 silent disengages + 628 Dormant
   High-Risk members with an immediate win-back offer (~£4.4M value at stake).
2. **Protect Premium Loyalists proactively** — personal outreach for High-Risk, quarterly
   VIP rewards for all others, regardless of current risk signal.
3. **Redirect ~20% of acquisition budget to targeted retention** — cutting churn from
   7.8% to 5% preserves ~470 members/year (~£3.9M annually).

---

## Tech Stack

- **Python** (pandas, numpy, scikit-learn) — data cleaning, feature engineering, modeling
- **scikit-learn** — Random Forest, Stratified K-Fold CV, KMeans, StandardScaler
- **HTML** — interactive retention dashboard prototype

---

## Limitations

- Activity data covers 2017–2018 only; earlier engagement history is proxied, not observed.
- OOF churn probabilities are well-ordered but not calibrated (would need Platt scaling /
  isotonic regression for exact expected-value budgeting).
- The 3-signal churn definition is analytical and should be validated against the airline's
  operational definition before production deployment.
- K-Means cluster IDs are not stable across reruns without a fixed random seed; naming
  rules must be reapplied on refresh.

---

## Deliverables

- Random Forest churn model (OOF AUC 0.840, leakage-free)
- 16,737 members scored with churn probability + risk tier
- 4 behavioral segments (Premium Loyalists, Frequent Flyers, Active Engagers, Dormant Members)
- 12-cell retention action matrix
- Interactive retention dashboard (`WORKING_PROTOTYPE.html`)
- `retention_actions_all.csv`, `retention_actions_urgent.csv` (exportable priority lists)
