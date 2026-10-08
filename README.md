<div align="center">

# ⚡ CohortX Platform 💖
### *E-Commerce Customer LTV Modeling, SHAP Explainability & Interactive Control Center*

[![Python](https://img.shields.io/badge/Python-E6E6FA?style=for-the-badge&logo=python&logoColor=333333)](#)
[![SQLite](https://img.shields.io/badge/SQLite-D8BFD8?style=for-the-badge&logo=sqlite&logoColor=333333)](#)
[![FastAPI](https://img.shields.io/badge/FastAPI-C71585?style=for-the-badge&logo=fastapi&logoColor=white)](#)
[![Streamlit](https://img.shields.io/badge/Streamlit-DA70D6?style=for-the-badge&logo=streamlit&logoColor=white)](#)
[![Docker](https://img.shields.io/badge/Docker-8A2BE2?style=for-the-badge&logo=docker&logoColor=white)](#)
[![MLflow](https://img.shields.io/badge/MLflow-9370DB?style=for-the-badge&logo=mlflow&logoColor=white)](#)
[![License: MIT](https://img.shields.io/badge/License-MIT-B0C4DE?style=for-the-badge)](#)

*Building end-to-end customer lifetime value estimation, behavioural churn explainability, and unit-economic retention engines with style.* ✨

</div>

---

## 💅🏽 Overview

Let's be real: keeping existing customers around and growing their value is *so* much easier and cheaper than constantly chasing new ones! 🌸 Most basic customer tools just hand you a random historical report without explaining **why** a customer's spending is dropping or **whether it's actually worth spending money** on targeted perks to keep them in your top tiers.

We built **CohortX** to fix that exact problem with a super smooth, step-by-step workflow:

1. **Spot High-Value & At-Risk Customers:** Runs clean SQL data pipelines and XGBoost to classify customers into 90-day future value bands (*Soft Churn / At-Risk*, *Moderate Value*, *High Value / VIP*).
2. **Uncover the "Why":** Uses TreeSHAP values so teams can see the exact behavioral features pushing a customer into each tier without real-time latency bottlenecks.
3. **Do the Math on Customer LTV:** Balances Customer Lifetime Value ($LTV$), acquisition costs, and special incentives so you only spend money saving or upgrading customers when it actually makes financial sense.
4. **Test "What-If" Scenarios:** Gives stakeholders a cute, interactive dashboard with sliders to test out different retention offers and parameter tweaks in real time.

---

## 🦄 Module Architecture & Responsibilities

The codebase follows a modular design split into six domain-specific directories:

| Directory | Core Focus | Module Lead | Primary Responsibilities |
| :--- | :--- | :--- | :--- |
| `core_warehouse/` | Data Engineering | **Malak Mostafa** | Relational schemas, SQLite metadata management, DuckDB batch OLAP queries, and SQL transformations. |
| `feature_factory/` | Data Prep & EDA | **Menna Ahmed** | Strict cut-off temporal splitting, cancellation processing, RFM feature extraction, and pipeline scaling. |
| `intelligence_core/` | Predictive Modeling | **Fatema Sadik** | Multi-class XGBoost/LightGBM tuning via Optuna, cost-matrix optimization, and batch TreeSHAP explainer generation. |
| `serving_layer/` | MLOps & APIs | **Rabab Mohamed** | MLflow model & scaler registry, Docker containerization, and low-latency REST API deployment via FastAPI. |
| `control_center/` | Business Intelligence | **Youssef El-Kholy** | Streamlit multi-page interface, customer lookup views, and interactive unit-economic scenario simulators. |
| `quality_assurance/` | QA & Testing | **Farah Nasser** | Pydantic data contract validation, Pytest suites, continuous integration workflows, and linting routines. |

<div align="center">
  Built with grit, coffee, and teamwork by 5 girls and 1 guy for our DEPI Capstone!🥳
</div>

---

## 💄 Technology Stack

- **Database & Storage:** SQLite, DuckDB
- **Data Wrangling & ML:** Python, Pandas, Scikit-learn, XGBoost, LightGBM, Optuna, SHAP
- **Serving & MLOps:** FastAPI, Uvicorn, MLflow, Docker, Docker Compose
- **Interface & Visualization:** Streamlit, Plotly
- **Testing & Quality Assurance:** Pytest, Pydantic, GitHub Actions

---

## 🎀 Getting Started

### 🛠️ Environment Setup

Confirm Python 3.10 or higher is installed before configuring your local environment:

```bash
# Clone the repository and navigate to the project root
git clone https://github.com/fatemasadik07/CohortX
cd CohortX

# Initialize the virtual environment
python -m venv venv

# Activate on Windows (Command Prompt):
venv\Scripts\activate

# Activate on macOS/Linux:
source venv/bin/activate

# Upgrade pip & install required dependencies
pip install --upgrade pip
pip install -r requirements.txt
```

> **Note:** Ensure your raw transaction dataset (e.g., `online_retail.csv`) is placed inside `data/raw/` before running data initialization! ✨

---

### 🚀 How to Run the System

You can run services through Docker Compose or execute pipeline modules individually:

#### Option A: Docker Compose (Recommended) 🐳

Launch the FastAPI serving layer, Streamlit control center, and MLflow tracking server simultaneously:

```bash
docker-compose up --build
```

#### Option B: Step-by-Step Module Execution 🛠️

##### 1. Ingest Data & Initialize Database
Initialize database schemas and seed raw transaction datasets into SQLite/DuckDB:
```bash
python -m core_warehouse.connectors.db_session
```

##### 2. Run Preprocessing & Feature Engineering
Clean raw data, process cancellations, apply temporal cutoffs, and generate RFM features:
```bash
python -m feature_factory.pipelines.cleaning
```

##### 3. Train Models & Generate SHAP Explanations
Train the multi-class classifier using Optuna hyperparameter optimization and generate pre-computed TreeSHAP values:
```bash
python -m intelligence_core.trainers.train_xgboost
```

##### 4. Start the FastAPI Serving Layer
Launch the REST API locally:
```bash
uvicorn serving_layer.app.main:app --reload --port 8000
```
Access interactive OpenAPI documentation at [http://localhost:8000/docs](http://localhost:8000/docs) 💌.

##### 5. Launch the Streamlit Dashboard
Start the multi-page control center for customer lookups and scenario simulation:
```bash
streamlit run control_center/app.py
```
The application will automatically open at [http://localhost:8501](http://localhost:8501) 🌸.

##### 6. Run Quality Assurance & Test Suite
Execute integration tests and validate data contracts:
```bash
pytest quality_assurance/suite/
```

---

## 📚 DEPI Documentation

Project planning, specification, and evaluation records are located in the `.depi/` directory:

| Document | Description |
| :--- | :--- |
| `01_PROJECT_PLANNING.md` | Project Scope, Milestones, and Timelines |
| `02_REQUIREMENTS_SPECS.md` | System Specifications and Functional Requirements |
| `03_LITERATURE_REVIEW.md` | Literature Review and Methodological Background |

<p align="center">
  <i>Developed with ✨ for the Digital Egypt Pioneers Initiative (DEPI) Capstone Project.</i>
</p>
