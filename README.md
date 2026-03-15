<div align="center">

# 🌾 Farmer Market Analysis 🌾

**Assignment-3** — Exploratory Data Analysis of Farmers' Market Observations

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue?logo=python&logoColor=white)](https://www.python.org/)
[![Jupyter Notebook](https://img.shields.io/badge/Jupyter-Notebook-orange?logo=jupyter)](https://jupyter.org/)
[![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-150458?logo=pandas&logoColor=white)](https://pandas.pydata.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Last Commit](https://img.shields.io/github/last-commit/kirmanioussema12/Farmer_Market_Analysis?color=green)](https://github.com/kirmanioussema12/Farmer_Market_Analysis/commits/main)

</div>

---

## 📖 Project Overview

This project performs **exploratory data analysis (EDA)** on a dataset of **Farmers' Market observations**. It aims to uncover patterns, trends, and insights about farmers' markets — such as product availability, geographic distribution, operational characteristics, customer observations, or seasonal variations (depending on the columns in the dataset).

The analysis is implemented in a single, well-documented **Jupyter Notebook** — perfect for students, researchers, data enthusiasts, or anyone interested in agriculture economics, local food systems, or market trends.

**Main Goals:**
- Clean and explore the `Farmers' Market Observation-dataset.csv`
- Visualize key variables and relationships
- Draw meaningful conclusions about farmers' markets

---
## 📊 Key Dataset Columns

The dataset contains **vendor-level observations** from farmers' markets in the Greater Toronto Area (mostly Toronto and York region), collected in 2018. Each row represents one vendor stall visit with detailed food safety, hygiene, and handling notes.

Below are the **most important columns** for analysis:

| Column Name                  | Data Type    | Description                                                                 | Importance / Why Analyzed                                      | Typical Values / Notes                              |
|------------------------------|--------------|-----------------------------------------------------------------------------|----------------------------------------------------------------|-----------------------------------------------------|
| market_ID                    | Integer      | Unique identifier for the farmers' market                                   | Groups observations by market; used for aggregation            | 1, 2, 3, ..., 60                                    |
| vendor_ID                    | Float        | Unique identifier for the vendor/stall within a market                      | Primary key per observation                                    | 1.0, 2.0, ..., 454.0                                |
| City                         | String       | City where the market is located                                            | Geographic segmentation (Toronto vs suburbs)                   | Toronto, North York, Etobicoke, Aurora, Newmarket, etc. |
| Health_unit                  | String       | Public health unit responsible (all Toronto in sample)                      | Regulatory context                                             | Toronto, York                                       |
| Yearround                    | String       | Is the market open year-round?                                              | Seasonal vs permanent markets                                  | Yes / No                                            |
| Visit_date                   | Date (str)   | Date of the observation visit                                               | Time-based trends, seasonality                                 | 2018-06-07 to 2018-09-19 (summer–early fall 2018)   |
| Visit_month / Visit_day      | String       | Month and weekday of visit                                                  | Patterns by day of week or month                               | June–September, Thursday–Sunday                     |
| Location                     | String       | Indoor / outdoor setting                                                    | Environmental factors affecting food safety                    | Outdoors only, Indoors only                         |
| Public_private               | String       | Ownership type of market location                                           | Public vs private property differences                         | Publicly owned property..., Privately owned...      |
| no_vendors                   | Integer      | Total number of vendors at the market that day                              | Market size / busyness                                         | 2–28                                                |
| no_TCSfood_vendors           | Integer      | Number of vendors selling TCS foods                                         | Exposure to higher-risk foods                                  | 1–22                                                |
| communal_sinks / electrical_access | String | Availability of shared sinks / electricity                                  | Infrastructure quality                                         | Yes / No                                            |
| pets_visible / pet_signs     | String       | Pets seen or pet signs posted                                               | Animal presence & contamination risk                           | Yes / No                                            |
| vendor_roles                 | String       | Vendor's work role(s)                                                       | Single vs multiple roles (potential workload impact)           | Single work roles / Multiple work roles             |
| TCS_rawmeat / TCS_rtemeat / TCS_preparedfood / TCS_produce / ... | Float | Binary (0/1) — sells this TCS food category                                 | Risk profile of products sold                                  | 0.0 = No, 1.0 = Yes (many categories)               |
| Visible_handwashing_station / Visible_sanitizer | Float | Visibility of hygiene stations                                              | Hand hygiene opportunity                                       | 0.0 / 1.0                                           |
| Clean_contact_surfaces / Food_off_ground | String | Vendor stall cleanliness & food protection                                  | Basic food safety practices                                    | Yes / No                                            |
| Protect_sneeze / Protect_mesh / Protect_plastic / ... | Float | Sneeze / cross-contamination protection methods                             | Physical barriers against contamination                        | 0.0 / 1.0 (multiple columns)                        |
| Coldholding_foods_sold / Cold_units / Cold_coolers / ... | String / Float | Cold-holding practices & equipment                                          | Temperature control for perishable foods                       | Yes/No + equipment types + N/a                      |
| Hotholding_foods_sold / Hot_units / ... | String / Float | Hot-holding practices                                                       | Temperature control for hot foods                              | Mostly N/a in this dataset                          |
| Food_samples / TCSfood_samples / Sample_types | String | Offering food samples? What kind?                                           | Sampling risk & single-serve usage                             | Yes/No + types (e.g. Cheeses, Cured meat)           |
| gender                       | String       | Observed gender of primary vendor                                           | Demographic patterns (exploratory)                             | Male / Female                                       |
| vendor_cleanattire / vendor_hair / vendor_healthy | String | Vendor personal hygiene (clothing, hair restraint, apparent health)        | Personal hygiene compliance                                    | Yes / No                                            |
| beh_handle_food / beh_cash / beh_touchface / ... | Float | Count of observed unsafe behaviors (0–several)                              | Critical hygiene violation counts                              | 0.0–several (higher = more issues)                  |
| transactions                 | Float        | Observed number of customer transactions during visit                       | Vendor busyness / sales activity                               | 1.0–10.0+                                           |
| handwashing_attempts / handwashing_adequate | Float | Handwashing observed (attempts & adequacy)                                  | Hand hygiene compliance                                        | 0.0–several                                         |
| Handle_hands / Handle_gloves / Handle_tongs / ... | Float | Method used to handle ready-to-eat food                                     | Best practice usage (gloves/tongs > bare hands)                | 0.0 / 1.0                                           |

**Notes:**
- Most food-safety-related fields are **binary (0.0/1.0)** or **Yes/No**.
- Many "N/a - no …" strings appear when a condition doesn't apply (e.g., no hot foods sold → N/a for hot holding).
- **beh_*** columns count risky behaviors — higher values indicate poorer practices.
- Dataset covers **summer 2018 visits** in Ontario (Toronto/York area), mostly outdoor public markets.
- Full list has **87 columns** — many are product-specific TCS flags or detailed protection methods.

You can explore all columns by running:
```python
import pandas as pd
df = pd.read_csv("Farmers' Market Observation-dataset.csv")
print(df.columns.tolist())
print(df.info())
```
---


## 🛠️ Technologies & Libraries Used

| Technology / Library     | Version (approx.) | Purpose                              | Badge / Link                                      |
|--------------------------|-------------------|--------------------------------------|---------------------------------------------------|
| Python                   | ≥ 3.8             | Core programming language            | ![Python](https://img.shields.io/badge/Python-3.8+-blue) |
| Jupyter Notebook         | Latest            | Interactive analysis & visualization | ![Jupyter](https://img.shields.io/badge/Jupyter-orange) |
| pandas                   | Latest            | Data manipulation & cleaning         | ![Pandas](https://img.shields.io/badge/Pandas-150458) |
| numpy                    | Latest            | Numerical computations               | —                                                 |

Full list of dependencies → [`requirements.txt`](requirements.txt)

---

## 📁 Project Structure

```text
Farmer_Market_Analysis/
├── Assignment-3.ipynb          # Main analysis notebook (EDA + visualizations)
├── Farmers' Market Observation-dataset.csv   # Raw dataset
├── requirements.txt            # Python dependencies
└── README.md                   # This file (you're reading it!)
```
---

## 🚀 Getting Started
1. Clone the Repository
```bash
git clone https://github.com/kirmanioussema12/Farmer_Market_Analysis.git
cd Farmer_Market_Analysis
```
2. Install Dependencies
```bash
python -m venv venv
source venv/bin/activate    # Linux / macOS
venv\Scripts\activate       # Windows
pip install -r requirements.txt
```




