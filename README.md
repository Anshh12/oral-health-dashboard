# 🦷 OralHealth Analytics Pro

A comprehensive Streamlit dashboard for analyzing oral health survey data (WHO Oral Health Survey format).

## Features

- **13 Analysis Tabs** covering demographics, household SES, oral hygiene, dental services, caries/DMFT, periodontal health, dental conditions, TMJ, oral lesions, treatment needs, examiner analysis, cluster analysis, and raw data explorer
- **Dynamic Column Detection** — auto-detects relevant columns from any REDCap CSV export
- **Interactive Filters** — filter by date, age group, gender, cluster, and examiner
- **Clinical Indices** — automated DMFT/dmft calculation with WHO benchmarks
- **Examiner & Cluster Reports** — downloadable CSV reports
- **Professional UI** — custom-styled with Plotly interactive charts

## Quick Start

```bash
pip install -r requirements.txt
streamlit run "patient_dashboard (2).py"
```

## Data Format

Upload a CSV exported from REDCap with the "Labels" option. The dashboard expects:
- **Household rows**: `Repeat Instrument` is empty
- **Participant rows**: `Repeat Instrument` = "Participant"

## Documentation

See [DASHBOARD_DOCUMENTATION.md](DASHBOARD_DOCUMENTATION.md) for complete architecture, tab descriptions, and chart analysis explanations.

## Tech Stack

| Component | Technology |
|-----------|------------|
| Framework | Streamlit |
| Charts | Plotly |
| Data | Pandas, NumPy |
| Styling | Custom CSS |
