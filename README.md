# ZeroWaste-Health ♻️🏥

> **Predict surplus before it expires. Verify real need. Route healthcare supplies where they create the most impact.**

ZeroWaste-Health is a production-style data engineering and analytics platform that forecasts potential healthcare inventory waste from manufacturers, hospitals, pharmacies, and clinics, estimates available surplus before expiry, verifies eligible demand from underserved users and partner organizations, and generates geography-aware redistribution recommendations.

This project is designed as a data engineering solution for reducing medical supply waste while improving access for approved recipient groups such as students,vulnerable populations, low-income families, clinics, and NGO/community partners.

> **Note:** This project is a simulated data engineering portfolio system. Any real-world redistribution of prescription medication or regulated medical supplies would require licensed partners, compliance approval, identity verification, privacy controls, and applicable federal/state regulatory review.

---

## 🔍 The Problem

Healthcare inventory waste happens across multiple points in the supply chain:

- Manufacturers may overproduce or misforecast product demand, leaving unsold inventory at risk of expiry.
- Hospitals, pharmacies, and clinics often hold supplies that remain unused until they approach expiry.
- Basic health supplies such as bandages, OTC medicines, supplements, first-aid items, and approved non-controlled  medical supplies may go unused while underserved communities lack affordable access.
- Students,vulnerable populations, low-income families, clinics, and emergency support groups may need these supplies, but demand is often not captured in a structured way.
- Redistribution is difficult because supply, demand, eligibility, geography, and expiry timelines are stored across disconnected systems.

The gap is not only an inventory problem. **It is a forecasting, eligibility, logistics, and data integration problem.**

---

## 💡 Project Goal

The goal of ZeroWaste-Health is to build a data platform that can:

1. **Collect inventory and sales data from manufacturers** to forecast future sales, unsold quantity, and expected waste risk.
2. **Ingest supply data from hospitals, pharmacies, and clinics** to identify items that are regularly left unused and likely to expire.
3. **Predict available surplus quantity** by combining manufacturer forecasts, pharmacy inventory, hospital/clinic stock, historical sales, expiry windows, and demand trends.
4. **Build an access platform for eligible recipients** such as students, government school children, below-middle-income families, clinics, and NGO partners.
5. **Capture eligibility information during signup** including student email, student ID, graduation year, government program information, age group, income range, employment proof category, and household/lifestyle indicators where applicable.
6. **Allow NGOs and community partners to request access** so they can help distribute supplies to verified recipient groups.
7. **Use geographic and demographic data** to plan deliveries, prioritize nearby supply-demand matches, and reduce redistribution delays.
8. **Generate redistribution recommendations** that balance expiry urgency, quantity available, eligibility demand, distance, supply category, and delivery priority.
9. **Leverage eligibility, income, demand, and geographic insights** to design financial assistance and subsidized distribution programs for verified underserved recipients, helping prioritize free or reduced-cost access based on need, supply availability, and delivery feasibility.

---

## 🧠 Core Business Logic

ZeroWaste-Health is built around five major business questions:

1. **How much healthcare inventory will likely remain unsold or unused?**
   - Forecast manufacturer sales and remaining inventory.
   - Estimate pharmacy, hospital, and clinic surplus.
   - Identify quantity at risk before expiry.

2. **Which products are safe and eligible for redistribution?**
   - Filter by category, expiry window, quantity, product restrictions, and compliance eligibility.
   - Separate OTC/non-controlled supplies from restricted or prescription-based items.

3. **Who needs the supplies?**
   - Capture verified demand from students, vulnerable populations, low-income families, clinics, and NGOs.
   - Classify demand by product category, urgency level, location, and eligibility profile.

4. **Where should the supplies go?**
   - Use zip code, distance, delivery zone, demand density, and partner coverage.
   - Prioritize areas where need is high and delivery is feasible before expiry.

5. **What action should be taken?**
   - Dispatch immediately.
   - Hold for local demand.
   - Route through NGO partner.
   - Alert operations team.
   - Exclude due to expiry, compliance, or quality risk.

---
## 🏗️ Architecture

