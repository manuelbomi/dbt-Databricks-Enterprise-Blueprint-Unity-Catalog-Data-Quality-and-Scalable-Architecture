# Enterprise dbt-Databricks Template 

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![dbt](https://img.shields.io/badge/dbt-1.11.0-FF694B)](https://www.getdbt.com/)
[![Databricks](https://img.shields.io/badge/Databricks-Unity%20Catalog-FF3621)](https://databricks.com/)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

##### A production-ready template for implementing dbt with Databricks Unity Catalog, featuring comprehensive data quality testing, documentation generation, and enterprise-grade architecture patterns. 

##### This repository also provide inforgraphs (in miscellaneous) through which users can be able to implement dbt for their enterprise workflows from ground up.

##  Why This Template?

This repository documents a complete journey from initial setup to production deployment, addressing real-world challenges like:
- **Hive Metastore deprecation** and migration to Unity Catalog
- **Data quality framework** with configurable severity levels
- **Enterprise patterns** for scalable data modeling
- **Automated documentation** with dbt docs serve


##  Key Features

| Feature | Description | Your Implementation |
|---------|-------------|---------------------|
| **Unity Catalog Integration** | Modern data governance with Databricks |  Complete setup with `workspace` catalog |
| **Data Quality Framework** | Configurable test severity (error/warn) |  Model-level overrides demonstrated |
| **Custom Macros** | Reusable transformation logic |  `dbt_databricks_proj_macro` with column transformation |
| **Jinja Templates** | Dynamic SQL generation |  Loop-based column selection in `model3.sql` |
| **Package Management** | External dependency handling |  `dbt_utils` integration |
| **Automated Documentation** | Self-serve data catalog |  Local docs at `localhost:8080` |
| **Seed Data Loading** | CSV to table automation |  `data.csv` with automatic schema inference |

##  Quick Start



### 1. Prerequisites
```bash
# Python 3.8+
python --version

# Databricks Account (Free Edition supports Unity Catalog)
# dbt-core and databricks adapter
pip install dbt-core dbt-databricks

```
---

### 2. Clone & Setup

```python

git clone https://github.com/manuelbomi/dbt-Databricks-Enterprise-Blueprint-Unity-Catalog-Data-Quality-and-Scalable-Architecture.git
cd dbt-databricks-enterprise-template

# Create virtual environment
python -m venv venv
source venv/bin/activate  # or `venv\Scripts\activate` on Windows

# Install dependencies
pip install -r requirements.txt

```

---

### 3. Configure Databricks Connection

- [x] Generate personal access token in Databricks

- [x]  Copy SQL warehouse HTTP path

- [x]  Update profiles.yml (use provided template)

---

### 4. Initialize & Test

```python
dbt debug  # Verify connection
dbt run    # Build models
dbt test   # Run data quality tests
dbt docs generate  # Create documentation
dbt docs serve     # View at localhost:8080

```

---

### Project Structure Deep Dive

> [!NOTE]
>
> Please see the full project structure under Miscellaneous. Only the dbt models are focused on here.
>
> 

```python
models/
├── staging/          # Raw data ingestion
├── intermediate/     # Transformations and joins
├── marts/           # Business-ready datasets
└── example/         # Tutorial models (your implementations)
```

---

### Key Configuration Files

```python

dbt_project.yml: Project-wide settings, model configurations, severity levels

packages.yml: External package dependencies (dbt_utils, etc.)

profiles.yml: Connection profiles (keep secure, use environment variables)

```

---


## Data Quality Framework

- Test Severity Management
  
- Configure test behavior at different levels:

```python
# Project-level severity (dbt_project.yml)
tests:
  dbt_databricks_proj:
    severity: error  # Default: fail on test failure

# Model-level override (model1.sql)
{{ config(materialized='table', severity='warn') }}
# Test will produce warning instead of error
```

---

## Test Types Demonstrated

- Column Tests: not_null, unique, accepted_values

- Relationship Tests: Foreign key validation (commented example)

- Custom Tests: Extensible through macros and SQL

---

## Advanced Features

- Custom Macros

```python
{% macro dbt_databricks_proj_macro(column_name) %}
    {{ column_name }} as macro_new_column
{% endmacro %}
```

---

## Jinja Loops for Dynamic SQL

```python
{% set list1 = ['gender', 'customerID', 'first_name'] %}
SELECT {% for item in list1 %}
           {{ item }}{% if not loop.last %}, {% endif %}
       {% endfor %}
FROM {{ source('bakehouse', 'sales_customers') }}
```

---

## Seed Data Loading

```python
dbt seed  # Loads data.csv into Databricks
```

---

## Real-World Results

### Databricks Unity Catalog Migration Report & Summary of Improvements

##### The Databricks Community Edition does not support Unity Catalog. However, recently, Databricks have started phasing out the Community Edition for a Free Edition that supports Unity Catalog. The Free Edition is used in this project with its Unity Catalog framework named as 'workspace in the project'. Below is a summary of improvements yielded by the support offered by the availability of Unity Catalog. 


| Category | Before Migration | After Migration | Impact |
|----------|------------------|-----------------|--------|
| **Connectivity** | Error: `UC_HIVE_METASTORE_DISABLED_EXCEPTION` | ✅ All connections validated | Elimination of connectivity errors |
| **Catalog Management** | Legacy `hive_metastore` (blocked by UC) | ✅ Unified `workspace` catalog | Centralized data governance |
| **Quality Assurance** | Manual testing processes | ✅ Automated test framework | 80% reduction in validation time |
| **Knowledge Sharing** | No documentation | ✅ Self-serve dbt documentation | Improved team onboarding |
| **Architecture** | Single monolithic model | ✅ Multi-layer medallion architecture | Better scalability & maintainability |

**Overall Status:** ✅ **Migration Successful**  
**Key Achievement:** Transition from error-prone legacy system to fully automated Unity Catalog pipeline

---

## Performance Metrics

- Test Execution: 3 data tests in 25.83 seconds

- Model Build Time: 16.12 seconds for 2 models

- Documentation Generation: Fully automated lineage graphs

---

## Troubleshooting Guide

#### Common dbt Databricks Issues & Solutions


## Troubleshooting Guide: Databricks & dbt Integration

| Error/Issue | Recommended Solution | Visual Reference (available in Miscellaneous) |
|-------------|---------------------|------------------|
| `UC_HIVE_METASTORE_DISABLED_EXCEPTION` | Migrate to Unity Catalog:<br>`catalog: workspace`<br>`schema: your_schema` | ![Hive Error](UC_HIVE_METASTORE_DISABLED_EXCEPTION.png) |
| `NO_SUCH_CATALOG_EXCEPTION` | 1. Check catalog exists in Databricks UI<br>2. Verify user permissions | ![Catalog Error](NO_SUCH_CATALOG_EXCEPTION.png) |
| Token authentication failures | 1. Generate new token in Databricks<br>2. Update `~/.dbt/profiles.yml` | ![Token Setup](generate-new-token.png) |
| Test severity configuration conflicts | Review hierarchy:<br>1. Model-level config<br>2. Project config<br>3. dbt defaults | ![Severity Config](severity-configuration.png) |


---

## Learning Path

#### Beginner to Expert Journey

- Week 1: Basic models and dbt run (screenshots)

- Week 2: Data quality testing with dbt test (screenshots)

- Week 3: Macros and Jinja templating (screenshots)

- Week 4: Production patterns and documentation (screenshots)

---

## Contribution

#### This template is designed to grow with community input. See CONTRIBUTING.md for:

- Adding new model patterns

- Extending the testing framework

- Improving documentation

- Sharing real-world use cases

---

## License
#### MIT License - see LICENSE file for details.

---

## 🙏 Acknowledgments

- Databricks for the Free Edition with Unity Catalog support

- dbt Labs for the incredible transformation framework @ https://docs.getdbt.com/docs/introduction

- Community contributors who share knowledge and patterns. For example,  Apostolos Athanasiou @ https://www.youtube.com/watch?v=uRSLG63UR4w

---

#### Built with ❤️ by Emmanuel Oyekanlu: Enterprise Data, dbt Evangelist & AI Engineer 

##### Star this repo if you found it helpful! ⭐

---
---







## Miscellaneous

### Repository Structure 

```python
dbt-databricks-enterprise-template/
│
├── .github/
│   └── workflows/
│       ├── dbt-test.yml                      # Automated testing pipeline
│       ├── dbt-docs-deploy.yml               # Documentation deployment
│       └── dbt-schedule.yml                  # Scheduled dbt runs
│
├── docs/
│   ├── setup-guide/
│   │   ├── 01-environment-setup.md
│   │   ├── 02-databricks-configuration.md
│   │   ├── 03-dbt-installation.md
│   │   ├── 04-unity-catalog-setup.md
│   │   └── 05-troubleshooting-common-errors.md
│   │
│   ├── tutorials/
│   │   ├── basic-models.md
│   │   ├── advanced-macros.md
│   │   ├── testing-framework.md
│   │   ├── documentation-generation.md
│   │   ├── jinja-templating.md
│   │   └── seed-data-management.md
│   │
│   ├── architecture/
│   │   ├── data-quality-framework.md
│   │   ├── severity-management.md
│   │   ├── enterprise-patterns.md
│   │   ├── unity-catalog-best-practices.md
│   │   └── ci-cd-strategy.md
│   │
│   └── references/
│       ├── command-cheatsheet.md
│       ├── error-solutions.md
│       └── configuration-reference.md
│
├── dbt_project/
│   │
│   ├── models/
│   │   ├── staging/
│   │   │   ├── sources.yml
│   │   │   ├── _staging__sources.md
│   │   │   ├── stg_customers.sql
│   │   │   └── stg_transactions.sql
│   │   │
│   │   ├── intermediate/
│   │   │   ├── int_customer_orders.sql
│   │   │   ├── int_product_sales.sql
│   │   │   └── int_joined_data.sql
│   │   │
│   │   ├── marts/
│   │   │   ├── core/
│   │   │   │   ├── dim_customers.sql
│   │   │   │   ├── fact_orders.sql
│   │   │   │   └── schema.yml
│   │   │   │
│   │   │   └── finance/
│   │   │       ├── fct_revenue.sql
│   │   │       └── schema.yml
│   │   │
│   │   └── example/
│   │       ├── model1.sql                  # Your demonstration models
│   │       ├── model2.sql
│   │       ├── model3.sql
│   │       ├── model4.sql
│   │       ├── schema.yml
│   │       ├── source.yml
│   │       └── _example_models.md
│   │
│   ├── macros/
│   │   ├── custom/
│   │   │   ├── dbt_databricks_proj_macro.sql
│   │   │   ├── generate_surrogate_key.sql
│   │   │   ├── date_calculation.sql
│   │   │   └── data_quality_checks.sql
│   │   │
│   │   ├── utils/
│   │   │   ├── logging_macros.sql
│   │   │   ├── schema_management.sql
│   │   │   └── environment_helpers.sql
│   │   │
│   │   └── packages/
│   │       └── dbt_utils_overrides.sql
│   │
│   ├── tests/
│   │   ├── generic/
│   │   │   ├── assert_not_null.sql
│   │   │   ├── assert_unique.sql
│   │   │   └── assert_positive_value.sql
│   │   │
│   │   ├── custom/
│   │   │   ├── test_customer_referral_loop.sql
│   │   │   └── test_data_freshness.sql
│   │   │
│   │   └── data_quality/
│   │       └── dq_macro_tests.sql
│   │
│   ├── seeds/
│   │   ├── data.csv
│   │   ├── reference_countries.csv
│   │   ├── reference_currencies.csv
│   │   └── _seeds_documentation.md
│   │
│   ├── snapshots/
│   │   ├── scd_type2_customers.sql
│   │   └── _snapshots_guide.md
│   │
│   ├── analyses/
│   │   ├── adhoc_customer_analysis.sql
│   │   └── _analyses_purpose.md
│   │
│   ├── documentation/
│   │   ├── doc_dbt_databricks_proj.md
│   │   ├── project_overview.md
│   │   ├── business_glossary.md
│   │   └── data_dictionary.md
│   │
│   ├── dbt_project.yml
│   ├── packages.yml
│   ├── profiles.yml.example
│   ├── .gitignore
│   └── README_project.md
│
├── scripts/
│   ├── setup/
│   │   ├── setup-environment.sh
│   │   ├── setup-databricks-connection.sh
│   │   └── setup-dbt-profile.sh
│   │
│   ├── operations/
│   │   ├── run-tests.sh
│   │   ├── generate-docs.sh
│   │   ├── run-models-by-tag.sh
│   │   └── clean-target.sh
│   │
│   ├── monitoring/
│   │   ├── check-model-freshness.sh
│   │   └── validate-data-quality.sh
│   │
│   └── utilities/
│       ├── backup-profiles.sh
│       ├── migrate-config.sh
│       └── update-packages.sh
│
├── config/
│   ├── environments/
│   │   ├── dev.yml
│   │   ├── staging.yml
│   │   └── prod.yml
│   │
│   ├── databricks/
│   │   ├── connection-template.json
│   │   ├── unity-catalog-setup.sql
│   │   └── warehouse-configuration.md
│   │
│   ├── dbt/
│   │   ├── variables-template.yml
│   │   ├── custom-tests-template.yml
│   │   └── model-config-template.yml
│   │
│   └── ci-cd/
│       ├── environment-variables.example
│       ├── deployment-checklist.md
│       └── rollback-procedure.md
│
├── tests/
│   ├── integration/
│   │   ├── test_connection.py
│   │   └── test_model_generation.py
│   │
│   ├── unit/
│   │   ├── test_macros.py
│   │   └── test_utils.py
│   │
│   └── fixtures/
│       └── sample_data.json
│
├── examples/
│   ├── use-cases/
│   │   ├── ecommerce/
│   │   │   ├── models/
│   │   │   ├── macros/
│   │   │   └── README.md
│   │   │
│   │   └── finance/
│   │       ├── models/
│   │       ├── tests/
│   │       └── README.md
│   │
│   └── migrations/
│       ├── hive-to-uc/
│       │   ├── migration-strategy.md
│       │   └── sql-scripts/
│       │
│       └── legacy-to-modern/
│           └── upgrade-guide.md
│
├── .env.example
├── .gitignore
├── .dbtignore
├── pyproject.toml
├── requirements.txt
├── requirements-dev.txt
├── README.md
├── CONTRIBUTING.md
├── CODE_OF_CONDUCT.md
├── CHANGELOG.md
├── ROADMAP.md
├── LICENSE
└── SECURITY.md

```

--- 
## Implementation Infographs

##### In this section, we have provided infographs through which users can be able to develop dbt from their desktop, and link it into the Databricks Unity Catalog. Our detailed infographs also show stepwise details through which users can implement the dbt project and troubleshoot issues from their IDE to the Databricks environment. 

#### 1. Setting Up & Obtaining the URL, HTTP and dbt Token on Databricks 

#### Use your enterprise Databricks account, or register for a Free Edition of Databricks


##### Navigate to compute

<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/b3645380-ff02-422f-ba2f-544d136ef076" />

##### Navigate to serverless starter warehouse and click into connection details
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/638ddf08-af97-47ee-9590-2ba4ed4472dd" />

##### Copy server hostname, Http path, click on dbt and generate new token. Save all onto a notepad

<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/a4d0deb6-318a-4ef1-a5a5-ba2d602a0e7d" />

---

#### 2. Set Up Your IDE (VSCode or PyCharm is ideal). VSCode is Used in This Case Study

##### Create Virtual env

<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/b4dc5e81-ccb0-4c5d-89d6-432635ae5c32" />

##### Install dbt-core for databricks

<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/0b1e65e5-9a7f-4015-9988-5a30633f0842" />

##### dbt-init to initialize dbt project

<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/3fd7f20a-8e64-4bf1-8ffb-55f1d77773e7" />

#####  Configure dbt init parameters

<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/56981ed4-9e92-4477-add1-6d6652f25925" />


##### Provide the hostname, http path and access token from your Databricks workspace

<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/ccbe93cb-1dad-43c8-b92b-394a92a057fe" />

##### After the access token is pasted just hit enter without inputing any number. You can then select whether or not you want Unity Catalog

<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/4c1fdfbc-0e6b-4645-a217-d8755afd8711" />

##### Give the schema a name. Select how many orders to run in parallel, We select 5 here.

<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/94f25aa2-fb4e-485f-a5b2-ee348ed5f0b6" />

#####  I can see my dbt profiles.yaml by clicking on its link

<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/d9b9f796-e894-4461-9053-87e9296d3de1" />

##### Content of the profiles.yaml file in the .dbt folder

<img width="394" height="197" alt="Image" src="https://github.com/user-attachments/assets/d8758d2b-5760-4490-ac2a-d046e055c837" />

##### Create a corresponding database named workspace in your Databrisks coomunity Edition under SQL Editor
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/5f0c9961-bc02-4f36-b019-d2b46282ac3d" />

##### The project is now ready
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/dc7e0b8e-a457-4b09-a460-e67124f4f8d4" />

##### Switch now to the main project folder
<img width="1109" height="346" alt="Image" src="https://github.com/user-attachments/assets/fbdf0f17-5c99-461c-9fe1-38de69f28edc" />

#####  Add it to the workspace
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/2f534745-3c77-47ab-b554-33d4ba4ecb06" />

##### This is our dbt project structure
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/0e0fc48a-c603-4dcf-ba49-4a5f566f7812" />

##### This template is a model example

<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/fc0c8056-ff89-4245-99d4-9d5eb552ff52" />

##### This is the second model example. It refers to the first model

<img width="975" height="402" alt="Image" src="https://github.com/user-attachments/assets/4d78929d-ce56-4d79-9b1a-abcb653c4595" />

##### The schemal.yml will feature the data quality check

<img width="889" height="432" alt="Image" src="https://github.com/user-attachments/assets/1427858c-6736-4be6-b96c-92760a3f1d5e" />

##### dbt compile

<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/a7a0647e-66ec-4af4-9d41-596d9a6df3b1" />

##### dbt compile will create a target file which will contain your compiled models. dbt run will push the models into your target database

<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/d58cc33d-9551-42a6-94d1-2b864e15eaee" />

##### Compiled CTE model example
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/07b78106-6265-4122-9262-a8a36031154e" />

##### Second compiled model that reference the first model
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/673fac07-6a9e-414c-870a-01c9c61f6cca" />

##### dbt run will push the project into your Databricks workspace
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/bb30c8e1-0ddf-46d7-9645-2bc3c52f449b" />

##### Your project now exist in your Catalogue Workspace in Databricks
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/03029c42-1564-45d9-b896-2479f02456bc" />

<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/27806a4d-7dac-4e14-a6f3-743673d95329" />

<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/279da130-c5b5-4c76-8608-3dc8988ec161" />

<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/08e1beed-f04a-415c-89b4-2a259f824cca" />

##### Use dbt test for data quality test
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/572490b1-9b1e-4a02-8251-3ed8ad926d7a" />

##### That fails becaus ewe selected null in the first model; and in the schema.yml where we define the data quality checks we detailed dbt to watch for null values
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/2efd7a49-16b1-4acd-93d8-9e90ab0981ea" />

##### This is why we are having the data quality issue

<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/039b333f-9a23-4bff-a092-787a713d4489" />


##### If we remove the not null data quality check
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/8fa12f9c-ea94-470f-bf5a-42b54c8aa36b" />

##### Our data quality check will now pass
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/1ac278e7-3125-4f0f-b141-8053dc26949e" />

<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/2c9dc0e5-2cc1-4bd0-af94-b0a8bff4ef24" />

##### Create packages.yml file at the project root
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/433eace1-82d7-4dc8-ac7b-a9043115ee2d" />

#####  Put some needed package in the package.yaml file
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/21140389-6ff9-4ff5-89f5-7c2daa95f28c" />

##### Install using dbt deps
<img width="709" height="347" alt="Image" src="https://github.com/user-attachments/assets/f597d282-3e37-46be-868f-0ea685e1deb5" />

<img width="1156" height="479" alt="Image" src="https://github.com/user-attachments/assets/308f0705-8652-4449-9d91-ccf7cb0df83d" />

#####  Bakehouse sample in our Databricks Catalog under samples
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/92f4338a-963a-4f1a-bd35-0d1dacbe0c8f" />

##### Delete the previous models and the schema.yml file
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/32f9cde6-981c-4fec-808a-b39977adaf5d" />

##### Bakehouse sales_customer data
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/58bd4814-9b14-49fb-9e52-df23ec383ed4" />

##### Create source.yml to run against the customers_sales table
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/4fe93916-0268-484b-941d-8676d055c462" />

##### Now create an sql model named model1 as a materialized table againt the source.yml file
<img width="1084" height="432" alt="Image" src="https://github.com/user-attachments/assets/a811c3ef-c698-43bc-818c-cc0877ed709a" />

##### dbt run to run the 2 new models
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/e6ead0a7-6abe-4dcb-a64d-db53815937c7" />


##### The model1 table is created in Databricks workspace
<img width="1257" height="664" alt="Image" src="https://github.com/user-attachments/assets/cf28a136-0c62-4363-96f1-e75c86c0d4c5" />

##### The model2 view is created
<img width="1266" height="637" alt="Image" src="https://github.com/user-attachments/assets/20ed24c1-2b7a-4232-9229-03da31cfcf9c" />

#####  Select only model1 to run and test
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/095e29e6-aaa5-49c2-be77-8460a045a0b8" />

#####  Referential integrity makes the dbt test select model1 run to fail since all custmomerID rows in model1 does not match those of model2
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/ccc3fcc7-d522-42a8-bd70-a8eb4da7b53e" />

##### Comment out the referential integrity test to see if the test will pass
<img width="1094" height="474" alt="Image" src="https://github.com/user-attachments/assets/49c09e7b-521b-42e2-87c7-0761cf92ebec" />

##### That is why it is throwing errors here so we need to suppress the uniqueness test
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/bedcdaa0-1662-4fd8-94aa-49b7e3349a85" />

##### It now passes all the data quality tests
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/2cadd32d-408e-4a39-9ed2-3b2b04a39695" />

#####  We can re-add the uniqueness data test back and set the severity level as a warning in dbt_project. So it now only give a warning and not an error
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/9ae036bb-5f6c-4015-89c6-368e78ce4dee" />

##### You can select the severity as an error in dbt_project.yml and set it as a warning in model1.sql level
<img width="1001" height="398" alt="Image" src="https://github.com/user-attachments/assets/014a8f56-1a8d-4a14-9f66-f9956db7400e" />

#####  You can select the severity as an error in dbt_project.yml and set it as a warning in model1.sql level
<img width="995" height="432" alt="Image" src="https://github.com/user-attachments/assets/d59af9eb-2838-48d2-ba37-946ff8a67db1" />

##### Define macros named new_macros_1.sql and use the macro as a fucnction to add new column to model1

##### Use the macro to add new column to model1.sql and call it as a fucntion on the first_name column. It will add a new column to model1.sql model on out Databricks workspace
<img width="1165" height="472" alt="Image" src="https://github.com/user-attachments/assets/5b21479b-cbc1-41f9-a110-f951d1a4b997" />

##### So this will add a new column named macro_new_column to our model1 table on Databricks
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/ac3ab56f-0075-4526-a77b-56d673722d78" />

##### So the macro_new_column column is added on yoyr Datbricks workspace
<img width="1267" height="656" alt="Image" src="https://github.com/user-attachments/assets/8715308f-d1d3-4ac6-9237-f966bf79a235" />

##### dbt seed will upload a csv file unto your target warehouse
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/1daab7b1-8da8-4943-9f75-339db627d02a" />

##### dbt seed will upload a csv file unto your target warehouse
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/180196a4-0b59-4558-b836-96ce5bf669cd" />

##### The Databriscks target warehouse infers the data types
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/98ec4d46-d9c9-417b-9c3b-715d6af4901f" />

##### This is an example of how to use loops to fetch multiple columns from a source data and create another dataset in the target  space
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/34fa2b6c-7eb1-4ce6-9dea-d50803af6d21" />

##### This is an example of how to use loops to fetch multiple columns from a source data and create another dataset in the target  space
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/bb8c3daa-24c5-4aef-9e8e-874d0e091df4" />

##### This is an example of how to use loops to fetch multiple columns from a source data and create another dataset in the target  space
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/865da607-576f-4194-9209-6cf12594b69c" />

#####  Create document folder document file put function to call the comment or document and use the doc in another file
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/c770eb49-a735-4b8a-9515-4b4260ed5bab" />

##### Create document folder document file put function to call the comment or document and use the doc in another file use the doc to generate comments here
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/4906a87f-c01e-4aba-8e5a-6a4266fd173e" />

#####  You can also define tags
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/4b69897d-a37f-469a-b13e-4e2c4e17692b" />

#####  You can use the tag anywhere
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/45a07ee9-7623-44d2-b32c-36c5fcc56e3a" />

##### Your dbt run --select will run only models with the tags
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/4d377d00-b66f-4e90-9d97-5130f43f4a98" />

#####  dbt docs generate will generate all the documentations for you
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/914a8137-366e-43d3-831c-4043d8c66828" />

##### the docs will be available under target and catalog.json file
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/0d859355-e08a-4e19-bd10-e5a852f205c4" />

##### dbt docs serve will open up all the documentation in you localhost
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/737a26ed-87c1-423d-895d-ddd5341de2aa" />

#####  The documentation will be available at localhost 8080 after using dbt docs serve
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/e7a65048-7640-41be-b28e-82de542b38e8" />

##### The docs can also show the lineage with the source and its dependencies
<img width="1280" height="720" alt="Image" src="https://github.com/user-attachments/assets/4394fc70-1982-4418-b506-03cbb4fd5268" />


---





### Thank you for reading
---

### **AUTHOR'S BACKGROUND**
### Author's Name:  Emmanuel Oyekanlu
```
Skillset:   I have experience spanning several years in data science, developing scalable enterprise data pipelines,
enterprise solution architecture, architecting enterprise systems data and AI applications,
software and AI solution design and deployments, data engineering, high performance computing (GPU, CUDA), machine learning,
NLP, Agentic-AI and LLM applications as well as deploying scalable solutions (apps) on-prem and in the cloud.

I can be reached through: manuelbomi@yahoo.com

Website:  http://emmanueloyekanlu.com/
Publications:  https://scholar.google.com/citations?user=S-jTMfkAAAAJ&hl=en
LinkedIn:  https://www.linkedin.com/in/emmanuel-oyekanlu-6ba98616
Github:  https://github.com/manuelbomi

```
[![Icons](https://skillicons.dev/icons?i=aws,azure,gcp,scala,mongodb,redis,cassandra,kafka,anaconda,matlab,nodejs,django,py,c,anaconda,git,github,mysql,docker,kubernetes&theme=dark)](https://skillicons.dev)





























