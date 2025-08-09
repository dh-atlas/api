# DH ATLAS REST API (v0.0.1) — RAMOSE Configuration

This repository contains the initial configuration for generating a REST API for the DH ATLAS project using [RAMOSE](https://github.com/opencitations/ramose).

> ⚠️ Version 0.0.1 unofficial: the API currently works only in a local environment (`localhost`).

---

## 📘 Description

DH ATLAS is a knowledge graph that maps international research on Italian digital cultural heritage. This REST API allows querying the graph via SPARQL endpoints, offering semantic and statistical operations over the data.

The configuration is defined in the `test.hf` file, compatible with RAMOSE.

---

## 📂 Repository v0.0.1 Contents

- `test.hf`: RAMOSE configuration file for the REST API.
- `README.md`: this file.
- `start.sh`: script to launch RAMOSE.
- `requirements.txt`: Python dependencies.

---

## 🚀 Local Setup

1. Clone the repository:
   ```bash
   git clone https://github.com/dh-atlas/api.git
   cd dh-atlas-api
   ```
2. Create and activate a virtual environment (recommended):
   ```bash
   python3 -m venv env
   source env/bin/activate
   ```
3. Install RAMOSE and dependencies: RAMOSE is available on PyPI. You can install it with:
   ```bash
   pip install ramose
   ```
If you want to install from source (e.g., for development), clone the RAMOSE repository and run:
```bash
git clone https://github.com/opencitations/ramose.git
cd ramose
pip install -r requirements.txt
python setup.py install
```
4. Run the API locally: From the root of your DH ATLAS API repository:
   ```bash
   ramose test.hf
   ```
5. Access the API documentation: Open your browser and go to:
   ```bash
   http://127.0.0.1:8081/api/v1
   ```
---

## 🔍 Available Operations implemented by the API

/records
Returns metadata for each dataset in the DH ATLAS graph.

/labels/{pid}
Returns the human-readable label of a resource.

/classes/{pid}
Returns the RDF classes associated with a resource.

/stats/entities
Returns the total number of distinct entities.

/stats/projects
Returns the number of research projects.

/stats/products
Returns the number of research products.

/stats/organizations
Returns the number of organizations.

/stats/persons
Returns the number of researchers.

/stats/organizations/by-location
Returns organizations grouped by geographical location.

/stats/products/by-additional-type
Returns research products grouped by type.

/stats/products/by-organization
Returns research products grouped by producing organization.

---

## 📜 License

The .hf configuration file is licensed under CC BY 4.0.
The API is generated using RAMOSE, licensed under the ISC License.
All returned data is made available under CC0.

