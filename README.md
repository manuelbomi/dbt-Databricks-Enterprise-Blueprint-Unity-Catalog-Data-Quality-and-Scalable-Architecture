# Enterprise dbt-Databricks Template 

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![dbt](https://img.shields.io/badge/dbt-1.11.0-FF694B)](https://www.getdbt.com/)
[![Databricks](https://img.shields.io/badge/Databricks-Unity%20Catalog-FF3621)](https://databricks.com/)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

A production-ready template for implementing dbt with Databricks Unity Catalog, featuring comprehensive data quality testing, documentation generation, and enterprise-grade architecture patterns.

##  Why This Template?

This repository documents a complete journey from initial setup to production deployment, addressing real-world challenges like:
- **Hive Metastore deprecation** and migration to Unity Catalog
- **Data quality framework** with configurable severity levels
- **Enterprise patterns** for scalable data modeling
- **Automated documentation** with dbt docs serve
- **CI/CD integration** for testing and deployment

##  Key Features

| Feature | Description | Your Implementation |
|---------|-------------|---------------------|
| **Unity Catalog Integration** | Modern data governance with Databricks | ✅ Complete setup with `workspace` catalog |
| **Data Quality Framework** | Configurable test severity (error/warn) | ✅ Model-level overrides demonstrated |
| **Custom Macros** | Reusable transformation logic | ✅ `dbt_databricks_proj_macro` with column transformation |
| **Jinja Templates** | Dynamic SQL generation | ✅ Loop-based column selection in `model3.sql` |
| **Package Management** | External dependency handling | ✅ `dbt_utils` integration |
| **Automated Documentation** | Self-serve data catalog | ✅ Local docs at `localhost:8080` |
| **Seed Data Loading** | CSV to table automation | ✅ `data.csv` with automatic schema inference |

##  Quick Start

### 1. Prerequisites
```bash
# Python 3.8+
python --version

# Databricks Account (Free Edition supports Unity Catalog)
# dbt-core and databricks adapter
pip install dbt-core dbt-databricks
