# FinOps Toolkit

> A practical, production-minded collection of tools, notebooks, and dashboards for cloud cost management. Built to demonstrate FinOps practices: visibility, optimization, governance, forecasting, and collaboration across Azure, AWS, and GCP.

---

## 🚀 Project Overview

**FinOps-Toolkit** is an opinionated, end-to-end repository that helps teams adopt FinOps practices using code, automation, and stakeholder-ready artifacts. It contains example datasets, production-like automation scripts, exploratory notebooks, and dashboard templates so you can demonstrate real-world FinOps workflows to engineering, finance, and executive stakeholders.

This repo is ideal for interview demos, internal POCs, or bootstrapping a production FinOps pipeline.

---

## 📌 Key Features

* **Cost Analysis**: multi-cloud sample datasets and notebooks that surface top cost drivers and trends.
* **Tagging Governance**: automated checks for required tags (`owner`, `project`, `environment`) and reports for remediation.
* **Anomaly Detection**: simple statistical detectors and notebook-driven visual validation of spikes.
* **Rightsizing Recommender**: identify underutilized compute/storage with actionable recommendations.
* **Forecasting**: simple, explainable time-series forecasting (ARIMA) and guidance to plug in Prophet or ML models.
* **Stakeholder Artifacts**: Power BI/Tableau templates, sample reports (PDF/XLSX), and slide recommendations for exec briefings.
* **Production Friendly**: examples show packaging, requirements, test hooks, and CI patterns (GitHub Actions).

---

## 📁 Repository Structure

```
FinOps-Toolkit/
├── README.md
├── requirements.txt
├── LICENSE
├── data/
│   ├── azure_costs.csv
│   ├── aws_costs.csv
│   └── gcp_costs.csv
├── notebooks/
│   ├── 01_cost_exploration.ipynb
│   ├── 02_anomaly_detection.ipynb
│   └── 03_forecasting.ipynb
├── scripts/
│   ├── tagging_policy_checker.py
│   ├── anomaly_alert.py
│   ├── rightsizing_recommender.py
│   └── budget_forecast.py
├── dashboards/
│   ├── powerbi/FinOpsDashboard.pbix
│   ├── tableau/FinOpsDashboard.twb
│   └── screenshots/
├── reports/
│   ├── Sample_FinOps_Report.pdf
│   └── Savings_Plan_Analysis.xlsx
└── tests/
    ├── test_tagging.py
    ├── test_anomaly.py
    └── test_forecasting.py
```

---

## 📥 Sample Data Schema

> The example CSVs (`data/*.csv`) use a simple, analysis-friendly schema. Real billing datasets from cloud providers are richer — treat these as simplified, normalized examples used by the scripts and notebooks.

| Column                         |           Type | Description                                  |
| ------------------------------ | -------------: | -------------------------------------------- |
| date                           |   `YYYY-MM-DD` | Billing date (string or ISO timestamp)       |
| account\_id / subscription\_id |         string | Cloud account identifier                     |
| service                        |         string | Cloud service name (e.g., EC2, Blob Storage) |
| resource\_id                   |         string | Unique resource identifier                   |
| cost                           |          float | Cost for the row's time period (USD)         |
| cpu\_utilization               | float \[0-100] | CPU utilization (percent, optional)          |
| memory\_utilization            | float \[0-100] | Memory utilization (optional)                |
| owner                          |         string | Tag identifying resource owner (optional)    |
| project                        |         string | Tag identifying project/product (optional)   |
| environment                    |         string | Tag: `prod` / `staging` / `dev` (optional)   |

---

## 🛠 Installation & Quickstart

> Recommended: run in a Python virtual environment (venv, conda) or inside an isolated container.

```bash
# clone repo
git clone https://github.com/<your-username>/FinOps-Toolkit.git
cd FinOps-Toolkit

# create venv (optional)
python -m venv .venv
source .venv/bin/activate  # mac/linux (or .venv\Scripts\activate on Windows)

# install
pip install -r requirements.txt
```

### Quick run examples

```bash
# run tagging policy check
python scripts/tagging_policy_checker.py --input data/azure_costs.csv --required owner project environment

# run anomaly detector
python scripts/anomaly_alert.py --input data/aws_costs.csv --column cost --z_threshold 2.5

# get rightsizing recommendations
python scripts/rightsizing_recommender.py --input data/gcp_costs.csv --cpu-threshold 20 --mem-threshold 30

# produce budget forecast (next N periods)
python scripts/budget_forecast.py --input data/azure_costs.csv --periods 6
```

> All scripts accept `--help` for CLI usage. (Examples in `scripts/` include `argparse` patterns.)

---

## 🧭 Scripts (what they do)

### `scripts/tagging_policy_checker.py`

* Loads a CSV of resources and billing data.
* Validates presence of required tags (`owner`, `project`, `environment`).
* Outputs a summary report and a CSV of non-compliant resources for remediation.

**Usage pattern**: governance/ops teams can run nightly/weekly checks and open tickets for owners missing tags.

### `scripts/anomaly_alert.py`

