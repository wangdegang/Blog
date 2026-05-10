---
layout:     post
title:      "From Databricks to Your Analytics Tool: A RWD Workflow"
subtitle:   ""
excerpt:    "Query, filter, and prepare real-world data in Databricks, then bring analytic-ready tables to Python, R, or SAS"
author:     "Degang Wang"
date:       2026-05-10
description: "How to manipulate large-scale real-world data in Databricks and bring analysis-ready datasets to Python, R, or SAS."
image: "/img/header-tech.jpeg"
publishDate: 2026-05-10
tags:
    - Databricks
    - PySpark
    - RWD

categories: [ Tech ]
URL: "/2026/05/10/databricks-rwd-workflow/"
---

## Why Not Just Load Everything Locally?

Real-world data (RWD) — claims, EHR, registries — is massive. A single claims database can have billions of rows across diagnosis, procedure, and pharmacy tables. You cannot `pd.read_csv()` your way through it.

The workflow that works:

1. **Manipulate data in Databricks** — filter, join, aggregate using SQL on distributed compute
2. **Create an analytic-ready table** — a focused cohort with only the columns and rows you need
3. **Bring the result to your analytics tool** — Python, R, or SAS for statistical analysis and visualization

This post walks through that workflow using a diabetes cohort as an example. While we use Databricks here, the same pattern applies to any big data computing engine — Snowflake, Impala, AWS Athena, or Google BigQuery. The principle is the same: reduce data remotely, then bring the result to your analytics tool.

<!--more-->

## Prerequisites

This tutorial assumes you have Databricks Connect set up. If not, see the [previous post](/2026/05/08/databricks-connect/) for setup instructions.

We'll use a helper module (`dbk.py`) that wraps common Databricks operations:

```python
import os
from databricks.connect import DatabricksSession

_spark = None


def get_spark_session(host=None, token=None, cluster_id=None):
    global _spark
    host = host or os.getenv("DATABRICKS_HOST")
    token = token or os.getenv("DATABRICKS_TOKEN")
    cluster_id = cluster_id or os.getenv("DATABRICKS_CLUSTER_ID")

    _spark = (
        DatabricksSession.builder
        .host(host)
        .token(token)
        .clusterId(cluster_id)
        .getOrCreate()
    )
    return _spark


def _get_spark():
    if _spark is None:
        get_spark_session()
    return _spark


def run_query(sql):
    return _get_spark().sql(sql)


def show_query(sql, truncate=False):
    _get_spark().sql(sql).show(1000, truncate=truncate)


def drop_schema(schema_name):
    spark = _get_spark()
    tables_df = spark.sql(f"SHOW TABLES IN {schema_name}")
    table_names = [row.tableName for row in tables_df.collect()]
    for table_name in table_names:
        spark.sql(f"DROP TABLE {schema_name}.`{table_name}`")
    spark.sql(f"DROP SCHEMA {schema_name} RESTRICT")
```

## Step 1: Preview the Source Data

Start by looking at what's in your source table:

```python
from dbk import get_spark_session, show_query, run_query

get_spark_session()

# Show the first 100 records from the diagnosis table
show_query("""
    SELECT *
    FROM my_catalog.claims.diagnosis
    LIMIT 100
""")
```

This runs on the Databricks cluster — only the 100-row result travels to your local machine.

## Step 2: Define Your Cohort Logic

For a diabetes cohort, we define the ICD-10 codes of interest in a Python object, then use them in SQL:

```python
# ICD-10-CM codes for diabetes mellitus
DIABETES_DX_CODES = [
    "E10",   # Type 1 diabetes
    "E11",   # Type 2 diabetes
    "E13",   # Other specified diabetes
]

# Build SQL filter condition
dx_filter = " OR ".join(
    [f"dx_code LIKE '{code}%'" for code in DIABETES_DX_CODES]
)
```

## Step 3: Create an Analytic-Ready Table in Databricks

Now we filter the large diagnosis table and save a focused cohort table back to Databricks:

```python
# Create a schema for your project
run_query("CREATE SCHEMA IF NOT EXISTS my_catalog.diabetes_study")

# Create the diabetes cohort table
run_query(f"""
    CREATE TABLE my_catalog.diabetes_study.cohort AS
    SELECT DISTINCT
        patient_id,
        dx_code,
        service_date,
        provider_id
    FROM my_catalog.claims.diagnosis
    WHERE {dx_filter}
""")

# Verify the result
show_query("""
    SELECT COUNT(*) AS n_records, COUNT(DISTINCT patient_id) AS n_patients
    FROM my_catalog.diabetes_study.cohort
""")
```

The heavy lifting (scanning billions of rows, filtering, deduplicating) happens on the cluster. The resulting table is much smaller and analysis-ready.

## Step 4: Bring Data to Your Analytics Tool

Now that the analytic table is small enough, bring it to your local session.

### Python (pandas)

```python
df = run_query("""
    SELECT * FROM my_catalog.diabetes_study.cohort
""").toPandas()

print(df.shape)
df.head()
```

### R (sparklyr or odbc)

```r
library(DBI)
library(odbc)

con <- dbConnect(
  odbc::odbc(),
  Driver = "Simba Spark ODBC Driver",
  Host = Sys.getenv("DATABRICKS_HOST"),
  Port = 443,
  HTTPPath = "/sql/1.0/warehouses/your_warehouse_id",
  AuthMech = 3,
  UID = "token",
  PWD = Sys.getenv("DATABRICKS_TOKEN"),
  ThriftTransport = 2,
  SSL = 1
)

cohort <- dbGetQuery(con, "SELECT * FROM my_catalog.diabetes_study.cohort")
str(cohort)
```

### SAS (SAS/ACCESS to Spark or ODBC)

```sas
libname dbk odbc
  datasrc="Databricks"
  user="token"
  password=&databricks_token;

proc sql;
  create table work.cohort as
  select * from dbk.'diabetes_study.cohort'n;
quit;

proc means data=work.cohort;
  class dx_code;
  var patient_id;
run;
```

## Step 5: Clean Up When Done

After your analysis is complete, drop the intermediate tables:

```python
from dbk import drop_schema

drop_schema("my_catalog.diabetes_study")
```

## The Big Picture

```
┌─────────────────────────────────────────────────────────────┐
│                    Databricks (Remote)                      │
│                                                             │
│  ┌──────────────┐    SQL Filter     ┌──────────────────┐    │
│  │ Raw RWD      │ ──────────────►   │ Analytic-Ready   │    │
│  │ (billions    │   (runs on        │ Table            │    │
│  │  of rows)    │    cluster)       │ (manageable size)│    │
│  └──────────────┘                   └────────┬─────────┘    │
│                                              │              │
└──────────────────────────────────────────────┼──────────────┘
                                               │
                             ┌─────────────────┼─────────────────┐
                             │                 │                 │
                             ▼                 ▼                 ▼
                        ┌──────────┐     ┌──────────┐      ┌──────────┐
                        │  Python  │     │    R     │      │   SAS    │
                        │ (pandas) │     │(DBI/odbc)│      │ (ODBC)   │
                        └──────────┘     └──────────┘      └──────────┘
                             │                 │                 │
                             ▼                 ▼                 ▼
                         Statistical analysis, visualization, modeling
```

## Key Takeaways

- **Never pull raw RWD to local** — filter and aggregate in Databricks first
- **SQL is your data reduction tool** — use it to create focused, analytic-ready tables
- **The analytic table is the handoff point** — small enough for pandas, R, or SAS
- **Keep diagnosis codes in Python objects** — version-controlled, reusable, auditable
- **Clean up** — drop intermediate tables when the study is complete
