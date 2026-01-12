# Airbnb End-to-End Data Engineering Project

##  Overview
This project implements a **complete end-to-end data engineering pipeline** for Airbnb data using modern cloud and analytics engineering tools.  
It demonstrates **industry best practices** in data warehousing, transformation, and analytics using **Snowflake, dbt (Data Build Tool), AWS S3, and Python**.

The pipeline processes Airbnb **listings, bookings, and hosts** data using a **Medallion Architecture (Bronze → Silver → Gold)** and supports:

- Incremental data loading  
- Slowly Changing Dimensions (SCD Type 2)  
- Analytics-ready fact and OBT tables  
- Modular, testable, and documented dbt models  

>  **Important:**  
> The **core dbt implementation and transformation logic lives inside the `aws_dbt_snowflake_project/` folder**.  
> The **`Supporting Images/` folder documents the development progress, architecture, and execution screenshots**.

---

##  Architecture

### Data Flow
```

Source CSV Files
↓
AWS S3
↓
Snowflake (Staging)
↓
Bronze Layer (Raw)
↓
Silver Layer (Cleaned)
↓
Gold Layer (Analytics Ready)

```

### Medallion Layers
- **Bronze** → Raw ingested data
- **Silver** → Cleaned, standardized data
- **Gold** → Business-ready analytical datasets

---

##  Technology Stack

- **Cloud Data Warehouse:** Snowflake  
- **Transformation Layer:** dbt (Data Build Tool)  
- **Cloud Storage:** AWS S3  
- **Programming Language:** Python 3.12+  
- **Version Control:** Git & GitHub  

### Key dbt Features Used
- Incremental models  
- Snapshots (SCD Type 2)  
- Custom macros  
- Jinja templating  
- Testing & documentation  
- Ephemeral models  

---

## Data Model

### Bronze Layer (Raw Data)
Minimal transformation on ingested data:
- `bronze_bookings`
- `bronze_hosts`
- `bronze_listings`

### Silver Layer (Cleaned Data)
Validated and standardized datasets:
- `silver_bookings`
- `silver_hosts`
- `silver_listings`

### Gold Layer (Analytics-Ready)
Optimized for analytics and BI:
- `fact` – Fact table for dimensional modeling  
- `obt` – One Big Table (denormalized)  
- Ephemeral models for intermediate joins  

---

##  Snapshots (SCD Type 2)
Tracks historical changes using dbt snapshots:
- `dim_bookings`
- `dim_hosts`
- `dim_listings`

Features:
- Valid-from / valid-to timestamps  
- Historical state preservation  
- Point-in-time analysis support  

---

##  Project Structure

```

AIRBNB DE END TO END
│
├── aws_dbt_snowflake_project/      # MAIN dbt project (core logic)
│   ├── dbt_project.yml
│   ├── models/
│   │   ├── sources/
│   │   ├── bronze/
│   │   ├── silver/
│   │   └── gold/
│   ├── macros/
│   ├── analyses/
│   ├── snapshots/
│   ├── tests/
│   └── seeds/
│
├── Supporting Images/              # Screenshots & progress documentation
│
├── bookings.csv
├── hosts.csv
├── listings.csv
├── main.py
├── pyproject.toml
├── README.md
└── .gitignore

````

---

## 🚀 Getting Started

### Prerequisites
- Snowflake Account  
- AWS Account (for S3)  
- Python 3.12+  
- Git  

---

## 🔧 Installation

### 1️ Clone the Repository
```bash
git clone <repository-url>
cd AIRBNB-DE-END-TO-END
````

### 2️ Create Virtual Environment

```bash
python -m venv .venv
.venv\Scripts\Activate.ps1    # Windows
# or
source .venv/bin/activate     # Linux/Mac
```

### 3️ Install Dependencies

```bash
pip install -e .
```

Core dependencies:

* `dbt-core>=1.11.2`
* `dbt-snowflake>=1.11.0`
* `sqlfmt`

---

##  Configure Snowflake Connection

Create `~/.dbt/profiles.yml`:

```yaml
aws_dbt_snowflake_project:
  target: dev
  outputs:
    dev:
      type: snowflake
      account: <your-account-identifier>
      user: <your-username>
      password: <your-password>
      role: ACCOUNTADMIN
      warehouse: COMPUTE_WH
      database: AIRBNB
      schema: dbt_schema
      threads: 4
```


---

## 🗄️ Data Setup

### Create Staging Tables

Run `DDL/ddl.sql` in Snowflake to create staging tables.

### Load Source Data

```
bookings.csv → AIRBNB.STAGING.BOOKINGS
hosts.csv    → AIRBNB.STAGING.HOSTS
listings.csv → AIRBNB.STAGING.LISTINGS
```

---

##  Usage (dbt)

Navigate to the dbt project:

```bash
cd aws_dbt_snowflake_project
```

### Test Connection

```bash
dbt debug
```

### Install Packages

```bash
dbt deps
```

### Run Models

```bash
dbt run
```

### Run by Layer

```bash
dbt run --select bronze.*
dbt run --select silver.*
dbt run --select gold.*
```

### Tests & Snapshots

```bash
dbt test
dbt snapshot
```

### Documentation

```bash
dbt docs generate
dbt docs serve
```

### Build Everything

```bash
dbt build
```

---

##  Key Features

### Incremental Loading

Efficient processing of new/updated records:

```sql
{{ config(materialized='incremental') }}

{% if is_incremental() %}
  WHERE created_at > (SELECT COALESCE(MAX(created_at), '1900-01-01') FROM {{ this }})
{% endif %}
```

### Custom Macros

Reusable business logic:

```sql
{{ tag('CAST(price_per_night AS INT)') }}
```

### Dynamic SQL with Jinja

Maintainable joins using loops and configs.

---

##  Data Quality & Lineage

* Source-level validation tests
* Not-null & unique checks
* Referential integrity
* Full lineage visualization via dbt docs

---

##  Security & Best Practices

* Credentials stored via environment variables
* Role-based access control in Snowflake
* Clean Git history
* Modular dbt project structure

---

##  Notes

* The **main transformation logic is inside `aws_dbt_snowflake_project/`**
* The **Supporting Images folder documents development progress**
* This project is designed for **portfolio and real-world demonstration**

---

##  Author

**Gurpreet Kaur**
Data Engineer / Analytics Engineer

```