* Detects cost anomalies using a configurable statistical method (Z-score by default).
* Outputs anomalous rows with context (service, date, cost) and creates a CSV/JSON file for alerts.
* Notebook `02_anomaly_detection.ipynb` visualizes the anomalies on a time-series plot.

**Extension ideas**: replace or augment with Prophet/LOESS, seasonal decomposition, or ML-based detectors (isolation forest).

### `scripts/rightsizing_recommender.py`

* Uses utilization metrics (CPU, memory) to identify underutilized compute instances.
* Outputs recommended new instance types or a `downsize` suggestion and an estimated monthly saving.
* Includes safe-guards: flag resources with recent scale events or noisy metrics to avoid false positives.

**Production tip**: combine with historical utilization windows (7/30/90 days) and exclude bursting workloads.

### `scripts/budget_forecast.py`

* Small demo using ARIMA (statsmodels). Takes a timeseries of daily/monthly costs and forecasts future spend.
* Notebooks show how to replace ARIMA with Prophet or simple exponential smoothing.

**Business use**: run forecasts weekly to update finance budgets and provide early notices for projected overspend.

---

## 📓 Notebooks (for demos & stakeholder conversations)

* **01\_cost\_exploration.ipynb**: walk-through of cleaning billing CSVs, top-k service identification, and monthly trend visuals. Designed as the first demo when you onboard stakeholders.
* **02\_anomaly\_detection.ipynb**: shows anomaly detection, visual validation, and how to translate anomalies into action items (e.g., investigate spike source).
* **03\_forecasting.ipynb**: trains a small ARIMA model, plots horizon forecasts, and includes a short section on how to integrate Prophet and how to interpret confidence intervals.

Each notebook is annotated with talking points to present to non-technical stakeholders.

---

## 📊 Dashboards & Stakeholder Deliverables

* **Power BI / Tableau templates** (stored in `dashboards/`) provide pre-built visuals:

  * Monthly spend trend, top services, top projects, savings opportunities, and anomaly timeline.
  * A single-page executive summary that shows: 1) Top 3 cost drivers, 2) Optimization opportunities (USD), 3) Forecast vs Actual.

* **reports/** contains example executive PDFs and spreadsheets. These are ready to attach to stakeholder emails or embed in monthly review slides.

**Tip**: When presenting to the CFO/CTO, always include the dollar impact and suggested next steps — not just the raw metric.

---

## 📈 FinOps KPIs & How to Present Them

* **Total Cloud Spend (MTD / YTD)** — show trend and month-on-month % change.
* **Spend by Product / Team** — heatmap or stacked bar.
* **Cost per Unit Metric** — e.g., cost per transaction, cost per user, cost per model inference.
* **Forecast Accuracy** — MAPE/RMSE of forecasting model.
* **Optimization Potential** — estimated annual savings from rightsizing or reserved instances.
* **Tag Coverage** — % of resources with required tags.
* **Anomalies Count & Impact** — number of anomalies and cumulative unexpected spend.

When reporting, translate technical findings into business outcomes (e.g., "Rightsizing X yields \$Y/year — enough to fund Z features").

---

## 🔧 CI / CD, Testing, and Productionization

* **Tests**: `tests/` includes pytest examples for key script behaviors (tag checking, anomaly detection thresholds, forecasting pipeline). Run with:

```bash
pytest -q
```

* **GitHub Actions**: include CI to run unit tests, linting (flake8/black), and notebook execution (nbconvert or papermill) for regression checks.

* **Packaging**: `setup.cfg` / `pyproject.toml` guidance to package helpers into a pip-installable tool for internal use.

* **Scheduling**: schedule scripts via GitHub Actions, Airflow, or cloud-native schedulers (Azure Functions, AWS Lambda, GCP Cloud Scheduler) depending on scale.

---

## 🔐 Security & Data Privacy

* Example CSVs are synthetic. When connecting to real cloud billing/APIs: ensure least-privileged credentials, rotate keys, and do not commit secrets.
* Use secret managers (Azure Key Vault, AWS Secrets Manager) for production credentials.
* Mask PII in reports and adhere to your organization’s data governance policies.

---

## 🤝 Contributing

Contributions are welcome — you can add new detectors, better forecasting, cloud connectors, or visualizations.

* Fork the repo
* Create a feature branch
* Run tests and linters
* Open a PR with a clear description and sample outputs

---

## 🗺 Roadmap / Next Steps

1. Cloud-native connectors: integrate with Azure Cost Management API, AWS Cost & Usage Reports, and GCP Billing export.
2. Advanced anomaly detection: seasonal decomposition and ML-based detectors.
3. Auto-remediation playbooks: safe autoscaling or cleanup flows triggered after human approval.
4. Multi-tenant dashboards and RBAC for stakeholders.

---

## 📜 License

This project is available under the MIT License. See `LICENSE` for details.

---

## ✉️ Contact / Demo Request

If you want a tailored demo for your company or an interview walkthrough, open an issue in this repo or email: `your.email@example.com` (replace with your contact info).

---

*Prepared as a candidate-grade FinOps showcase: production-minded, stakeholder-ready, and interview-focused.*