```text
Manufacturer Sales + Inventory Data
Hospital / Pharmacy / Clinic Supply Data
Recipient Signup + NGO Demand Data
Geographic + Demographic Data
        │
        ▼
┌──────────────────────────────┐
│      Ingestion Layer         │  ← Python + PySpark
│  - Manufacturer sales data   │
│  - Provider inventory data   │
│  - Recipient signup data     │
│  - NGO partner demand data   │
│  - Geographic/demographic    │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│   Data Lake Storage Layer    │  ← Local folders / Amazon S3
│  - raw/                      │
│  - processed/                │
│  - curated/                  │
│  - audit/                    │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│      Transform Layer         │  ← PySpark + DuckDB
│  - Sales forecasting         │
│  - Surplus prediction        │
│  - Expiry risk scoring       │
│  - Eligibility classification│
│  - Subsidy planning logic    │
│  - Demand matching           │
│  - Geographic matching       │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│    Data Quality Layer        │  ← pytest + custom checks
│  - Null checks               │
│  - Schema validation         │
│  - Duplicate detection       │
│  - Source-target recon       │
│  - Freshness checks          │
│  - Business rule validation  │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│   Curated Analytics Layer    │  ← DuckDB / Athena
│  - Surplus predictions       │
│  - Waste-risk inventory      │
│  - Verified recipient demand │
│  - Subsidy recommendations   │
│  - Redistribution queue      │
│  - Delivery planning outputs │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│ AWS Metadata & Query Layer   │  ← Glue + Athena
│  - Glue Data Catalog         │
│  - External tables           │
│  - Athena SQL queries        │
│  - Dashboard-ready datasets  │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│      Orchestration Layer     │  ← Airflow + Docker
│  - Ingestion DAG             │
│  - Transformation DAG        │
│  - Data quality DAG          │
│  - AWS S3 sync DAG           │
│  - Glue/Athena DAG           │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│   Streamlit Platform Layer   │
│  - Surplus forecast view     │
│  - Provider risk dashboard   │
│  - Recipient signup portal   │
│  - Eligibility review queue  │
│  - Subsidy planning view     │
│  - Geographic demand map     │
│  - Redistribution queue      │
│  - Waste reduction KPIs      │
└──────────────────────────────┘
```

---

## 📦 Tech Stack

| Layer | Technology |
|---|---|
| Data Processing | Python, PySpark, SQL |
| Local Warehouse | DuckDB |
| Cloud Option | AWS S3, Glue, Athena, Snowflake |
| Orchestration | Apache Airflow |
| Dashboard / Platform Prototype | Streamlit |
| Data Quality | pytest, custom validation checks |
| Forecasting / ML | scikit-learn, regression/time-series models |
| Geospatial Logic | Zip code distance scoring, regional demand mapping |
| Containerization | Docker |
| Version Control | GitHub |

