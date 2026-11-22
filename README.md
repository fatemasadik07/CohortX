<div align="center">

# ⚡ CohortX Platform

### *E-Commerce Customer Intelligence: SQL Analytics, Repeat-Purchase Prediction & Interactive Dashboard*

[![Python](https://img.shields.io/badge/Python-E6E6FA?style=for-the-badge&logo=python&logoColor=333333)](#) [![DuckDB](https://img.shields.io/badge/DuckDB-CC76D6?style=for-the-badge&logo=duckdb&logoColor=333333)](#) [![XGBoost](https://img.shields.io/badge/XGBoost-C618D6?style=for-the-badge&logoColor=white)](#) [![Streamlit](https://img.shields.io/badge/Streamlit-DA70D6?style=for-the-badge&logo=streamlit&logoColor=white)](#) [![FastAPI](https://img.shields.io/badge/FastAPI-C71585?style=for-the-badge&logo=fastapi&logoColor=white)](#) [![License: MIT](https://img.shields.io/badge/License-MIT-B0C4DE?style=for-the-badge)](#)

*Turning raw order data into SQL insights, repeat-buyer predictions, and a dashboard that actually tells a story.* ✨

</div>

---

## 💅🏽 Overview

Let's be real: keeping existing customers around is *so* much easier and cheaper than constantly chasing new ones! But most e-commerce teams are sitting on piles of order data without a clear view of **how customers buy** or **who is likely to come back**.

We built **CohortX** to fix that with a super smooth, step-by-step workflow:

1. **Store It Properly:** A relational database holding customers, orders, products, categories, and payments, so the data finally lives in one organised place.
2. **Ask SQL the Big Questions:** Purchase frequency, revenue, average order value, product performance, and repeat-purchase patterns, all answered with clean SQL.
3. **Clean & Explore:** Python handles cleaning, missing values, feature creation, EDA, and correlation analysis, so we know our data before we trust it.
4. **Predict Who Comes Back:** An XGBoost model predicts whether a customer will buy again within X days after a cutoff date.
5. **Show It Off:** A cute, interactive dashboard covering revenue, customers, products, customer segments, and model predictions.

---

## 📊 Dataset

We use the **[Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)** from Kaggle: around **100K orders across 9 tables**, placed on a Brazilian marketplace between 2016 and 2018.

The data is **included in this repo** in `data/raw/`, so you can run everything right after cloning. We kept 8 of the 9 files and left out `olist_geolocation_dataset.csv` to keep the repo light. It isn't needed for our analysis, and you can grab it from Kaggle if you want it.

| File | Used for |
| ---- | -------- |
| `olist_customers_dataset.csv` | Customers |
| `olist_orders_dataset.csv` | Orders and delivery timestamps |
| `olist_order_items_dataset.csv` | Order lines and prices |
| `olist_products_dataset.csv` | Products |
| `product_category_name_translation.csv` | Categories (English names) |
| `olist_order_payments_dataset.csv` | Payments |
| `olist_order_reviews_dataset.csv` | Review scores (model feature) |
| `olist_sellers_dataset.csv` | Sellers (optional analysis) |

> **Heads up:** In this dataset, `customer_id` is generated *per order*. We use `customer_unique_id` to recognise the same person across multiple orders! 🔑

> **License note:** The dataset keeps its original license (see the Kaggle page, which credits Olist) and is not covered by this repo's MIT license.

---

## 🦄 Module Architecture & Responsibilities

The codebase follows a modular design split into six domain-specific directories:

| Directory            | Core Focus            | Module Lead          | Primary Responsibilities                                                                                           |
| -------------------- | --------------------- | -------------------- | ------------------------------------------------------------------------------------------------------------------ |
| `core_warehouse/`    | Data Engineering      | **Youssef El-Kholy** | Relational schema (customers, orders, products, categories, payments), data loading, and SQL analytics queries.    |
| `feature_factory/`   | Data Prep & EDA       | **Menna Ahmed**      | Data cleaning, missing-value treatment, cutoff-based labelling, feature creation, EDA, and correlation analysis.    |
| `intelligence_core/` | Predictive Modelling   | **Fatema Sadik**     | Repeat-purchase XGBoost classifier, baseline comparison, class-imbalance handling, and model evaluation.          |
| `serving_layer/`     | API       | **Rabab Mohamed**    | FastAPI endpoint that serves the trained model's predictions.                                                      |
| `control_center/`    | Dashboards & UI       | **Malak Mostafa**    | Streamlit multi-page dashboard, interactive charts, layout design, and overall user experience.                    |
| `quality_assurance/` | QA & Testing          | **Farah Nasser**     | Data validation, leakage checks, Pytest suites, continuous integration workflows, and linting routines.            |

<div align="center">
  Built with grit, coffee, and teamwork by 5 girls and 1 guy for our DEPI Capstone!🥳
</div>

---

## 🗄️ Database Design

Five core tables, all linked by keys:

```
customers ──< orders ──< order_items >── products >── categories
                │
                └──< payments
```

- Schema files live in `core_warehouse/schemas/`
- SQL analytics queries live in `core_warehouse/queries/`

**SQL insights we cover:** customer purchase frequency, revenue over time, average order value, product & category performance, and repeat-purchase patterns.

---

## 🤖 Machine Learning Task

**Repeat-Purchase Prediction (Binary Classification):** given a customer's order history up to a **cutoff date**, predict whether they buy again within **X days** after it.

- **No peeking!** Features (recency, frequency, order value, payment type, delivery time, review score, product category, etc.) only use orders placed *before* the cutoff.
- **Label:** `1` if the customer orders again within X days after the cutoff, otherwise `0`.
- **Imbalance alert:** Repeat buyers are a small minority, so we use class weighting and judge the model with precision, recall, F1, and PR-AUC instead of accuracy alone.
- **Sanity check:** XGBoost is compared against a simple logistic regression baseline.

---

## 💄 Technology Stack

- **Database & SQL:** DuckDB
- **Data Wrangling & ML:** Python, Pandas, NumPy, Scikit-learn, XGBoost
- **Interface & Visualisation:** Streamlit, Plotly, Matplotlib, Seaborn
- **API (Optional):** FastAPI, Uvicorn
- **Testing & Quality Assurance:** Pytest, Pydantic, GitHub Actions

---

## 🎀 Getting Started

### 🛠️ Environment Setup

Confirm that Python 3.10 or higher is installed before configuring your local environment:

```
# Clone the repository and navigate to the project root
git clone https://github.com/fatemasadik07/CohortX
cd CohortX

# Initialise the virtual environment
python -m venv venv

# Activate on Windows (Command Prompt):
venv\Scripts\activate

# Activate on macOS/Linux:
source venv/bin/activate

# Upgrade pip & install required dependencies
pip install --upgrade pip
pip install -r requirements.txt
```

> **Note:** The dataset is already inside `data/raw/`, so there's nothing to download! 🎉

---

### 🚀 How to Run the System

Run the pipeline modules one by one, in this order:

#### 1. Build the Database & Load the Data 🗄️

Create the schema and load the raw Olist files into DuckDB:

```
python -m core_warehouse.connectors.db_session
```

#### 2. Clean Data & Engineer Features 🧼

Clean the data, handle missing values, apply the cutoff date, and build the features and labels:

```
python -m feature_factory.pipelines.cleaning
```

#### 3. Train & Evaluate the Model 🤖

Train the XGBoost classifier and compare it against the baseline:

```
python -m intelligence_core.trainers.train_xgboost
```

#### 4. Launch the Dashboard 💻

Start the multi-page dashboard:

```
streamlit run control_center/app.py
```

The app will automatically open at [http://localhost:8501](http://localhost:8501).

#### 5. Run the Test Suite ✅

Validate the data and make sure nothing leaks past the cutoff:

```
pytest quality_assurance/suite/
```

#### ✨ Serve the Model with FastAPI

```
uvicorn serving_layer.app.main:app --reload --port 8000
```

Access the interactive API docs at [http://localhost:8000/docs](http://localhost:8000/docs).

---

## 📈 Dashboard Pages

| Page                  | What You'll See                                                  |
| --------------------- | ---------------------------------------------------------------- |
| 💰 Revenue            | Revenue over time, average order value, payment methods          |
| 👯‍♀️ Customers          | Customer counts, geography, new vs. repeat buyers                |
| 🛍️ Products           | Top products and categories, product performance                 |
| 🎯 Customer Segments  | RFM-based segments and how each group behaves                    |
| 🔮 Model Predictions  | Repeat-purchase probabilities and model performance              |

---

## 🏆 Results

<!-- Fill in after training: baseline vs. XGBoost (precision, recall, F1, PR-AUC),
     top features, and 2-3 key EDA findings. -->

*Coming soon, once the model finishes training!* 💫

---

## ⚠️ Limitations

- Repeat buyers are rare in this dataset, so predictions for them are uncertain.
- The data covers 2016-2018 on a single marketplace, so findings may not generalise.
- `customer_unique_id` can't link a person who used different accounts.

---

## 📚 DEPI Documentation

Project planning, specification, and evaluation records are located in the `.depi/` directory:

| Document                   | Description                                       |
| -------------------------- | ------------------------------------------------- |
| `01_PROJECT_PLANNING.md`   | Project Scope, Milestones, and Timelines          |
| `02_REQUIREMENTS_SPECS.md` | System Specifications and Functional Requirements |
| `03_LITERATURE_REVIEW.md`  | Literature Review and Methodological Background   |

*Developed with ✨ for the Digital Egypt Pioneers Initiative (DEPI) Capstone Project.*
