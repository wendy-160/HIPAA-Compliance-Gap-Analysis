# HIPAA-Compliance-Gap-Analysis
A data-driven analysis of 6,000+ healthcare data breaches reported to the U.S. Department of Health and Human Services (HHS) Office for Civil Rights, identifying which HIPAA safeguard categories represent the greatest systemic risk to patient data.

**Finding:** Technical Safeguard Failure accounts for 60.5% of all reported breaches but 91.6% of all individuals affected. This made it the sole disproportionate-risk category in the dataset and the clearest priority. 

Full write-up: (reports/HIPAA_Breach_Control_Gap_Analysis.pdf)

## Motivation
Breach counts alone can be misleading. A category that causes many small incidents can look worse on paper than one that causes fewer, catastrophic ones. This project reframes HIPAA breach data by prioritizing **impact over frequency**, mapping each reported breach to its most likely underlying safeguard failure (Administrative, Physical, or Technical) and comparing how often each occurs against how many people it actually affects. 

The dataset covers breaches affecting 500+ individuals submitted between 2013 and 2026 and is sourced directly from HHS OCR's public "Wall of Shame" breach portal. 

## Project Structure
```
HIPPA-Compliance-Gap-Analysis/
|
├── data/
|     ├── raw/
|     |     ├── breach_report.csv                # Original
|     └── processed/
|           ├── breach_report_cleaned.csv        # notebook 1 output
|           ├── breach_report_features.csv       # notebook 2 output
|           └── gap_summary.csv                  # notebook 3 output
├── notebooks/
|     ├── 01_exploration.ipynb                   # Data cleaning & EDA
|     ├── 02_feature_engineering.ipynb           # Feature creation & control gap mapping
|     └── 03_analysis.ipynb                      # Frequency, impact, trend, & gap analysis
├── reports/
|     ├── figures /                              # Exported Chart Images
|     ├── HIPAA_Breach_Control_Gap_Analysis.pdf  # Summary write up
|     └── key_findings.txt                       # 11 findings from notebook 3
└── README.md
```

## Methodology
**1. Exploration (`01_exploration.ipynb`)**
Cleans the raw HHS OCR export - resolves missing values (Web Description, State, Covered Entity Type, Individuals Affected), and profiles breach type, location, entity type, and territory distribution. 

**2. Feature Engineering (`02_feature_engineering.ipynb`)**
Multi-hot encodes breach type and location fields (which are comma-separated in the source data), maps each category to its most likely HIPAA safeguard failure (Administrative / Physical / Technical), flags business associate involvement, and tiers breach size by order of magnitude.  

**3. Analysis (`03_analysis.ipynb`)**
Four-part analysis:
- **Frequency Analysis** - which breach types and locations are most common
- **Impact Analysis** - individuals affected by breach type, entity type, and BA involvement (with statistical testing)
- **Trend Analysis** - how breach types have shifted year over year
- **Gap Identification** - comparing frequency share vs. impact share by safeguard category to flag disproportionate risk

## Key Findings
![TechnicalFailure](reports/figures/03_4b_gap_summary_table_Technical_Safeguard_Failure.png)
![AdministrativeFailure](reports/figures/03_4b_gap_summary_table_Administrative_Safeguard_Failure.png)

- Hacking/IT Incidents grew from 10.4% of annual breaches in 2013 to ~80% by 2022-2024, while physical breach types (theft, loss, improper disposal) declined steadily over the same period
- Breaches involving a business associate affect significantly more individuals (median 4,500) than those without (median 3,859).
- Risk composition varies by entity type: Business Associates skew heavily toward Technical Safeguard Failure (65.3%), while Health Plans show a notably higher share of Administrative Safeguard Failure (38.6%).

Full Findings, notes, and recommendations are in the summary and `key_findings.txt`

## Tech Stack
- **Python:** pandas, numpy, matplotlib, seaborn, scipy
- **Jupyter**
- Data source: HHS OCR Breach Portal

## Data Limitations
- Breaches can take up to 24 months to be investigated and finalized by OCR, so 2024-2026 counts likely understate the true totals for those years.
- Safeguard category mappings are analytical approximations based on the most likely control failure implied by each breach type/location. They are not official OCR determinations.