---
## 📁 Project Structure
```text
ZeroWaste-Health/
│
├── synthetic_data/                         # Synthetic data generation
│   ├── generate_manufacturer_sales.py      # Manufacturer sales, unsold quantity, and waste-risk data
│   ├── generate_provider_inventory.py      # Hospital, pharmacy, and clinic inventory data
│   ├── generate_recipient_demand.py        # Student, family, clinic, NGO, and community demand data
│   └── generate_geographic_data.py         # Zip-code, distance, and demand-zone data
│
├── ingestion/                              # Raw data ingestion pipelines
│   ├── manufacturer_ingestion.py           # Ingest manufacturer sales and inventory forecast data
│   ├── provider_inventory_ingestion.py     # Ingest hospital, pharmacy, and clinic inventory data
│   ├── recipient_signup_ingestion.py       # Ingest recipient eligibility and demand signup data
│   ├── ngo_partner_ingestion.py            # Ingest NGO/community partner access requests
│   └── geographic_ingestion.py             # Ingest zip-code, demographic, and delivery-zone data
│
├── transform/                              # Business transformation logic
│   ├── surplus_forecasting.py              # Forecast future surplus and expected waste quantity
│   ├── expiry_risk_scoring.py              # Score inventory by expiry window and waste risk
│   ├── eligibility_classification.py       # Classify recipient eligibility and access category
│   ├── subsidy_planning.py                 # Plan financial assistance and subsidy access logic
│   ├── demand_matching.py                  # Match available supply to verified demand
│   ├── geographic_matching.py              # Match supply and demand using zip-code/distance logic
│   └── redistribution_router.py            # Generate dispatch, hold, NGO route, or alert decisions
│
├── aws/                                    # AWS cloud integration scripts
│   ├── s3_upload.py                        # Upload raw, processed, and curated datasets to Amazon S3
│   ├── glue_crawler_setup.py               # Create or trigger AWS Glue Crawlers
│   ├── athena_table_ddl.sql                # Athena external table definitions
│   ├── athena_queries.sql                  # Analytical SQL queries for curated datasets
│   ├── lambda_trigger.py                   # Optional Lambda trigger for event-based processing
│   └── cloudwatch_logging.py               # CloudWatch logging and pipeline monitoring helpers
│
├── data_lake/                              # Local data lake zones; mirrors AWS S3 layout
│   ├── raw/                                # Raw ingested data
│   │   ├── manufacturers/
│   │   ├── providers/
│   │   ├── recipients/
│   │   ├── ngos/
│   │   └── geography/
│   │
│   ├── processed/                          # Cleaned and standardized datasets
│   │   ├── manufacturer_forecasts/
│   │   ├── provider_inventory/
│   │   ├── recipient_profiles/
│   │   ├── eligibility_results/
│   │   └── geographic_zones/
│   │
│   └── curated/                            # Final trusted analytics-ready datasets
│       ├── surplus_predictions/
│       ├── waste_risk_inventory/
│       ├── verified_recipient_demand/
│       ├── subsidy_recommendations/
│       ├── redistribution_recommendations/
│       └── delivery_planning/
│
├── quality/                                # Data quality and validation checks
│   ├── null_checks.py                      # Missing values in inventory, expiry, income, and eligibility fields
│   ├── schema_validation.py                # Schema drift checks across pipeline runs
│   ├── duplicate_checks.py                 # Duplicate inventory, recipient, and request detection
│   ├── reconciliation.py                   # Source-to-target row count and quantity validation
│   ├── freshness_checks.py                 # SLA checks for updated inventory and demand data
│   └── business_rule_checks.py             # Eligibility, subsidy, expiry, and quantity rule validation
│
├── models/                                 # Forecasting and AI/ML models
│   ├── sales_forecast_model.py             # Forecast product sales and expected unsold quantity
│   ├── surplus_prediction_model.py         # Predict available surplus from manufacturers/providers
│   ├── demand_forecast_model.py            # Forecast demand by category and geography
│   └── assistance_priority_model.py        # Rank eligible recipients by need and access priority
│
├── orchestration/                          # Workflow orchestration
│   ├── zerowaste_dag.py                    # Main Airflow DAG
│   ├── aws_s3_sync_dag.py                  # Optional DAG to sync local data lake to S3
│   ├── glue_athena_dag.py                  # Optional DAG to trigger Glue Crawlers and Athena queries
│   └── data_quality_dag.py                 # Dedicated data quality validation workflow
│
├── dashboard/                              # Streamlit dashboard and platform interface
│   ├── app.py                              # Main dashboard app
│   ├── surplus_forecast_view.py            # Manufacturer/provider surplus forecast dashboard
│   ├── inventory_risk_view.py              # Provider inventory expiry-risk dashboard
│   ├── recipient_signup_view.py            # Recipient signup and demand capture interface
│   ├── eligibility_review_view.py          # Eligibility review and verification queue
│   ├── subsidy_planning_view.py            # Financial assistance and subsidy planning dashboard
│   ├── geographic_map_view.py              # Supply-demand map and delivery planning view
│   └── redistribution_queue_view.py        # Ranked redistribution recommendation queue
│
├── sql/                                    # SQL models and analytical queries
│   ├── duckdb_tables.sql                   # Local DuckDB table creation scripts
│   ├── athena_external_tables.sql          # AWS Athena external table DDL
│   ├── surplus_analysis.sql                # Surplus and waste-risk analysis queries
│   ├── eligibility_analysis.sql            # Recipient eligibility and access analysis
│   └── redistribution_kpis.sql             # KPI queries for dashboard metrics
│
├── tests/                                  # Automated tests
│   ├── unit/
│   │   ├── test_surplus_forecasting.py
│   │   ├── test_expiry_risk_scoring.py
│   │   ├── test_eligibility_rules.py
│   │   └── test_subsidy_planning.py
│   │
│   └── integration/
│       ├── test_ingestion_to_processed.py
│       ├── test_processed_to_curated.py
│       ├── test_recommendation_pipeline.py
│       └── test_aws_s3_athena_flow.py
│
├── config/                                 # Configuration files
│   ├── config.yaml                         # Pipeline configuration
│   ├── aws_config.yaml                     # S3 bucket, Glue database, Athena workgroup settings
│   ├── eligibility_rules.yaml              # Recipient category and verification rules
│   ├── subsidy_rules.yaml                  # Financial assistance and subsidy rules
│   └── logging.yaml                        # Logging configuration
│
├── docs/                                   # Documentation
│   ├── architecture.md                     # Architecture explanation
│   ├── data_dictionary.md                  # Field definitions
│   ├── aws_setup.md                        # AWS S3, Glue, Athena setup guide
│   ├── eligibility_framework.md            # Recipient verification and access rules
│   └── business_rules.md                   # Forecasting, routing, and subsidy logic
│
├── logs/                                   # Local pipeline logs
├── docker-compose.yml                      # Docker setup for Airflow and local services
├── requirements.txt                        # Python dependencies
├── README.md                               # Project documentation
└── .gitignore                              # Files/folders excluded from Git
```

