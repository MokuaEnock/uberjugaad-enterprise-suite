# UberJugaad Enhanced SALT Dataset

## About Dataset

**UberJugaad Enhanced SALT Dataset** is an enterprise dataset containing:

* **1.9 million ERP transactions**
* **151,673 business emails**
* **3,499 supporting documents**

It represents a realistic business ecosystem from **UberJugaad GmbH**, a fictional **€14.8B German industrial supplier**, and is built on SAP’s **SALT** dataset, enhanced with synthetic business communications for AI/ML applications.

File descriptions, feature descriptions, and column details are documented in the accompanying `.md` summary files.

---

# Dataset Structure & Size

| File                           | Records |    Size | Description                                                |
| ------------------------------ | ------: | ------: | ---------------------------------------------------------- |
| `all_communications.parquet`   | 151,673 |  6.9 MB | Business emails with realistic subjects, bodies, sentiment |
| `erp_transactions.parquet`     |    1.9M | 40.8 MB | Complete SAP sales transactions                            |
| `supporting_documents.parquet` |   3,467 |  0.1 MB | Purchase orders, invoices, shipping notices                |
| `business_documents.parquet`   |      32 |   22 KB | Meeting agendas, quality reports                           |
| `uberjugaad_email.db`          |   151K+ |   76 MB | SQLite email database with contacts                        |

**Total Dataset Size:** ~160 MB

---

# Key Features & Use Cases

## What Makes This Special

* **Realistic Business Ecosystem**: Complete email threads, documents, and transactions all linked together
* **Natural Language Content**: 151K emails with authentic business communication patterns
* **Linked Data Architecture**: All files connected via order numbers and customer IDs
* **Time-Synchronized**: Chronologically consistent data from 2019–2020
* **Discovery-Oriented**: Business patterns embedded in content, not pre-labeled

## Perfect For

* Email classification & routing
* Sentiment & urgency detection
* Customer behavior analysis
* Document information extraction
* Business process mining
* Anomaly detection
* Multi-modal AI combining text and structured ERP data

---

# Company Profile: UberJugaad GmbH

* **Industry:** Industrial B2B Distribution & Manufacturing
* **Scale:** €14.8B annual revenue, 7,000–8,500 employees
* **Operations:** 153 locations across 203 countries
* **Customers:** 13,155 active customers (139,611 total)
* **Products:** 164,358 SKUs in industrial supplies and components

## Business Model

* **Distributors (114):** €4.0B revenue — resellers with high-volume automated ordering
* **Small Business (2,921):** €5.3B revenue — regular procurement needs
* **Manufacturers (193):** €1.2B revenue — consistent production supply requirements
* **Service Companies (867):** €1.6B revenue — emergency repair and maintenance
* **Dealers (110):** €1.7B revenue — channel partners with large batch orders

---

# Email Corpus Highlights

## Communication Types

* Customer business emails: order issues, complaints, urgent requests
* Internal escalations: sales team coordination, problem resolution
* Vendor communications: supply chain updates, delivery notifications
* Spam/marketing: realistic vendor pitches
* HR announcements: company policies, holiday notices
* IT support: help desk tickets, system issues

## Sample Email Thread

* **Customer → Sales:** “Order 0002456789 arrived damaged, production line at risk”
* **Sales → Logistics:** “URGENT: Major delivery failure, customer threatening €2.3M pullout”
* **Logistics → Customer:** “Expedited replacement shipping today, compensation proposal attached”

## Sentiment & Urgency Distribution

* **Positive:** 45,501 emails (30%)
* **Neutral:** 83,004 emails (55%)
* **Negative:** 23,168 emails (15%)
* **Urgency Levels:** 0–5 scale, with 23% marked as urgent (3+)

---

# Document Types

## Supporting Documents (3,467)

* Purchase orders
* Invoices
* Shipping notices
* Quality reports
* Credit memos

## Business Documents (32)

* Meeting agendas
* Quality metrics
* Overdue reports
* Vendor scorecards

---

# ERP Transaction Data

## Core Tables

