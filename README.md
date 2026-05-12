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
Manufacturer Sales & Inventory Data
Hospital / Pharmacy / Clinic Inventory
Recipient Signup & Eligibility Data
NGO / Community Partner Requests
Geographic & Demographic Data
        │
        ▼
┌──────────────────────────────┐
│        Ingestion Layer        │  ← Python + PySpark
│  manufacturer_ingestion       │
│  provider_inventory_ingestion │
│  recipient_signup_ingestion   │
│  ngo_partner_ingestion        │
│  geospatial_ingestion         │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│      Forecasting Layer        │  ← PySpark + ML Models
│  manufacturer_sales_forecast  │
│  unused_inventory_prediction  │
│  surplus_quantity_forecast    │
│  expiry_risk_prediction       │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│      Eligibility Layer        │  ← Python + SQL Rules
│  student_verification         │
│  vulnerable_eligibility       │
│  income_based_eligibility     │
│  ngo_access_approval          │
│  privacy_masking              │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│      Matching & Routing       │  ← PySpark + DuckDB
│  expiry_scoring               │
│  demand_matching              │
│  geo_distance_scoring         │
│  redistribution_router        │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│       Data Quality Layer      │  ← pytest + custom checks
│  schema_validation            │
│  null_checks                  │
│  duplicate_detection          │
│  eligibility_validation       │
│  reconciliation               │
│  freshness_checks             │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│        Curated Layer          │  ← DuckDB local / Snowflake cloud
│  surplus_forecasts            │
│  verified_recipients          │
│  ngo_partner_access           │
│  demand_zones                 │
│  redistribution_recommendations│
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│        Orchestration          │  ← Apache Airflow
│  zerowaste_forecast_dag       │
│  zerowaste_matching_dag       │
│  zerowaste_quality_dag        │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│          Platform UI          │  ← Streamlit
│  Recipient signup             │
│  NGO partner portal           │
│  Inventory risk dashboard     │
│  Delivery planning dashboard  │
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
├── synthetic_data/
│   ├── generate_manufacturer_sales.py
│   ├── generate_provider_inventory.py
│   ├── generate_recipient_signups.py
│   ├── generate_ngo_partners.py
│   └── generate_geographic_data.py
│
├── ingestion/
│   ├── manufacturer_ingestion.py
│   ├── provider_inventory_ingestion.py
│   ├── recipient_signup_ingestion.py
│   ├── ngo_partner_ingestion.py
│   └── geospatial_ingestion.py
│
├── transform/
│   ├── sales_forecasting.py
│   ├── surplus_quantity_prediction.py
│   ├── expiry_scoring.py
│   ├── eligibility_scoring.py
│   ├── demand_matching.py
│   └── redistribution_router.py
│
├── quality/
│   ├── null_checks.py
│   ├── schema_validation.py
│   ├── duplicate_detection.py
│   ├── eligibility_validation.py
│   ├── reconciliation.py
│   └── freshness_checks.py
│
├── models/
│   ├── manufacturer_sales_forecast_model.py
│   ├── expiry_risk_model.py
│   └── demand_forecast_model.py
│
├── orchestration/
│   ├── zerowaste_forecast_dag.py
│   ├── zerowaste_matching_dag.py
│   └── zerowaste_quality_dag.py
│
├── dashboard/
│   ├── app.py
│   ├── recipient_portal.py
│   ├── ngo_partner_portal.py
│   └── utils.py
│
├── data/
│   ├── raw/
│   ├── processed/
│   └── curated/
│
├── tests/
│   ├── unit/
│   └── integration/
│
├── docs/
│   ├── architecture.md
│   ├── data_dictionary.md
│   └── privacy_and_compliance_notes.md
│
├── config/
│   ├── config.yaml
│   └── logging.yaml
│
├── logs/
├── docker-compose.yml
├── requirements.txt
└── README.md
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

## 🌱 Business Impact

| Metric | Target |
|---|---|
| Projected surplus identified | 10,000+ units/month simulated |
| Waste-risk inventory flagged | 80%+ of near-expiry eligible inventory |
| Verified recipient groups served | Students, school children, low-income families, clinics, NGOs |
| Redistribution recommendation SLA | < 1 hour after ingestion |
| Data quality pass rate | > 95% |
| Delivery planning coverage | Zip-code level routing and demand mapping |

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
