# Oslo Meetup Demo Script

> **Dataset:** `MORTGAGE_LENDING_DEMO_DATA.csv` — 369K mortgage applications with approval outcomes.
> **Goal:** End-to-end ML pipeline from raw CSV to deployed model + Snowflake Intelligence agent.

---

## Part 1: Data Setup (Cortex Code CLI)

> *Open a terminal with Cortex Code CLI.*

**1.1 — Explore the data**

```
Given the dataset for a mortgage lending prediction exercise, give me a quick overview of the data.
```

**1.2 — Create database and schema**

```
Create a database and schema to be used in this project.
```

**1.3 — Upload CSV to stage**

```
Upload the CSV data to a stage in the database and schema we just created.
```

---

## Part 2: Data Ingestion & EDA (Cortex Code in Snowsight)

> *Switch to Snowsight and open a Cortex Code workspace.*

**2.1 — Load data into a table**

```
Load the data from the stage into a table for easier analysis.
```

**2.2 — Run exploratory analysis**

```
Run exploratory analysis in a Python notebook.
```

> *Cortex Code will create a notebook with visualizations: approval rates, loan distributions, county analysis, monthly trends.*

---

## Part 3: Machine Learning (Cortex Code in Snowsight)

**3.1 — Train a model**

```
Based on this data, build a machine learning model to predict mortgage approval.
```

> *Cortex Code will likely choose a Random Forest. After it finishes, ask:*

```
Which model did you create?
```

**3.2 — Try XGBoost**

```
Let's try with XGBoost instead.
```

> *Optionally, use `/plan` to review the implementation plan before execution.*

**3.3 — Register the model**

```
Register the XGBoost model to the Snowflake Model Registry for versioning and governance. Make sure you use Warehouse.
```

**3.4 — Add explainability**

```
Add model explainability using SHAP.
```

**3.5 — Run SQL inference**

```
Run SQL inference using the registered model.
```

**3.6 — (Optional) Segment analysis**

```
Analyze prediction performance by loan type and county to identify which segments have the highest approval rates.
```

---

## Part 4: Session Summary & Continuity

**4.1 — Generate documentation**

```
Summarize all the steps done in this session in a markdown file named readme.md. Also create a session with suggestions for next steps that I can use next time to continue developing this.
```

**4.2 — Discuss continuity**

```
What is the best way to start from where I stopped here?
```

> *Cortex Code will explain session resume, forking, and rewind capabilities.*

---

## Part 5: Semantic View & Snowflake Intelligence Agent

**5.1 — Create semantic view and agent**

```
Create a semantic view named COCO_MORTGAGE_SEMANTIC_VIEW in "COCO-MEETUP-OSLO". Then create a Snowflake Intelligence agent named COCO_MORTGAGE_DEMO_AGENT using that semantic view. The agent should be used to investigate the data from "COCO-MEETUP-OSLO".PUBLIC.MORTGAGE_LENDING.
```

> *This creates the natural-language query interface over the mortgage data.*