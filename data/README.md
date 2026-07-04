# UberJugaad Enhanced SALT Dataset

## Overview
**1.9 million ERP transactions + 151,673 business emails + 3,499 supporting documents**

Comprehensive enterprise dataset from UberJugaad GmbH, a EUR 14.8B German industrial supplier with 1,467 employees serving 15,606 customers. Enhanced version of the SAP SALT dataset with realistic business communications and documents.

## Dataset Structure

```
UberJugaad Enhanced SALT Dataset/
│
├── 📊 CORE DATA FILES (83.5 MB)
│   ├── all_communications.parquet     [151,673 emails - 6.9 MB]
│   ├── erp_transactions.parquet       [1.9M transactions - 40.8 MB]
│   ├── sales_documents.parquet        [243K headers - 7.1 MB]
│   ├── sales_items.parquet            [1.9M line items - 28.4 MB]
│   ├── supporting_documents.parquet   [3,467 documents - 0.1 MB]
│   └── business_documents.parquet     [32 reports - 22 KB]
│
├── 📧 EMAIL DATABASE (76 MB)
│   └── uberjugaad_email.db           [SQLite: 151K emails + 19K contacts]
│
├── 📖 DOCUMENTATION
│   ├── README.md                      [This file - overview]
│   ├── columns.md                     [Column descriptions]
│   ├── quickstart.md                  [Getting started guide]
│   ├── sample_communications.md       [Email examples]
│   ├── business_docs.md              [Document conversion guide]
│   └── dataset-metadata.json         [Kaggle metadata]
│
├── 💻 CODE EXAMPLES
│   └── uberjugaad-gmbh-notebook-for-data-exploration.ipynb    [Sample analysis code]
│
└── ⚖️ LICENSE                        [MIT License]
```

*Note: All files are in the root directory for easy access. This diagram shows logical groupings.*

## Dataset Contents

| Category | Files | Total Size | Description |
|----------|-------|------------|-------------|
| **Core Data** | 6 parquet files | 83.5 MB | ERP transactions, emails, documents |
| **Email Database** | 1 SQLite file | 76 MB | Complete email system backup |
| **Documentation** | 6 files | < 1 MB | Guides, examples, metadata |
| **Code** | 1 Python file | < 1 MB | Starter notebook |

## Key Features
- **Realistic business ecosystem**: Complete email threads, documents, and transactions
- **Natural language content**: 151K emails with realistic subjects and bodies
- **Linked data**: All files connected via SALESDOCUMENT and customer_id
- **Time-synchronized**: Chronologically consistent 2019-2020 data
- **Discovery-oriented**: Patterns embedded in content, not labels

## Use Cases
- Email classification and routing
- Sentiment and urgency detection
- Customer behavior analysis
- Document information extraction
- Business process mining
- Anomaly and pattern detection
- Natural language understanding
- Multi-modal analysis (text + structured data)

## Quick Start

```python
import pandas as pd

# Load emails
emails = pd.read_parquet('all_communications.parquet')
print(f"Total emails: {len(emails):,}")

# Load transactions
transactions = pd.read_parquet('erp_transactions.parquet')
print(f"Total orders: {len(transactions):,}")

# Find emails mentioning specific orders
order = "0002315309"
order_emails = emails[emails['body'].str.contains(order, na=False)]
print(f"Emails about order {order}: {len(order_emails)}")

# Query email database
import sqlite3
conn = sqlite3.connect('uberjugaad_email.db')
cursor = conn.cursor()
cursor.execute("SELECT COUNT(*) FROM contacts WHERE contact_type = 'external'")
print(f"External contacts: {cursor.fetchone()[0]:,}")
conn.close()
```

## File Descriptions

### Core Data Files
- **all_communications.parquet**: All business emails with subjects, bodies, and metadata
- **erp_transactions.parquet**: SAP sales transactions with customer and product data
- **sales_documents.parquet**: Sales document headers from SAP
- **sales_items.parquet**: Detailed line items for each transaction
- **supporting_documents.parquet**: Business documents (POs, invoices, shipping notices)
- **business_documents.parquet**: Meeting agendas and reports

### Email Database
- **uberjugaad_email.db**: SQLite database simulating Exchange backup with emails, contacts, folders

### Documentation
- **README.md**: Dataset overview and structure (this file)
- **columns.md**: Detailed column descriptions for all files
- **quickstart.md**: Getting started guide with code examples
- **sample_communications.md**: Example emails showing variety and realism
- **business_docs.md**: Guide to converting documents to PDF/HTML
- **dataset-metadata.json**: Kaggle dataset configuration

### Code
- **uberjugaad-gmbh-notebook-for-data-exploration.ipynb**: Jupyter notebook for comprehensive data exploration and analysis


## Author
**Patrick Rutledge**
Enterprise systems specialist and independent data scientist created the fictional UberJugaad GmbH company and enhanced dataset for AI/ML research.

## Project Description
This dataset transforms SAP's SALT transactional tables into a full enterprise simulation by layering realistic emails, documents, and embedded patterns.
All enhancements are synthetic but modeled on real-world communication and ERP logic.

**GitHub**: [PatrickRutledge/uberjugaad-enhanced-salt](https://github.com/PatrickRutledge/uberjugaad-enhanced-salt)
**Development Year**: 2025

---

## Citation
If you use this dataset, please cite both the enhanced version and the original SALT dataset:

**Enhanced Dataset**
Rutledge, P. (2025). *UberJugaad Enhanced SALT Dataset*. Kaggle.
[https://www.kaggle.com/datasets/PatrickRutledge/uberjugaad-enhanced-salt-dataset](https://www.kaggle.com/datasets/PatrickRutledge/uberjugaad-enhanced-salt-dataset)

```bibtex
@dataset{rutledge2025uberjugaad,
  title={UberJugaad Enhanced SALT Dataset},
  author={Rutledge, Patrick},
  year={2025},
  publisher={Kaggle},
  url={https://www.kaggle.com/datasets/PatrickRutledge/uberjugaad-enhanced-salt-dataset}
}
```

**Original SALT Dataset (Required)**
Klein, T., Biehl, C., Costa, M., Sres, A., Kolk, J., & Hoffart, J. (2024). *SALT: Sales Autocompletion Linked Business Tables Dataset*. NeurIPS 2024 Third Table Representation Learning Workshop.

```bibtex
@inproceedings{klein2024salt,
  title={{SALT}: Sales Autocompletion Linked Business Tables Dataset},
  author={Klein, Tassilo and Biehl, Clemens and Costa, Margarida and Sres, Andre and Kolk, Jonas and Hoffart, Johannes},
  booktitle={NeurIPS 2024 Third Table Representation Learning Workshop},
  year={2024},
  url={https://openreview.net/forum?id=UZbELpkWIr}
}
```

## License

MIT License for enhanced content / SAP Sample Code License for original SALT data

## Acknowledgments

Built on SAP's SALT dataset. Enhanced with synthetic business communications for AI/ML applications. The original SALT authors are not affiliated with these enhancements.
