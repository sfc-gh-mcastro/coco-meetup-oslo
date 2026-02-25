# Machine Learning with Snowflake Cortex Code

<a href="https://www.snowflake.com/en/product/features/cortex-code/">
  <img src="https://www.snowflake.com/wp-content/uploads/2025/06/cortex-code-hero.png" alt="Snowflake Cortex Code" width="600"/>
</a>

---

## About the Event

This repository was presented at the [**Oslo Data & Analytics Meetup**](https://www.meetup.com/oslo-data-analytics/events/312746690/) on **February 24, 2026** — *"Data fra Innsiden - Virksomhet og Plattform - Norges Bank | Snowflake"* at Rebel, Oslo.

**Presenter:** Marcel Castro — Senior Solution Engineer / Data Scientist at Snowflake

---

## What's in This Repo

A machine learning project built entirely using [**Cortex Code**](https://www.snowflake.com/en/product/features/cortex-code/) — Snowflake's AI coding assistant for data & AI work. The project walks through a complete ML workflow from the CLI: data ingestion, exploratory data analysis, feature engineering, model training (XGBoost), explainability (SHAP), model registration, monitoring, and SQL inference — all within Snowflake.

> **Try Cortex Code for free:** [signup.snowflake.com/cortex-code](https://signup.snowflake.com/cortex-code)

---

## Mortgage Lending ML Project

### Project Overview

End-to-end machine learning pipeline for predicting mortgage approval outcomes using Snowflake's ML capabilities.

**Database:** `COCO-MEETUP-OSLO.PUBLIC`  
**Data:** 369,245 mortgage applications from Jan-Nov 2024

---

### Session Summary

#### 1. Data Ingestion
- Loaded CSV file from stage `@DATA_STAGE/MORTGAGE_LENDING_DEMO_DATA.csv`
- Created table `MORTGAGE_LENDING` with 369,245 records
- **Columns:** LOAN_ID, TS, LOAN_TYPE_NAME, LOAN_PURPOSE_NAME, APPLICANT_INCOME_000S, LOAN_AMOUNT_000S, COUNTY_NAME, MORTGAGERESPONSE

#### 2. Exploratory Data Analysis
- Overall approval rate: **78.4%**
- Average loan amount: **$324K**
- 4 loan types across 63 counties
- Visualizations: approval rates, loan distributions, county analysis, monthly trends

#### 3. Feature Engineering
- Encoded categorical variables (LOAN_TYPE, PURPOSE, COUNTY)
- Created `INCOME_TO_LOAN_RATIO` feature
- Final dataset: 315,746 records (after dropping nulls)

#### 4. Model Training

**Random Forest**

| Metric | Value |
|--------|-------|
| Accuracy | 78.5% |
| ROC-AUC | 0.732 |

**XGBoost (Selected)**

| Metric | Value |
|--------|-------|
| Accuracy | 79.0% |
| ROC-AUC | 0.744 |
| Improvement | +1.21% |

**Parameters:** n_estimators: 100, max_depth: 6, learning_rate: 0.1, subsample: 0.8

#### 5. Model Explainability (SHAP)
- Added TreeExplainer for XGBoost
- Generated feature importance rankings
- Created single prediction explanations
- Key drivers: PURPOSE_ENC, LOAN_TYPE_ENC, INCOME_TO_LOAN_RATIO

#### 6. Model Registration
Registered to Snowflake Model Registry:
- **Model:** `MORTGAGE_APPROVAL_MODEL`
- **V1:** SPCS inference
- **V2_WAREHOUSE:** Warehouse inference (SQL-compatible)

#### 7. Model Monitoring
- Created `MORTGAGE_PREDICTIONS` table (63,150 predictions)
- Created `MODEL_MONITORING_DASHBOARD` view tracking:
  - Hourly accuracy
  - Confidence scores
  - Predicted vs actual approvals
  - Feature distributions

#### 8. SQL Inference
- Enabled batch inference using `mv_warehouse.run()`
- Demonstrated predictions on new data from source table

#### 9. Segment Analysis

**Best performing segments:**
- Home Purchase loans: 81%+ accuracy
- Tompkins County: 87.7% approval rate

**Challenging segments:**
- Home Improvement: 59-66% accuracy
- Sullivan County: 62.9% approval rate

---

### Artifacts Created

| Object | Type | Location |
|--------|------|----------|
| MORTGAGE_LENDING | Table | COCO-MEETUP-OSLO.PUBLIC |
| MORTGAGE_PREDICTIONS | Table | COCO-MEETUP-OSLO.PUBLIC |
| MODEL_MONITORING_DASHBOARD | View | COCO-MEETUP-OSLO.PUBLIC |
| MORTGAGE_APPROVAL_MODEL | Model | COCO-MEETUP-OSLO.PUBLIC |
| mortgage_eda.ipynb | Notebook | Workspace |

---

### Key Metrics Summary

| Metric | Value |
|--------|-------|
| Total Records | 369,245 |
| Training Records | 252,596 |
| Test Records | 63,150 |
| Model Accuracy | 79.0% |
| ROC-AUC | 0.744 |
| Approval Rate | 78.4% |

---

### Next Steps & Suggestions

#### Immediate Improvements

1. **Handle Class Imbalance**
   - Current denied recall is only 19%
   - Try SMOTE oversampling or class_weight parameter
   ```python
   from imblearn.over_sampling import SMOTE
   smote = SMOTE(random_state=42)
   X_train_bal, y_train_bal = smote.fit_resample(X_train, y_train)
   ```

2. **Hyperparameter Tuning**
   - Use GridSearchCV or Optuna for better parameters
   ```python
   from sklearn.model_selection import GridSearchCV
   param_grid = {
       'max_depth': [4, 6, 8],
       'learning_rate': [0.05, 0.1, 0.2],
       'n_estimators': [100, 200]
   }
   ```

3. **Feature Engineering**
   - Add debt-to-income ratio
   - Time-based features (month, quarter)
   - County-level aggregates (avg approval rate per county)

#### Model Deployment

4. **Deploy REST API Endpoint**
   ```python
   mv.deploy(
       deployment_name="mortgage_predictor",
       platform="SNOWPARK_CONTAINER_SERVICES",
       num_workers=1
   )
   ```

5. **Create Streamlit App**
   - Interactive loan approval predictor
   - Input form for applicant details
   - Real-time prediction with SHAP explanation

#### MLOps & Monitoring

6. **Set Up Drift Detection**
   - Compare feature distributions over time
   - Alert when accuracy drops below threshold

7. **A/B Testing Framework**
   - Compare V1 vs V2 model performance
   - Track business metrics (approval rate, default rate)

8. **Automated Retraining Pipeline**
   - Schedule monthly model refresh
   - Use Snowflake Tasks for automation

#### Advanced Analytics

9. **Fairness Analysis**
    - Analyze approval rates across demographics
    - Ensure model doesn't discriminate

10. **What-If Analysis**
    - Simulate how changes in income/loan amount affect approval
    - Create interactive scenarios

---

### Quick Start Commands

```sql
-- View model
SHOW MODELS IN SCHEMA "COCO-MEETUP-OSLO".PUBLIC;

-- Check predictions table
SELECT * FROM "COCO-MEETUP-OSLO".PUBLIC.MORTGAGE_PREDICTIONS LIMIT 10;

-- Monitor performance
SELECT * FROM "COCO-MEETUP-OSLO".PUBLIC.MODEL_MONITORING_DASHBOARD;

-- Get model functions
SHOW FUNCTIONS IN MODEL "COCO-MEETUP-OSLO".PUBLIC.MORTGAGE_APPROVAL_MODEL;
```

```python
# Load model for inference
from snowflake.ml.registry import Registry
reg = Registry(session=session, database_name='"COCO-MEETUP-OSLO"', schema_name='PUBLIC')
mv = reg.get_model('MORTGAGE_APPROVAL_MODEL').version('V2_WAREHOUSE')

# Run predictions
predictions = mv.run(your_dataframe, function_name='predict')
```

---

### What Else Can You Do with CoCo?

#### Access and Permissions

| Use case | Example prompt |
|:--|:--|
| Access discovery | "What databases do I have access to?" |
| Security auditing | "Find all tables that have PII in them." |

#### Data Discovery

| Use case | Example prompt |
|:--|:--|
| Tag discovery | "List every table tagged PII = TRUE in ANALYTICS_DB." |
| Lineage and tagging | "Show the lineage from RAW_DB.ORDERS to downstream dashboards." |
| Metadata search | "Where can I find tables related to customer churn and subscription status?" |

#### SQL Development and Optimization

| Use case | Example prompt |
|:--|:--|
| Logic explanation | "What does this SQL script do?" |
| Generation | "Write a query for top 10 customers by revenue and a 7-day moving average." |
| Query refinement | "Update the top performers query to show the top 100." |
| Performance optimization | "Explain why this query is slow and optimize it." |
| Data synthesis | "Generate synthetic data for 30 days of sales for an e-commerce site in the SAMPLESDATA.SALES table." |

#### Machine Learning and Engineering Pipelines

| Use case | Example prompt |
|:--|:--|
| Notebooks (EDA and machine learning) | "Build me a notebook for a customer churn prediction use case using pandas for data handling, matplotlib and seaborn for EDA and visualization, and scikit-learn for preprocessing, model training (logistic regression and a tree-based model), evaluation, and interpretation, with clear markdown explaining business impact and results." |
| Deep learning | "Create a new notebook and build a CNN for the MNIST dataset." |
| Pipeline engineering | "Create a dbt project to transform raw sales data." |

---

*Last Updated: February 25, 2026*