---

## 🗃️ Data Model

### Manufacturer Inventory & Sales Forecast Table

| Field | Type | Description |
|---|---|---|
| manufacturer_id | STRING | Manufacturer identifier |
| product_id | STRING | Product identifier |
| product_name | STRING | Medicine or supply name |
| category | STRING | OTC, Bandage, IV Fluid, Supplement, Approved Supply |
| production_quantity | INTEGER | Quantity produced |
| historical_sales_quantity | INTEGER | Historical sold units |
| forecasted_sales_quantity | INTEGER | Predicted future sales |
| projected_leftover_quantity | INTEGER | Quantity expected to remain unsold |
| expiry_date | DATE | Product expiry date |
| forecast_run_timestamp | TIMESTAMP | Forecast generation timestamp |

### Provider Inventory Table

| Field | Type | Description |
|---|---|---|
| inventory_id | STRING | Unique inventory record ID |
| provider_id | STRING | Hospital, pharmacy, or clinic identifier |
| provider_type | STRING | Manufacturer, Hospital, Pharmacy, Clinic |
| product_id | STRING | Product identifier |
| product_name | STRING | Supply or medicine name |
| category | STRING | Product category |
| quantity_available | INTEGER | Current available units |
| average_monthly_usage | INTEGER | Historical usage rate |
| projected_unused_quantity | INTEGER | Quantity likely to remain unused |
| expiry_date | DATE | Expiry date |
| days_to_expiry | INTEGER | Days remaining before expiry |
| zip_code | STRING | Provider location |
| ingestion_timestamp | TIMESTAMP | Record ingestion timestamp |

### Recipient Eligibility Table

| Field | Type | Description |
|---|---|---|
| recipient_id | STRING | Unique recipient ID |
| recipient_type | STRING | Student, vulnerable populations, LowIncomeFamily, Clinic, NGO |
| zip_code | STRING | Recipient location |
| requested_category | STRING | Requested product category |
| urgency_level | STRING | HIGH / MEDIUM / LOW |
| student_email_domain | STRING | Student email domain for verification |
| student_id_verified | BOOLEAN | Whether student ID was verified |
| graduation_year | INTEGER | Expected graduation year |
| age_group | STRING | Age range for eligibility grouping |
| family_income_band | STRING | Income range category |
| employment_proof_type | STRING | Job/income proof category |
| household_need_score | FLOAT | Score based on income, household, and lifestyle indicators |
| verification_status | STRING | PENDING / VERIFIED / REJECTED |
| signup_timestamp | TIMESTAMP | Signup timestamp |

### NGO Partner Table

| Field | Type | Description |
|---|---|---|
| ngo_id | STRING | NGO/community partner identifier |
| ngo_name | STRING | Partner organization name |
| service_zip_codes | STRING | Covered service areas |
| approved_categories | STRING | Product categories partner can distribute |
| partner_verification_status | STRING | PENDING / APPROVED / REJECTED |
| distribution_capacity | INTEGER | Monthly delivery/handling capacity |
| contact_channel | STRING | Email, phone, SMS, portal |

### Redistribution Recommendations Table

| Field | Type | Description |
|---|---|---|
| recommendation_id | STRING | Unique recommendation ID |
| inventory_id | STRING | Matched supply record |
| recipient_id | STRING | Matched eligible recipient or group |
| ngo_id | STRING | Optional NGO partner for delivery |
| match_score | FLOAT | Overall supply-demand match score |
| expiry_urgency_score | FLOAT | Urgency based on days to expiry |
| eligibility_score | FLOAT | Recipient eligibility and need score |
| distance_km | FLOAT | Distance between supply and demand location |
| recommended_quantity | INTEGER | Quantity recommended for redistribution |
| recommended_action | STRING | DISPATCH / NGO_ROUTE / HOLD / ALERT / EXCLUDE |
| generated_timestamp | TIMESTAMP | Recommendation generation timestamp |

---

## 🧪 Data Quality & Governance Checks

