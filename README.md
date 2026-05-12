# ZeroWaste-Health ♻️🏥

> **Medicines expire. People go without. This pipeline bridges that gap.**

A production-grade data engineering pipeline that intercepts near-expiry medical supplies from retail pharmacy inventory and intelligently routes them to underserved communities — before they hit the landfill.

---

## 🔍 The Problem

Every year in the United States alone:
- **$5 billion+** worth of usable medicine goes to waste
- **1 in 4 adults** struggle to afford prescription drugs
- Basic supplies — bandages, OTC medicines, IV fluids, supplements — expire on retail shelves while low-income families, students, and emergency patients go without
- Expired inventory ends up in **landfills**, contributing to environmental damage

The gap between surplus and need is not a supply problem. **It's a data problem.**

---

## 💡 The Solution

ZeroWaste-Health is a data engineering platform that:

1. **Ingests** retail pharmacy inventory data and tracks expiry windows in real time
2. **Scores** inventory by expiry urgency, quantity, and supply category
3. **Matches** surplus supply to underserved demand zones using geographic and demographic data
4. **Routes** redistribution recommendations to community partners, NGOs, and direct recipients
5. **Surfaces** actionable insights through a live Streamlit dashboard

---

## 🏗️ Architecture

```
Raw Inventory Data (Synthetic)
        │
        ▼
┌─────────────────────┐
│   Ingestion Layer   │  ← Python + PySpark
│  inventory_ingestion│
│  demand_ingestion   │
│  expiry_scanner     │
└────────┬────────────┘
         │
         ▼
┌─────────────────────┐
│  Transform Layer    │  ← PySpark + DuckDB
│  expiry_scoring     │
│  demand_matching    │
│  redistribution_    │
│  router             │
└────────┬────────────┘
         │
         ▼
┌─────────────────────┐
│  Data Quality Layer │  ← pytest + custom checks
│  null_checks        │
│  schema_validation  │
│  reconciliation     │
│  freshness_checks   │
└────────┬────────────┘
         │
         ▼
┌─────────────────────┐
│  Curated Layer      │  ← DuckDB (local) / Snowflake (cloud)
│  redistribution     │
│  recommendations    │
│  demand zones       │
│  expiry risk scores │
└────────┬────────────┘
         │
         ▼
┌─────────────────────┐
│  Orchestration      │  ← Apache Airflow
│  zerowaste_dag      │
└────────┬────────────┘
         │
         ▼
┌─────────────────────┐
│  Dashboard          │  ← Streamlit
│  Live inventory map │
│  Expiry risk alerts │
│  Redistribution KPIs│
└─────────────────────┘
```

---

## 📦 Tech Stack

| Layer | Technology | Cost |
|---|---|---|
| Data Processing | Python, PySpark | Free |
| Data Warehouse | DuckDB (local) | Free |
| Orchestration | Apache Airflow (Docker) | Free |
| Dashboard | Streamlit | Free |
| Containerization | Docker | Free |
| Testing | pytest | Free |
| Version Control | GitHub | Free |
| Cloud (optional) | AWS Free Tier (S3, Glue, Athena) | Free tier |

**Total infrastructure cost: $0**

---

## 📁 Project Structure

```
ZeroWaste-Health/
│
├── synthetic_data/                 # Synthetic data generators
│   ├── generate_inventory.py       # Fake pharmacy inventory data
│   └── generate_demand.py          # Fake community demand data
│
├── ingestion/                      # Raw data ingestion
│   ├── inventory_ingestion.py      # Ingest pharmacy inventory
│   ├── demand_ingestion.py         # Ingest community demand signals
│   └── expiry_scanner.py           # Scan and flag near-expiry items
│
├── transform/                      # Data transformation logic
│   ├── expiry_scoring.py           # Score inventory by expiry urgency
│   ├── demand_matching.py          # Match supply to demand zones
│   └── redistribution_router.py    # Generate routing recommendations
│
├── quality/                        # Data quality controls
│   ├── null_checks.py              # Null and missing value checks
│   ├── schema_validation.py        # Schema drift detection
│   ├── reconciliation.py           # Source to target reconciliation
│   └── freshness_checks.py         # Data freshness monitoring
│
├── models/                         # ML models
│   ├── expiry_risk_model.py        # Predict expiry risk window
│   └── demand_forecast_model.py    # Forecast community demand by zone
│
├── orchestration/                  # Airflow DAGs
│   └── zerowaste_dag.py            # Main pipeline DAG
│
├── dashboard/                      # Streamlit dashboard
│   ├── app.py                      # Main dashboard app
│   └── utils.py                    # Dashboard helper functions
│
├── data/                           # Data lake zones
│   ├── raw/                        # Raw ingested data
│   ├── processed/                  # Transformed data
│   └── curated/                    # Final trusted datasets
│
├── tests/                          # Test suite
│   ├── unit/                       # Unit tests
│   └── integration/                # Integration tests
│
├── docs/                           # Documentation
│   ├── architecture.md             # Architecture deep dive
│   └── data_dictionary.md          # Field definitions
│
├── config/                         # Configuration
│   ├── config.yaml                 # Pipeline configuration
│   └── logging.yaml                # Logging configuration
│
├── logs/                           # Pipeline logs
├── docker-compose.yml              # Docker setup for Airflow
├── requirements.txt                # Python dependencies
└── README.md                       # You are here
```

