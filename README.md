# Fintech Credit Risk Analysis & Loan Risk Prediction

## Project Overview

Credit risk is a major challenge for fintech lenders. While expanding access to credit can drive growth, lending to borrowers with a higher likelihood of repayment difficulty can increase portfolio risk and financial losses.

This project uses Python, exploratory data analysis, and machine learning to investigate borrower and loan characteristics associated with adverse loan performance and develop a model for identifying higher-risk borrowers.

The project is approached from both a **business analytics and data science perspective**, with an emphasis on translating predictive modelling results into actionable insights for lending and risk-management decisions.

---

## Business Problem

A fintech lender wants to better understand:

> **Which borrower and loan characteristics are associated with higher credit risk, and can historical lending data be used to identify borrowers who are more likely to experience repayment difficulties?**

The analysis aims to support:

- Credit-risk assessment
- Underwriting prioritization
- Borrower risk segmentation
- Portfolio monitoring
- Data-driven lending decisions

---

## Dataset

The analysis uses a historical consumer lending dataset containing **10,000 loan records** and borrower, credit, income, debt, and loan-related characteristics.

During initial exploration, the dataset was assessed for:

- Missing values
- Duplicate records
- Variable types
- Numerical distributions
- Loan-status distribution
- Potential data leakage

The raw dataset is intentionally excluded from this repository.

---

## Analytical Approach

The project follows the following workflow:

1. Business problem definition
2. Data understanding and quality assessment
3. Data cleaning
4. Exploratory data analysis
5. Credit-risk target definition
6. Feature selection and data-leakage prevention
7. Data preprocessing
8. Train/test splitting
9. Logistic Regression modelling
10. Random Forest modelling
11. Model evaluation and comparison
12. Credit-risk driver analysis
13. Borrower risk scoring
14. Risk segmentation
15. Classification threshold optimization
16. Business recommendations

---

## Target Definition

The original objective was to examine loan default. However, the dataset contained very few loans classified as **Charged Off**, making default-only modelling unreliable.

A broader **Adverse Loan Status** target was therefore developed.

Adverse outcomes include:

- In Grace Period
- Late (16–30 days)
- Late (31–120 days)
- Charged Off

Current and Fully Paid loans are treated as performing.

This provides a more appropriate target for examining repayment difficulty within the available dataset.

---

## Machine Learning Models

Two classification models were developed:

### Logistic Regression

Used as an interpretable baseline model and to examine the relationship between borrower characteristics and predicted credit risk.

### Random Forest

Used as an alternative model capable of identifying more complex and non-linear relationships.

Because adverse outcomes represent a small proportion of the portfolio, class imbalance was explicitly considered during model development.

Model performance was evaluated using:

- Precision
- Recall
- F1-score
- ROC-AUC
- Confusion matrices

Accuracy was not used as the primary decision metric because of the highly imbalanced target.

---

## Model Performance

The two models produced the following results:

| Model | Adverse Precision | Adverse Recall | Adverse F1-Score | ROC-AUC |
|---|---:|---:|---:|---:|
| Logistic Regression | 0.034 | 0.167 | 0.057 | 0.579 |
| Random Forest | 0.000 | 0.000 | 0.000 | 0.598 |

The results demonstrate the difficulty of predicting adverse loan outcomes within this dataset.

Random Forest achieved the higher ROC-AUC score (0.598), indicating slightly better overall ranking ability. However, at the default classification threshold, it failed to identify any adverse loans.

Logistic Regression achieved an adverse-loan recall of 16.7%, meaning it identified some adverse cases, but its low precision indicates a high number of false-positive classifications.

Neither model currently demonstrates sufficient predictive performance for use as a standalone credit-decision system.

The results highlight an important practical lesson in applied data science: model development does not always produce a highly predictive solution. Dataset limitations, class imbalance, target construction, feature availability, and the maturity of loan outcomes can significantly affect model performance.

---

## Classification Threshold Optimization

Because the adverse-loan class is highly imbalanced, the default 0.50 classification threshold may not provide the most useful balance between identifying risky borrowers and generating false positives.

Alternative probability thresholds were evaluated for the Logistic Regression model to assess the trade-off between adverse-class precision, recall, F1 score, and the number of borrowers flagged for review.

Among the thresholds evaluated, **0.40 produced the highest adverse-class F1 score of 0.070**, compared with 0.057 at the default 0.50 threshold.

At the 0.40 threshold:

- Adverse precision: **3.5%**
- Adverse recall: **25.0%**
- Adverse F1 score: **0.070**
- Borrowers flagged for review: **221**

Lower thresholds produced substantially higher recall but also generated considerably more false-positive classifications. For example, a threshold of 0.10 increased adverse recall to 61.1% but reduced precision to 2.3% and flagged 961 borrowers.