| Check | Description |
|---|---|
| Inventory Null Check | Flags missing expiry dates, quantities, provider IDs, product IDs, and zip codes |
| Forecast Validation | Checks forecasted sales, projected leftover quantity, and negative quantity anomalies |
| Eligibility Validation | Validates required signup fields based on recipient type |
| Duplicate Detection | Detects duplicate inventory, recipient, and NGO records |
| Schema Validation | Detects schema drift across ingestion runs |
| Reconciliation | Compares source, processed, and curated row counts |
| Freshness Check | Alerts if manufacturer, provider, or recipient data is stale |
| Privacy Control | Masks sensitive eligibility fields before analytics consumption |
| Compliance Filter | Excludes restricted or non-redistributable product categories |

---

## 📊 Dashboard / Platform Features

- **Surplus Forecast Dashboard** — projected leftover quantity by manufacturer, provider, category, and expiry window.
- **Provider Inventory Risk View** — hospitals, pharmacies, and clinics with high unused inventory risk.
- **Recipient Signup Portal** — captures student,vulnerable populations, low-income family, clinic, and NGO demand.
- **Eligibility Review Queue** — verifies recipient category, required proof fields, and access status.
- **Geographic Demand Map** — visualizes underserved zones, supply locations, and delivery distance.
- **Redistribution Recommendation Queue** — ranked dispatch, NGO route, hold, alert, and exclude decisions.
- **Waste Reduction KPIs** — units saved, projected waste avoided, communities served, and delivery SLA.

---
## 🌱 Expected Business Impact / Success Metrics

Because this project uses synthetic data, the impact metrics below represent simulated targets and success criteria the platform is designed to measure.

| Metric | Target / Simulated Goal |
|---|---|
| Projected surplus identified | 10,000+ units/month in simulated inventory data |
| Waste-risk inventory flagged | 80%+ of near-expiry eligible inventory identified before expiration |
| Verified recipient groups served | Students, Vulnerable population, low-income families, clinics, and NGO partners |
| Redistribution recommendation SLA | Recommendations generated within < 1 hour after ingestion |
| Data quality pass rate | > 95% validation success across required pipeline checks |
| Delivery planning coverage | Zip-code level routing, supply-demand matching, and demand-zone mapping |
| Subsidy planning support | Eligibility-based assistance logic available for free or reduced-cost distribution decisions |

---

## 🚀 Getting Started

### Prerequisites

- Python 3.9+
- Docker Desktop
- Git

### Installation

```bash
# Clone the repo
git clone https://github.com/yourusername/ZeroWaste-Health.git
cd ZeroWaste-Health

# Create virtual environment
python -m venv venv

# Windows
venv\Scripts\activate

# WSL/Linux/Mac
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Generate synthetic datasets
python synthetic_data/generate_manufacturer_sales.py
python synthetic_data/generate_provider_inventory.py
python synthetic_data/generate_recipient_signups.py
python synthetic_data/generate_ngo_partners.py
python synthetic_data/generate_geographic_data.py

# Run pipeline modules
python ingestion/manufacturer_ingestion.py
python ingestion/provider_inventory_ingestion.py
python transform/surplus_quantity_prediction.py
python transform/eligibility_scoring.py
python transform/redistribution_router.py

# Launch dashboard
streamlit run dashboard/app.py
```

### Run with Docker / Airflow

```bash
docker-compose up -d
# Visit http://localhost:8080
# Username: admin | Password: admin
```

---

## 🔮 Future Roadmap

- [ ] Real manufacturer inventory and sales API integration.
- [ ] Hospital, pharmacy, and clinic inventory integration with partner consent.
- [ ] AI-powered demand prediction using demographic, geographic, and seasonal trends.
- [ ] Recipient identity and eligibility verification workflow.
- [ ] NGO partner approval and delivery capacity management.
- [ ] SMS/email alerting for recipient approvals and supply availability.
- [ ] Cloud deployment using AWS S3, Glue, Athena, Lambda, and Snowflake.
- [ ] Role-based access control and privacy-preserving analytics layer.

---

## 🤝 Inspiration

This project was independently designed around the idea that healthcare waste reduction requires more than expiry tracking. A useful platform must forecast surplus before expiry, verify eligible demand, include community partners, and plan redistribution using geography-aware data engineering. Organizations like [SIRUM](https://sirum.org) and [MyTablet](https://mytablet.org) are doing incredible work in this space. ZeroWaste-Health is a data engineering exploration of how technology and automation can help scale solutions like these.

---

## 👩‍💻 Author

**Rishita Reddy**  
Master's in Business Analytics — University of Texas at Dallas  
Data Engineer |  
📧 rrmaryada@gmail.com

---

## 📄 License

This project is intended for educational and portfolio purposes only. All rights reserved.

---

*Predict surplus. Verify need. Route impact.*