* **Sales Documents:** 412K orders with customer and shipping details
* **Sales Items:** 1.9M line items with products and quantities
* **Complete Pricing:** unit prices, line amounts, discounts, taxes
* **Multi-Currency:** EUR (79%), USD (7%), GBP (4%) transactions

## Business Patterns

* **Order Timing:** 89.9% during business hours, 99.2% weekdays
* **Seasonal Trends:** peak activity Tuesday–Thursday
* **Customer Personas:** from €3K occasional buyers to €40M distributors
* **Product Mix:** from €27 commodity items to €10K specialized equipment

---

# Data Relationships

All data is interconnected through these key fields:

* `SALESDOCUMENT` — links transactions to emails and documents
* `customer_id = SOLDTOPARTY` — customer linkage across all files
* `order_number` — references in email content and document headers
* `timestamp` — synchronized chronological ordering

---

# Column Descriptors & Feature Descriptions

## `all_communications.parquet` (19 columns)

```text
message_id, timestamp, from, to, from_name, to_name, from_role, to_role,
subject, body, customer_id, customer_name, cc, triggered_by,
department_from, department_to, department, vendor, from_company
```

## `erp_transactions.parquet` (14 columns)

```text
SALESDOCUMENT, SOLDTOPARTY, PRODUCT, PLANT, CREATIONDATE, SALESOFFICE,
SALESGROUP, CUSTOMERPAYMENTTERMS, SALESORGANIZATION, DISTRIBUTIONCHANNEL,
TRANSACTIONCURRENCY, BILLINGCOMPANYCODE, CUMULATIVEORDERQUANTITY, NETAMOUNT
```

## `sales_documents.parquet` (7+ columns)

```text
SALESDOCUMENT, SALESDOCUMENTTYPE, CREATIONDATE, CREATIONTIME,
SALESORGANIZATION, DISTRIBUTIONCHANNEL, DIVISION
```

## `sales_items.parquet` (8+ columns)

```text
SALESDOCUMENT, SALESDOCUMENTITEM, PRODUCT, ORDERQUANTITY, NETPRICE,
NETAMOUNT, PLANT, SHIPPINGPOINT
```

## `supporting_documents.parquet` (30+ columns)

```text
document_type, document_id, order_number, customer_id, invoice_number,
ship_date, carrier, tracking_number, ...
```

## `business_documents.parquet` (6 columns)

```text
document_type, document_id, created_date, created_by, title, content
```

---

# Research Applications

## Academic Use Cases

* NLP research on business communication corpora
* Process mining and workflow discovery
* Anomaly detection in enterprise communication
* Multi-modal learning with text + structured data
* Temporal analysis of business communications

## Industry Applications

* Customer service automation
* Fraud detection
* Process optimization
* Predictive analytics
* Document AI

---

# Quick Start Code

```python
import pandas as pd
import sqlite3

# Load the main datasets
emails = pd.read_parquet("all_communications.parquet")
transactions = pd.read_parquet("erp_transactions.parquet")
documents = pd.read_parquet("supporting_documents.parquet")

# Find urgent customer issues
urgent = emails[(emails["urgency"] >= 4) & (emails["sentiment"] == "negative")]
print(f"Found {len(urgent)} urgent customer issues")

# Link emails to specific orders
order = "0002315309"
order_emails = emails[emails["body"].str.contains(order, na=False)]
order_docs = documents[documents["order_number"] == order]

# Query the email database
conn = sqlite3.connect("uberjugaad_email.db")
contacts = pd.read_sql(
    "SELECT * FROM contacts WHERE contact_type = 'external'",
    conn
)
print(f"External contacts: {len(contacts):,}")
```

---

# Data Quality & Ethics

## Quality Assurance

* **Consistency:** All relationships validated across files
* **Completeness:** Minimal missing values, comprehensive coverage
* **Accuracy:** Realistic business patterns and workflows
* **Timeliness:** 2019–2020 timeframe with logical chronological order

## Ethical Considerations

* **Synthetic Data:** All personal information is generated
* **Privacy Compliant:** No actual customer or employee data included
* **Research Purpose:** Designed for academic and commercial AI/ML development
* **SAP Licensed:** Built on officially released SAP SALT dataset

---

# Why Choose This Dataset