The results demonstrate that threshold adjustment can improve the trade-off between identifying adverse borrowers and the operational burden created by additional reviews. However, low precision across all evaluated thresholds indicates that threshold optimization alone cannot overcome the underlying limitations of the model and dataset.

---

## Risk Segmentation

Predicted risk scores were used to divide borrowers into four risk groups.

| Risk Segment | Borrowers | Actual Adverse Loans | Average Predicted Risk | Actual Adverse Rate |
|---|---:|---:|---:|---:|
| Low Risk | 500 | 6 | 1.70% | 1.20% |
| Moderate Risk | 500 | 8 | 6.16% | 1.60% |
| High Risk | 500 | 10 | 15.40% | 2.00% |
| Very High Risk | 500 | 12 | 46.63% | 2.40% |

Observed adverse rates increased consistently across the risk segments.

The **Very High Risk group experienced an adverse rate twice that of the Low Risk group**, indicating that the model provides useful information for ranking borrowers according to relative risk.

However, predicted probabilities were substantially higher than observed adverse rates. The model should therefore currently be interpreted as a **risk-ranking tool rather than a calibrated probability-of-default model**.

---

## Business Recommendations

The analysis suggests several potential applications and next steps for a fintech lender:

- Use model-generated risk rankings to prioritize applications requiring additional underwriting review.
- Apply enhanced affordability and credit-history assessments to borrowers identified as higher risk.
- Support more streamlined processing for lower-risk applicants while maintaining existing underwriting requirements.
- Use risk segmentation to prioritize portfolio monitoring and early intervention.
- Improve probability calibration before interpreting predicted scores as absolute risk probabilities.
- Consider a lower classification threshold, such as 0.40, for risk-screening purposes where identifying additional adverse borrowers justifies the additional review burden.
- Determine production thresholds based on the business costs of false negatives and false positives rather than model metrics alone.
- Use predictive analytics as a decision-support mechanism rather than replacing established lending controls and human judgment.

---

## Limitations

This project is an analytical prototype rather than a production credit-decision system.

Important limitations include:

- Significant class imbalance
- Very few charged-off loans
- A broader adverse-status definition was required
- Many loans remain active and their ultimate outcomes are unknown
- Predicted probabilities require further calibration
- Threshold optimization produced only a modest improvement in classification performance
- No external or temporal validation has been performed
- Additional fairness, governance, regulatory, and model-monitoring assessments would be required before production use

---

## Tools & Technologies

- Python
- Pandas
- NumPy
- Matplotlib
- Scikit-learn
- Jupyter Notebook
- Visual Studio Code
- Git
- GitHub

---

## Repository Structure

```text
fintech-credit-risk-analysis/
│
├── data/
│   ├── raw/
│   └── processed/
│       ├── borrower_risk_scores.csv
│       ├── model_comparison.csv
│       └── risk_segment_summary.csv
│
├── images/
├── notebooks/
│   └── fintech_credit_risk_analysis.ipynb
│
├── src/
├── .gitignore
├── requirements.txt
└── README.md
```

---

## Conclusion

This project demonstrates an end-to-end application of business analytics and machine learning to a fintech credit-risk problem, from data preparation and exploratory analysis through predictive modelling, risk segmentation, threshold optimization, and business decision support.

The modelling results illustrate an important reality of applied data science: not every dataset produces a highly predictive model. Both Logistic Regression and Random Forest demonstrated limited classification performance, largely within the context of a highly imbalanced target and limited observed adverse loan outcomes.

Despite these limitations, the analysis identified some ability to rank borrowers by relative risk. Observed adverse rates increased progressively from 1.20% among Low Risk borrowers to 2.40% among Very High Risk borrowers.

Threshold optimization further demonstrated the trade-off between identifying additional adverse borrowers and increasing the number of borrowers requiring review. Among the thresholds evaluated, a **0.40 classification threshold produced the highest adverse-class F1 score of 0.070**, compared with 0.057 at the default 0.50 threshold. At 0.40, adverse recall improved to 25.0%, with precision of 3.5% and 221 borrowers flagged for review.

Although threshold optimization produced a modest improvement, precision remained low across all evaluated thresholds. The model should therefore not be deployed as an automated credit-decision system. Instead, its current value is better positioned as an analytical prototype for borrower risk prioritization, portfolio monitoring, and decision support.

The appropriate next steps include further data collection, maturation of loan outcomes, feature development, probability calibration, continued threshold evaluation based on business costs, and rigorous model validation before any production deployment.

Ultimately, this project demonstrates not only the ability to build predictive models, but also the ability to critically evaluate model performance, recognize analytical limitations, quantify business trade-offs, and translate data-science findings into responsible business recommendations.
