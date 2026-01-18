# Customer Churn Risk Prediction (Retail)

## Overview
This project develops an **end-to-end churn risk prediction framework** for a non-subscription retail business using transactional data.

The emphasis is on **business-first analytics**: realistic assumptions, interpretable modelling, and decision-ready outputs that can be operationalised by marketing and retention teams.

Rather than maximising model complexity, the project prioritises **clarity, defensibility, and practical deployment considerations**.

---

## Relationship to Previous Project
This repository is a **direct extension of**:

**`online-retail-customer-value-rfm`**

Both projects use the **same Online Retail II dataset** and share the same cleaned transactional foundation.

- **Project #1 (`online-retail-customer-value-rfm`)** focuses on customer value analysis and RFM-based segmentation for marketing optimisation.
- **Project #2 (this repository)** builds on that foundation to predict **customer churn risk** and support retention decision-making.

Together, the projects form a coherent analytics pipeline:
- *Who are our most valuable customers?*
- *Which customers are at risk of leaving?*
- *How should limited retention budgets be prioritised?*

---

## Business Problem
In non-subscription retail, churn is rarely explicitly labelled. Customers simply stop purchasing.

This creates practical challenges for marketing and retention teams:
- Identifying customers most likely to disengage
- Prioritising retention efforts under constrained budgets
- Explaining churn risk drivers to non-technical stakeholders

This project addresses these challenges using a **snapshot-based, forward-looking churn definition** combined with interpretable modelling.

---

## Data
- **Source:** Online Retail II dataset  
- **Time Period:** 2009–2011  
- **Granularity:** Transaction-level retail data  
- **Key Fields:** Invoice date, customer ID, quantity, unit price, country  

The same cleaned dataset used in `online-retail-customer-value-rfm` is reused here to ensure analytical consistency.

---

## Churn Definition
Because churn is not explicitly labelled, an industry-standard proxy is applied.

- A **snapshot date** is selected.
- A customer is labelled as **churned** if they make **no purchases in the 90 days following the snapshot**.

### Why 90 Days?
- Typical retail purchase cycles occur within 30–60 days
- 90 days balances false positives and missed churn
- Maintains statistical power while reflecting meaningful disengagement

This approach avoids data leakage and mirrors how churn models are deployed in practice.

---

## Feature Engineering
Customer-level behavioural features are constructed using **only data prior to the snapshot date**:

- **Recency:** Days since last purchase  
- **Frequency:** Number of unique purchase occasions  
- **Monetary Value:** Total historical spend  
- **Average Order Value:** Mean transaction value  

These features are interpretable, business-aligned, and widely used in customer analytics.

---

## Modelling Approach
A **logistic regression model** is used to estimate churn risk.

### Why Logistic Regression?
- Interpretable coefficients
- Stable and transparent behaviour
- Easy to communicate to business stakeholders
- Appropriate given the available feature set

Moderate class imbalance is handled using `class_weight="balanced"` rather than resampling.

---

## Key Findings
- **Model performance:** ROC AUC ≈ **0.72**, indicating effective ranking of churn risk
- **Primary churn drivers:**
  1. **Recency** – strongest indicator of disengagement
  2. **Frequency** – declining purchase activity signals weakening habits
  3. **Monetary value** – lower lifetime spend reflects reduced commitment
- **Risk distribution:**
  - ~10% **High Risk**
  - ~20% **Medium Risk**
  - ~70% **Low Risk**

These findings align with established retail behaviour patterns and provide a defensible basis for prioritisation.

---

## Business Outputs
The model produces **decision-ready artefacts** suitable for operational use:

### Risk Tables
- **`customer_churn_risk_table.csv`**  
  Full customer-level table containing churn probabilities and risk tiers (High / Medium / Low), ranked by predicted risk.

- **`top_at_risk_customers.csv`**  
  A prioritised subset of customers representing the highest churn risk, suitable for direct targeting by retention teams.

All outputs are generated programmatically to ensure reproducibility.

---

## Business Application
In practice, this framework would support:
- Periodic churn risk scoring (weekly or monthly)
- Capacity-aware targeting of high-risk customers
- Alignment of intervention intensity with risk tier

Rather than applying blanket campaigns, retention efforts can be focused where they are most likely to prevent revenue loss.

---

## Limitations & Future Extensions
This project is intentionally scoped as a **baseline, interpretable churn model**.

Potential extensions include:
- Seasonal and trend-based features
- Rolling snapshot validation
- Cost-based threshold optimisation
- Ensemble model comparison
- Survival (time-to-churn) analysis

These are acknowledged but not implemented to preserve clarity and interpretability.

---

## Conclusion
This project demonstrates a **practical, business-driven approach to churn prediction** in non-subscription retail.

Combined with `online-retail-customer-value-rfm`, it forms a structured analytics narrative:
- Understand customer value
- Predict customer risk
- Enable data-driven retention decisions

---

*All figures and outputs are generated programmatically via the final notebook cell to ensure full reproducibility.*


