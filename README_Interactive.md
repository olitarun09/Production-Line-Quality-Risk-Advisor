# 🏭 Production Line Quality Risk Advisor (PLQRA)

<p align="center">
  <strong>From machine-failure prediction to smarter inspection decisions.</strong><br>
  A machine-learning decision-support framework for manufacturing quality engineers.
</p>

<p align="center">
  <a href="#-project-overview"><img alt="Python" src="https://img.shields.io/badge/Python-3.9+-3776AB?logo=python&logoColor=white"></a>
  <a href="#-machine-learning-approach"><img alt="Scikit-learn" src="https://img.shields.io/badge/ML-Scikit--learn-F7931E?logo=scikitlearn&logoColor=white"></a>
  <a href="#-interactive-dashboard--prototype"><img alt="Frontend" src="https://img.shields.io/badge/UI-Tailwind%20%2B%20HTML5-06B6D4?logo=tailwindcss&logoColor=white"></a>
  <a href="#-dataset"><img alt="Dataset" src="https://img.shields.io/badge/Dataset-AI4I%202020-0A7B83"></a>
  <img alt="Status" src="https://img.shields.io/badge/Status-Prototype%20Active-success">
</p>

> [!IMPORTANT]
> This project is designed for **decision support**, helping manufacturing teams decide **which production observations to inspect first** when inspection capacity is strictly limited. Any synthetic demo scores are illustrative and intended to support human quality inspection decisions.

---

## 🧭 Quick Navigation

