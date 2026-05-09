---
layout:     post
title:      "Databricks Connect: Run Spark Code from Your Local IDE"
subtitle:   ""
excerpt:    "Use Databricks Connect to run PySpark locally with remote cluster compute"
author:     "Degang Wang"
date:       2026-05-08
description: "A quick guide to using Databricks Connect for running PySpark code locally while leveraging Databricks cluster computing power — no notebook required."
image: "/img/header-tech.jpeg"
publishDate: 2026-05-08
tags:
    - Databricks
    - PySpark

categories: [ Tech ]
URL: "/2026/05/08/databricks-connect/"
---

## Why Databricks Connect?

The Databricks web notebook is great for interactive exploration, but sometimes you want to:

- Use your preferred local IDE (VS Code, PyCharm, etc.)
- Version control your code with git
- Run scripts in CI/CD pipelines
- Debug with local tools

Databricks Connect lets you do all of this while still using a remote Databricks cluster for compute.

<!--more-->

## How It Works

Databricks Connect replaces the local Spark execution engine with a connection to a remote Databricks cluster. Your code runs locally, but DataFrame operations are executed on the cluster.

## Setup

### 1. Install the package

```bash
pip install databricks-connect
```

Make sure the version matches your cluster's Databricks Runtime version.

### 2. Set environment variables

```bash
export DATABRICKS_HOST="https://your-workspace.cloud.databricks.com"
export DATABRICKS_TOKEN="your-personal-access-token"
export DATABRICKS_CLUSTER_ID="your-cluster-id"
```

You can find these in your Databricks workspace:
- **Host**: your workspace URL
- **Token**: User Settings > Developer > Access Tokens
- **Cluster ID**: Compute > your cluster > URL or JSON

### 3. Create a session

```python
import os
from databricks.connect import DatabricksSession

def get_spark_session(host=None, token=None, cluster_id=None):
    host = host or os.getenv("DATABRICKS_HOST")
    token = token or os.getenv("DATABRICKS_TOKEN")
    cluster_id = cluster_id or os.getenv("DATABRICKS_CLUSTER_ID")

    spark = (
        DatabricksSession.builder
        .host(host)
        .token(token)
        .clusterId(cluster_id)
        .getOrCreate()
    )
    return spark
```

### 4. Use it like any Spark session

```python
spark = get_spark_session()

df = spark.sql("SELECT * FROM my_catalog.my_schema.my_table LIMIT 10")
df.show()
```

## When to Use This vs. Notebooks

| Use Case | Recommended |
|----------|-------------|
| Quick data exploration | Notebook |
| Production scripts | Databricks Connect |
| Local debugging | Databricks Connect |
| Collaboration with non-Databricks users | Databricks Connect |
| Scheduled jobs | Databricks Jobs (or either) |

## Caveats

- The cluster must be running (or will auto-start if configured)
- Some Spark features (e.g., custom JVM libraries) may not work identically
- Network latency applies — large `collect()` calls will be slow
- Token expiration requires periodic refresh