* **Scale:** 151K+ emails provide statistical significance for ML models
* **Realism:** Authentic business communication patterns and scenarios
* **Integration:** Linked emails, transactions, and documents tell complete stories
* **Diversity:** Multiple communication types, personas, and business scenarios
* **Ready-to-Use:** Clean, structured data with comprehensive documentation
* **Research-Grade:** Suitable for academic publication and commercial development

---

# Citation

```bibtex
@dataset{uberjugaad_enhanced_salt_2024,
  title={UberJugaad Enhanced SALT Dataset: Enterprise Communications and Transactions},
  author={UberJugaad GmbH},
  year={2024},
  publisher={Kaggle},
  note={Enhanced version of SAP SALT dataset with business communications}
}
```

---

# File Descriptions

## Documentation

* `README.md` — Dataset overview and structure
* `columns.md` — Detailed column descriptions for all files
* `quickstart.md` — Getting started guide
* `sample_communications.md` — Real email examples
* `business_docs.md` — Document conversion to PDF/HTML
* `kaggle_starter_notebook.py` — Python analysis code

## Files Included

* `all_communications.parquet` — 151,673 emails
* `erp_transactions.parquet` — 1.9M transactions
* `sales_documents.parquet` — 243K headers
* `sales_items.parquet` — 1.9M line items
* `supporting_documents.parquet` — 3,467 documents
* `business_documents.parquet` — 32 reports
* `uberjugaad_email.db` — SQLite database with emails and contacts

---

# Perfect For

* Email classification & sentiment analysis
* Customer behavior prediction
* Document information extraction
* Business process mining
* Multi-modal ML (text + structured data)

> No pre-labeled sentiment or patterns — this is a discovery-oriented dataset.

---

# Author

**Patrick Rutledge**
Enterprise systems specialist and independent data scientist who created the fictional UberJugaad GmbH company and enhanced dataset for AI/ML research.

---

# Project Description

This dataset transforms SAP’s SALT transactional tables into a full enterprise simulation by layering realistic emails, documents, and embedded patterns.

All enhancements are synthetic but modeled on real-world communication and ERP logic.

* **GitHub:** `PatrickRutledge/uberjugaad-enhanced-salt`
* **Development Year:** 2025

---

# Citation Details

If you use this dataset, cite both the enhanced version and the original SALT dataset.

## Enhanced Dataset

**Rutledge, P. (2025). UberJugaad Enhanced SALT Dataset. Kaggle.**

```text
https://www.kaggle.com/datasets/PatrickRutledge/uberjugaad-enhanced-salt-dataset
```

```bibtex
@dataset{rutledge2025uberjugaad,
  title={UberJugaad Enhanced SALT Dataset},
  author={Rutledge, Patrick},
  year={2025},
  publisher={Kaggle},
  url={https://www.kaggle.com/datasets/PatrickRutledge/uberjugaad-enhanced-salt-dataset}
}
```

## Original SALT Dataset

**Klein, T., Biehl, C., Costa, M., Sres, A., Kolk, J., & Hoffart, J. (2024).**
*SALT: Sales Autocompletion Linked Business Tables Dataset.*
NeurIPS 2024 Third Table Representation Learning Workshop.

```text
https://openreview.net/forum?id=UZbELpkWIr
```

```bibtex
@inproceedings{klein2024salt,
  title={{SALT}: Sales Autocompletion Linked Business Tables Dataset},
  author={Klein, Tassilo and Biehl, Clemens and Costa, Margarida and Sres, Andre and Kolk, Jonas and Hoffart, Johannes},
  booktitle={NeurIPS 2024 Third Table Representation Learning Workshop},
  year={2024},
  url={https://openreview.net/forum?id=UZbELpkWIr}
}
```

---

# License

* **MIT License** for enhanced content
* **CC-BY-NC-SA-4.0 / SAP SALT license** for original SALT data depending on distribution context

---

# Tags

* Business
* Tabular
* NLP
* Multimodal
* Multilabel Classification
* Text Data
* Classification
* Sentiment Analysis
* Time Series
* Customer Analytics
* Sales Analytics
* Customer Churn
* Text Mining
* Business Intelligence
* Predictive Analytics
* Data Preprocessing
