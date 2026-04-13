# 📊 Behavioral Credit Risk Modeling Pipeline

## Project Overview

This repository presents a comprehensive, production-grade pipeline for **Behavioral Credit Risk Modeling**, designed to predict the probability that a customer will experience **financial distress**, defined as becoming **90+ days past due within a two-year horizon**. Unlike conventional binary classification problems, this project addresses the real-world complexities inherent in banking data systems, including severe class imbalance, legacy data artifacts, and highly non-linear relationships between financial behaviors and default risk.

The modeling approach is built with a strong emphasis on **practical applicability in financial institutions**, where accuracy alone is insufficient. Instead, the pipeline balances predictive performance with interpretability, regulatory compliance, and operational usability. By integrating robust data cleaning, advanced feature engineering, and cost-sensitive learning, the model is tailored to support risk-based decision-making in a controlled and auditable manner.

---

## 🛠️ Methodology & Technical Architecture

At the core of this project lies a **Cost-Sensitive Random Forest framework**, chosen deliberately over traditional approaches such as Logistic Regression. While logistic models assume linear relationships and independence between predictors, Random Forests excel at capturing **complex, non-linear interactions** between variables. This is particularly important in credit risk, where combinations of factors—such as high debt levels combined with recent delinquencies—can amplify risk disproportionately.

The cost-sensitive aspect of the model ensures that **misclassification penalties are asymmetric**, reflecting the business reality that failing to identify a high-risk borrower (false negative) is far more costly than incorrectly flagging a low-risk customer (false positive). This design choice aligns the model’s optimization objective with real-world financial risk management priorities.

---

## 🔍 Data Integrity & Forensic Cleaning

A significant portion of the pipeline is dedicated to **Forensic Data Cleaning**, recognizing that raw banking data is often noisy, inconsistent, and shaped by legacy system constraints. One of the key challenges addressed in this project was the presence of **"98" and "99" masking codes** in delinquency-related variables. These values are commonly used in legacy systems to represent missing or undefined data rather than actual observations.

Instead of discarding these records—which would reduce the dataset size and potentially introduce bias—we applied a **targeted imputation strategy**. Specifically, median values were computed within relevant sub-populations to preserve the structural integrity of the dataset while eliminating mathematical distortions.

Additionally, we implemented a **Hybrid Capping Strategy** for highly skewed variables such as `DebtRatio` and `RevolvingUtilization`. Extreme outliers—such as utilization values exceeding 300,000%—were systematically capped to prevent them from dominating model variance and destabilizing predictions. This approach ensures that the model remains robust while still retaining meaningful variation in the data.

---

## ⚙️ Behavioral Feature Engineering

To enhance predictive performance, the project moves beyond raw input variables and constructs **behavioral indicators** that better capture underlying financial dynamics.

Key engineered features include:

- **TotalLateEvents**: A cumulative measure representing the frequency of late payments, effectively capturing the *velocity of financial stress* rather than isolated incidents.
- **MonthlyDebtValue**: A reconstructed metric translating the debt-to-income ratio into an absolute monetary burden, providing clearer economic context.
- **High-Stress Synergies**: Binary interaction features that activate when multiple risk thresholds are breached simultaneously, such as high utilization combined with elevated debt ratios.

These features are designed to reflect **real-world borrower behavior**, enabling the model to detect early warning signals that may not be apparent from static variables alone.

---

## 📈 Model Performance & Validation

The model was evaluated using a **Stratified Validation Split**, ensuring that the minority class—representing default events, comprising approximately **6.68% of the dataset**—is proportionally preserved across training and validation sets.

Performance metrics demonstrate strong predictive capability:

- **ROC-AUC Score: 0.8636**  
  This indicates excellent ability to rank-order customers by risk, a critical requirement in credit scoring.

- **Sensitivity (Recall): 0.73**  
  The model successfully identifies nearly three-quarters of all potential default cases, minimizing the risk of undetected high-risk borrowers.

- **Calibration Strategy**  
  By using `class_weight='balanced'`, the model applies an approximate **14× penalty** for misclassifying a high-risk customer. This ensures a conservative bias, prioritizing risk containment and capital preservation over aggressive lending.

---

## 🏦 The Banking Masterscale

To translate model outputs into actionable business insights, predicted probabilities are mapped onto a **10-tier Banking Masterscale**, providing a standardized risk grading system.

- **Prime Segments (Grades 1–3)**  
  These customers exhibit very low risk, with observed default rates below **1.0%**, making them suitable for favorable lending terms.

- **Sub-prime Segments (Grades 8–10)**  
  Risk increases sharply in these tiers, with **Grade 10 reaching an observed default rate of 35.5%**, indicating severe financial distress.

A key strength of the model is its **perfect monotonicity**, meaning that each successive grade corresponds to a strictly increasing default rate. This property is essential for regulatory compliance and aligns with frameworks such as **Basel III** and **SR 11-7 Model Risk Management**, ensuring that the scorecard behaves consistently and predictably.

---

## 🔧 Tools & Technology Stack

The pipeline is implemented using widely adopted, industry-standard tools:

- **Programming Language**: Python  
- **Data Manipulation**: pandas, numpy  
- **Machine Learning**: scikit-learn (`RandomForestClassifier`)  
- **Visualization**: matplotlib, seaborn  
- **Evaluation Metrics**: ROC-AUC, Precision-Recall Curve, Confusion Matrix  

This stack ensures scalability, reproducibility, and ease of integration into existing data science workflows.

---

## ⚖️ Pros and Cons of the Approach

### Advantages

The use of a Random Forest model provides significant benefits in capturing **non-linear and non-monotonic relationships**, such as the complex interaction between age, income, and credit behavior. Ensemble methods also offer strong **robustness to noise**, which is particularly valuable given the presence of missing or imperfect data (e.g., 20% missing `MonthlyIncome`).

Another major advantage is the transformation of raw model outputs into a **1–10 Masterscale**, which makes the results immediately interpretable and actionable for business stakeholders, including credit analysts and risk managers.

### Limitations

However, the approach is not without trade-offs. The emphasis on high recall leads to a **lower precision (0.24)**, meaning that a larger number of low-risk customers may be incorrectly flagged as high risk. While this is acceptable from a risk containment perspective, it may result in suboptimal customer experience.

Additionally, the model is **computationally intensive**, requiring the training of 100+ decision trees on a dataset of approximately 150,000 records. This makes it more resource-demanding compared to simpler linear models, particularly in environments with limited computational capacity.

---

## Final Remarks

This project demonstrates how a thoughtfully designed machine learning pipeline can bridge the gap between **technical modeling and real-world financial application**. By addressing data quality challenges, incorporating behavioral insights, and aligning model outputs with business and regulatory requirements, the solution provides a robust foundation for modern credit risk management.