- [📋 Project Overview](#-project-overview)
- [⚠️ The Problem](#️-the-problem)
- [💡 Proposed Solution](#-proposed-solution)
- [🎯 Why Precision@K Matters](#-why-precisionk-matters)
- [🔄 Interactive Workflow Diagram](#-interactive-workflow-diagram)
- [🖥️ Interactive Dashboard & Prototype](#️-interactive-dashboard--prototype)
- [🤖 Machine-Learning Approach](#-machine-learning-approach)
- [🛡️ Preventing Target Leakage](#️-preventing-target-leakage)
- [📊 Dataset](#-dataset)
- [📁 Repository Structure](#-repository-structure)
- [🛠️ Technology Stack](#️-technology-stack)
- [⚙️ Setup and Usage](#️-setup-and-usage)
- [✅ Development Checklist](#-development-checklist)
- [⚠️ Scope and Limitations](#️-scope-and-limitations)
- [🚀 Future Improvements](#-future-improvements)

---

## 📋 Project Overview

The **Production Line Quality Risk Advisor** is a decision-support prototype built to bridge the gap between abstract machine-failure predictions and operational quality control in manufacturing plants.

Modern manufacturing lines generate high volumes of telemetry: air temperature, process temperature, rotational speed, torque, and tool wear. While traditional classifiers simply output a binary *failure / no-failure* label, physical inspection teams operate under rigid capacity constraints (e.g., inspecting only **K** units per shift). PLQRA ranks equipment runs by estimated failure risk, ensuring inspectors allocate their finite time where risk is highest.

### At a Glance

| Item | Details |
|---|---|
| **Primary User** | Manufacturing Quality Engineer / QC Supervisor |
| **Dataset** | AI4I 2020 Predictive Maintenance Dataset (10,000 records) |
| **ML Task** | Binary classification with risk ranking |
| **Decision Focus** | "If we can inspect only $ items, which should we inspect first?" |
| **Core Metric** | **Precision@K** alongside standard F1, Precision, and Recall |
| **UI Experience** | High-performance interactive dashboard with 3D tilt, telemetry simulation, and risk distribution |

---

## ⚠️ The Problem

In a busy production facility:
1. **Inspection Bottlenecks:** Quality teams cannot physically tear down or run deep non-destructive tests on 100% of manufactured units.
2. **Extreme Class Imbalance:** Normal machine operations far outnumber failure incidents (>96% non-failure), making raw accuracy misleading.
3. **Actionability Gap:** Standard classifiers output a label without ranking. If a model predicts 200 items could fail but the shift capacity is only 25 inspections, which 25 should be prioritized?

---

## 💡 Proposed Solution

PLQRA implements a 5-stage decision-support pipeline:
1. **Telemetry Ingestion:** Ingest and preprocess factory sensor streams.
2. **Leakage-Safe Feature Engineering:** Strip out retrospective failure flags to avoid target leakage.
3. **Probabilistic Risk Scoring:** Estimate calibrated risk probabilities using evaluated ML models.
4. **Capacity-Constrained Prioritization:** Sort items into a ranked queue and evaluate **Precision@K**.
5. **Interactive Operator Dashboard:** Present clear risk queues, feature attributions, and threshold controls to quality engineers.

---

## 🎯 Why Precision@K Matters

**Precision@K** measures the proportion of actual failure cases within the top **K** highest-risk observations:

\text{Precision@K} = \frac{\text{Actual positive cases among top } K}{K}

### Example Scenario
Suppose inspection capacity allows checking **25 units** during a shift ( = 25$). If **18** of those top-ranked units are confirmed defect/failure cases:

\text{Precision@25} = \frac{18}{25} = 0.72 \quad (72\%)

This metric directly reflects real operational yield and efficiency under tight inspection budgets.

---

## 🔄 Interactive Workflow Diagram

`mermaid
flowchart TD
    A["AI4I 2020 Dataset"] --> B["Data Ingestion & Cleaning"]
    B --> C["EDA & Feature Engineering"]
    C --> D["Leakage-Aware Split & Preprocessing"]
    D --> E["Model Training & Cross-Validation"]
    E --> F["Compare Candidates (RF, AdaBoost, Logistic, SVM)"]
    F --> G["Calibrate Risk Probabilities"]
    G --> H["Rank Queue by Estimated Risk"]
    H --> I["Evaluate Precision@K (Capacity K)"]
    I --> J["Feature Explanations & Inspection Queue"]
    J --> K["Interactive Advisor Dashboard (UI)"]
    K --> L["Quality Engineer Inspection Decision"]

    classDef data fill:#123047,stroke:#38bdf8,color:#ffffff
    classDef model fill:#25204b,stroke:#a78bfa,color:#ffffff
    classDef action fill:#123b32,stroke:#34d399,color:#ffffff
    class A,B,C,D data
    class E,F,G,H,I model
    class J,K,L action
`

---

## 🖥️ Interactive Dashboard & Prototype

The repository includes a modern, high-aesthetic web dashboard (index.html / Production_Line_Quality_Risk_Advisor.html):
- **Live Demo Modal:** Test real-time inspection capacity sliders and risk thresholds.
- **Micro-Animations & 3D Tilt:** Visual feedback on telemetry cards and factory status indicators.
- **Dynamic Risk Gauge:** Visual breakdown of High, Medium, and Low risk production observations.

### How to Run Locally

You can run the interactive prototype using Python's built-in HTTP server:

`powershell
# From the repository root
python -m http.server 8080
`
Open **[http://localhost:8080/index.html](http://localhost:8080/index.html)** in any browser.

---

## 🤖 Machine-Learning Approach

Candidate models evaluated for decision support:

| Model | Rationale |
|---|---|
| **Logistic Regression** | Linear baseline with probabilistic calibration and L1/L2 regularization |
| **Decision Tree** | Rule-based interpretable trees for fast inspection explanation |
| **Random Forest** | Ensemble modeling capturing nonlinear sensor interactions and feature importance |
| **AdaBoost** | Sequential boosting focusing on difficult borderline failure states |
| **Support Vector Machine (SVM)** | Robust boundary classification with RBF kernels |

---

## 🛡️ Preventing Target Leakage

The AI4I dataset contains specific failure breakdown flags (TWF, HDF, PWF, OSF, RNF) alongside the main Machine failure column.
- **Critical Guard:** These indicators are retrospective descriptions of the failure itself.
- **Rule:** They must **never** be used as input predictors when forecasting risk from operational telemetry.

---

## 📊 Dataset

The project uses the **AI4I 2020 Predictive Maintenance Dataset** (stored in [data/ai4i2020.csv](data/ai4i2020.csv)):
- **Volume:** 10,000 production records
- **Features:** Product Type (L, M, H), Air temperature [K], Process temperature [K], Rotational speed [rpm], Torque [Nm], Tool wear [min]
- **Target:** Machine failure (0 = Normal, 1 = Failure)
- **Source:** [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/601/ai4i+2020+predictive+maintenance+dataset)

---

## 📁 Repository Structure

`	ext
├── data/
│   └── ai4i2020.csv              # AI4I 2020 Predictive Maintenance dataset (10,000 rows)
├── docs/
│   ├── Production_Line_Quality_Risk_Advisor_Overview.pdf
│   ├── SRS_Production_Line_Quality_Risk_Advisor_FINAL.docx
│   └── The-Problem-Identifying-Production-Risk-Early.pptx
├── index.html                    # Interactive Dashboard (Root entry point for GitHub Pages)
├── Production_Line_Quality_Risk_Advisor.html # Standalone interactive dashboard file
├── requirements.txt              # Project Python dependencies
├── .gitignore                    # Python & IDE exclusion rules
└── README.md                     # Comprehensive project documentation
`

---

## 🛠️ Technology Stack

| Technology | Category | Role |
|---|---|---|
| **Python 3.9+** | Core Language | Data modeling and back-end logic |
| **pandas & NumPy** | Data Wrangling | Telemetry transformation and array operations |
| **scikit-learn** | Machine Learning | Classification pipelines, validation, and metrics |
| **Plotly & Seaborn** | Data Visualization | Correlation matrices and distribution plots |
| **Streamlit** | Planned Dashboard | Python-based interactive analytical application |
| **HTML5 & TailwindCSS** | Web Prototype | Modern interactive dashboard UI with micro-animations |

---

## ⚙️ Setup and Usage

### 1. Clone the Repository
`ash
git clone https://github.com/olitarun09/Production-Line-Quality-Risk-Advisor.git
cd Production-Line-Quality-Risk-Advisor
`

### 2. Create and Activate Virtual Environment
`powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
`

### 3. Install Dependencies
`powershell
pip install -r requirements.txt
`

---

## ✅ Development Checklist

- [x] Repository initialized and connected to GitHub
- [x] Interactive web dashboard implemented (index.html)
- [x] AI4I 2020 dataset added to [data/](data/)
- [x] Project specifications and presentation materials added to [docs/](docs/)
- [x] Comprehensive documentation and architecture diagrams in README.md
- [ ] Exploratory Data Analysis (EDA) notebook
- [ ] Baseline classification models and cross-validation suite
- [ ] Precision@K ranking evaluation script
- [ ] Streamlit analytics application integration

---

## ⚠️ Scope and Limitations

- **Academic Prototype:** Trained and evaluated using synthetic research datasets; results require field validation before actual factory deployment.
- **Human-in-the-Loop:** Designed strictly for decision support and inspection prioritization, not autonomous machine shutdown.

---

## 🚀 Future Improvements

- Dynamic API connectors for real-time SCADA/PLC telemetry streams.
- Automated SHAP/LIME explanation cards per ranked observation.
- Shift-level inspection capacity optimizer with cost-sensitive loss weighting.

---

**Developed by Tarun Oli ([@olitarun09](https://github.com/olitarun09))**
