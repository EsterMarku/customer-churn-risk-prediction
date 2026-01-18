# Customer Churn Risk Prediction (Retail)

## Overview
This project develops an **end-to-end churn risk prediction framework** for a non-subscription retail business using transactional data.

The focus is on **business-first analytics**: realistic assumptions, interpretable modelling, and decision-ready outputs that can be understood and acted upon by non-technical stakeholders.

Rather than maximising model complexity, the project prioritises **clarity, defensibility, and practical deployment considerations**.

---

## Relationship to Previous Project
This repository is a **direct extension of**:

**`online-retail-customer-value-rfm`**

Both projects use the **same Online Retail II dataset** and share the same cleaned transactional foundation.

- **Project #1 (`online-retail-customer-value-rfm`)** focuses on customer value analysis and RFM-based segmentation for marketing optimisation.
- **Project #2 (this repository)** builds on that foundation to predict **customer churn risk** and support retention decision-making.

Together, the two projects form a coherent analytics pipeline:
- *Who are our most valuable customers?*
- *Which customers are at risk of leaving?*
- *How should retention efforts be prioritised?*

---

## Business Problem
In non-subscription retail, churn is rarely explicitly labelled. Customers simply stop purchasing.

This creates key challenges for marketing and retention teams:
- Which customers are most likely to disengage?
- How can limited retention resources be allocated effectively?
- How can churn risk be explained clearly to business stakeholders?

This project addresses these questions using a **snapshot-based, forward-looking churn definition** combined with interpretable modelling.

---

## Data
- **Source:** Online Retail II dataset  
- **Time Period:** 2009–2011  
- **Granularity:** Transaction-level retail data  
- **Key Fields:** Invoice date, customer ID, quantity, unit price, country  

The same cleaned dataset used in `online-retail-customer-value-rfm` is reused here to ensure analytical consistency across projects.

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

These features are:
- Interpretable
- Aligned with business intuition
- Widely used in customer analytics

---

## Modelling Approach
A **logistic regression model** is used to estimate churn risk.

### Why Logistic Regression?
- Interpretable coefficients
- Stable performance
- Easy to communicate to non-technical stakeholders
- Appropriate given the available feature set

Moderate class imbalance is handled using `class_weight="balanced"` rather than resampling.

---

## Evaluation
Model performance is evaluated using:

- **ROC AUC (primary metric):** Measures the model’s ability to rank customers by churn risk
- **Precision & Recall (Churn Class):** Interpreted in a retention context
- **Confusion Matrix:** Evaluated at a baseline threshold of 0.5

A ROC AUC of ~0.72 reflects **realistic performance** for retail churn prediction without data leakage or synthetic features.

---

## Interpretation & Business Insight
Coefficient analysis highlights clear behavioural signals:

- **Recency** is the strongest driver of churn risk, indicating disengagement
- Declining **frequency** reflects weakening customer habits
- Lower **monetary value** and **average order value** suggest reduced commitment

These insights translate directly into retention strategy design.

---

## Business Application
In a real deployment, this model would be used to:

- Generate periodic churn risk scores
- Rank customers by predicted risk
- Prioritise high-risk customers for targeted retention interventions

Rather than contacting all customers, resources can be focused where intervention is most likely to prevent revenue loss.

---

## Outputs
All key outputs are generated programmatically to ensure reproducibility:

- **Figures**
  - ROC Curve – Churn Risk Model
  - Confusion Matrix (Threshold = 0.5)
- **Tables**
  - Model coefficients
  - Top at-risk customers

---

## Limitations & Future Extensions
This project is intentionally scoped as a **baseline, interpretable churn model**.

Potential extensions include:
- Seasonal and trend-based features
- Rolling snapshot validation
- Cost-based threshold optimisation
- Ensemble model comparison
- Survival (time-to-churn) analysis

These extensions are acknowledged but not implemented to preserve clarity and interpretability.

---

## Conclusion
This project demonstrates a **practical, business-driven approach to churn prediction** in non-subscription retail.

Combined with `online-retail-customer-value-rfm`, it forms a structured analytics narrative:
- Understand customer value
- Predict customer risk
- Enable data-driven retention decisions

---

*All figures and outputs are generated programmatically via the final notebook cell to ensure full reproducibility.*