---

## 🗃️ Data Model

### Inventory Table (raw → processed)
| Field | Type | Description |
|---|---|---|
| inventory_id | STRING | Unique inventory record ID |
| pharmacy_id | STRING | Source pharmacy identifier |
| product_name | STRING | Medicine or supply name |
| category | STRING | RX, OTC, Bandage, IV Fluid, Supplement |
| quantity | INTEGER | Units available |
| expiry_date | DATE | Product expiry date |
| days_to_expiry | INTEGER | Calculated days remaining |
| zip_code | STRING | Pharmacy location |
| ingestion_timestamp | TIMESTAMP | When record was ingested |

### Demand Table (raw → processed)
| Field | Type | Description |
|---|---|---|
| demand_id | STRING | Unique demand record ID |
| zone_id | STRING | Community demand zone |
| zip_code | STRING | Demand location |
| product_category | STRING | Category needed |
| urgency_level | STRING | HIGH / MEDIUM / LOW |
| estimated_beneficiaries | INTEGER | People who would benefit |
| source | STRING | NGO, Clinic, School, Gov |
| request_timestamp | TIMESTAMP | When demand was logged |

### Redistribution Recommendations Table (curated)
| Field | Type | Description |
|---|---|---|
| recommendation_id | STRING | Unique recommendation ID |
| inventory_id | STRING | Matched inventory record |
| demand_id | STRING | Matched demand record |
| match_score | FLOAT | Supply-demand match score 0-1 |
| expiry_urgency_score | FLOAT | Urgency based on days to expiry |
| distance_km | FLOAT | Distance from pharmacy to demand zone |
| recommended_action | STRING | DISPATCH / HOLD / ALERT |
| generated_timestamp | TIMESTAMP | When recommendation was created |

---

## 🚀 Getting Started

### Prerequisites
- Python 3.9+
- Docker Desktop (for Airflow)
- Git

### Installation

```bash
# Clone the repo
git clone https://github.com/yourusername/ZeroWaste-Health.git
cd ZeroWaste-Health

# Create virtual environment
python -m venv venv
venv\Scripts\activate  # Windows

# Install dependencies
pip install -r requirements.txt

# Generate synthetic data
python synthetic_data/generate_inventory.py
python synthetic_data/generate_demand.py

# Run the pipeline
python ingestion/inventory_ingestion.py

# Launch dashboard
streamlit run dashboard/app.py
```

### Run with Docker (Airflow)
```bash
docker-compose up -d
# Visit http://localhost:8080
# Username: admin | Password: admin
```

---

## 📊 Dashboard Features

- 🗺️ **Live Inventory Map** — near-expiry supplies by zip code
- 🚨 **Expiry Risk Alerts** — items expiring within 7, 14, 30 days
- 📦 **Redistribution Queue** — ranked recommendations ready to dispatch
- 📈 **KPI Metrics** — units saved, communities served, waste prevented
- 🔍 **Demand Heatmap** — underserved zones by category and urgency

---

## 🧪 Data Quality Checks

| Check | Description |
|---|---|
| Null Check | Flags missing expiry dates, quantities, zip codes |
| Schema Validation | Detects schema drift between pipeline runs |
| Reconciliation | Source vs target row count comparison |
| Freshness Check | Alerts if data has not updated within SLA window |
| Duplicate Detection | Removes duplicate inventory records |

---

## 🌱 Business Impact

| Metric | Target |
|---|---|
| Units intercepted before expiry | 10,000+ per month (simulated) |
| Communities served | 50+ demand zones |
| Waste reduction | 80% of near-expiry inventory redirected |
| Pipeline SLA | < 1 hour from ingestion to recommendation |
| Data quality pass rate | > 95% |

---

## 🔮 Future Roadmap

- [ ] Direct-access mobile app for recipients to request supplies
- [ ] Upstream manufacturer data integration for earlier expiry forecasting
- [ ] AI-powered demand prediction using zip code demographic data
- [ ] SMS/email alert system for community partners
- [ ] Real pharmacy API integration (with consent)
- [ ] Multilingual support for underserved communities

---

## 🤝 Inspiration

This project was independently designed after researching the gap between near-expiry retail medical inventory and underserved community access. Organizations like [SIRUM](https://sirum.org) and [MyTablet](https://mytablet.org) are doing incredible work in this space. ZeroWaste-Health is a data engineering exploration of how technology and automation can help scale solutions like these.

---

## 👩‍💻 Author

**Rishita Reddy**
Master's in Business Analytics — University of Texas at Dallas
5+ years | Data Engineer | Healthcare & Banking
📧 rrmaryada@gmail.com

---

## 📄 License

MIT License — free to use, adapt, and build on.

---

*Medicines expire. People go without. Data can bridge that gap.*
