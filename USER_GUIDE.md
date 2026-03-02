# 🦷 OralHealth Analytics Pro — Complete User Guide

## 1. Getting Started

### 1.1 System Requirements
- **Python** 3.9 or higher
- **Browser**: Chrome, Firefox, or Edge (latest version)
- **RAM**: Minimum 4 GB (8 GB recommended for large datasets)

### 1.2 Installation
```bash
# Install dependencies
pip install -r requirements.txt

# Run the dashboard
streamlit run "patient_dashboard (2).py"
```
The dashboard opens at **http://localhost:8501**

### 1.3 Uploading Data
1. Click **"📁 Upload CSV"** in the left sidebar
2. Select your REDCap CSV export (use the **"Labels"** export option from REDCap)
3. The dashboard auto-detects all columns and loads instantly

> **Data Privacy:** Your CSV data is processed entirely in your browser session. It is never stored on any server. When you close the tab, the data is gone.

---

## 2. Sidebar Filters (Global)

All filters apply to **every tab simultaneously**. Changing a filter instantly updates all charts, tables, and KPIs.

| Filter | Type | How to Use |
|--------|------|------------|
| **Date Range** | Date picker | Select start and end dates to filter by consent date |
| **Age Group** | Multi-select | Pick one or more WHO age groups (4-6, 12-15, 35-44, 65-74) |
| **Gender** | Multi-select | Filter by Male, Female, or Other |
| **Cluster** | Multi-select | Filter by cluster code. Records without clusters show as "Non-Identified" |
| **Examiner** | Multi-select | Filter by examiner number |

**Tip:** The footer of the sidebar shows the current count: *"📊 36 participants · 23 households"*

---

## 3. Tab-by-Tab Guide

### Tab 1: 📊 Overview & Demographics

**Purpose:** Bird's-eye view of who was surveyed, where, and when.

**What you see:**
- **KPI Cards:** Total participants, households, male %, female %, consent %
- **Gender Distribution** (donut chart) — proportion of males vs females
- **Age Group Distribution** (donut chart) — proportion across WHO age groups
- **Participants per Examiner** (bar chart) — workload per examiner
- **Gender × Age Group** (grouped bar) — gender balance within each age group
- **Participants per Cluster** (bar chart) — sample distribution across clusters
- **Enrolment Timeline** (area chart) — daily participant enrolment over time

**How to use:** Check this tab first to understand your sample composition. Look for gender imbalance, uneven cluster distribution, or gaps in the enrolment timeline.

---

### Tab 2: 🏠 Household & SES Analysis

**Purpose:** Socioeconomic profile of surveyed households.

**What you see:**
- **Water Source** (donut) — piped, borewell, bottled, etc.
- **Cooking Fuel** (donut) — LPG, wood, kerosene, etc.
- **Roof Material** (donut) — concrete, tin, thatch, etc.
- **Toilet Facility** (donut) — flush, pit latrine, open defecation
- **Asset Ownership** (horizontal bar) — % owning TV, fridge, smartphone, vehicle, etc.
- **Wealth Score Distribution** (histogram) — computed asset-based wealth index

**How to use:** This tab helps you understand the community's economic profile. Borewell water → check fluorosis tab. Low asset ownership → expect lower dental service utilisation.

---

### Tab 3: 🪥 Oral Hygiene Behaviours

**Purpose:** How people clean their teeth and their risk behaviours.

**What you see:**
- **Cleaning Method** (donut) — toothbrush, finger, twig, charcoal, none
- **Brushing Frequency** (donut) — once, twice, three times per day
- **Toothpaste Type** (donut) — fluoridated vs non-fluoridated vs none
- **Sugar Consumption** (donut) — frequency of sugary snacks between meals
- **Tobacco Use** (donut + age group bar) — current tobacco use prevalence
- **Alcohol Use** (donut + age group bar) — current alcohol consumption
- **Cleaning Method by Gender** (grouped bar) — gender differences in hygiene

**How to use:** If >50% use non-fluoridated paste → recommend fluoride programs. High tobacco in 35-44 group → target cessation campaigns.

---

### Tab 4: 🏥 Dental Service Utilisation

**Purpose:** Access to and use of dental care services.

**What you see:**
- **Oral Pain** (donut) — mouth/teeth problems in last 6 months
- **Ever Visited Dentist** (donut) — first-time vs repeat access
- **Self-Perception** (donut) — how people rate their oral health
- **Reasons for Dental Visit** (bar) — pain, checkup, filling, extraction
- **Barriers to Dental Care** (bar) — cost, fear, distance, self-medication
- **Facility Type** (donut) — private clinic vs government hospital
- **Dental Expenditure** (histogram + box plot) — money spent on last visit

**How to use:** Pain-driven visits mean people only see dentists in emergencies. High "cost" barrier → need for subsidised services.

---

### Tab 5: 🦷 Caries & DMFT Analysis

**Purpose:** The core clinical analysis — tooth decay measurement using WHO's DMFT index.

**What you see:**
- **KPI Cards:** Mean DMFT, D (decayed), M (missing), F (filled)
- **DMFT by Age Group** (stacked bar) — D/M/F components per age group
- **DMFT by Gender** (table) — male vs female comparison
- **DMFT Distribution** (histogram) — spread of individual DMFT scores
- **Caries Prevalence** (bar) — % with at least one decayed tooth per age group
- **Tooth-Level Caries Heatmap** — which specific teeth are most affected

**How to use:**
- DMFT < 2.6 = Low caries | 2.7-4.4 = Moderate | > 4.5 = High
- High D with low F = untreated disease (access problem)
- Heatmap: first molars (16, 26, 36, 46) are always highest — target for sealant programs

---

### Tab 6: 🩸 Periodontal Health

**Purpose:** Gum disease assessment — bleeding, pockets, and attachment loss.

**What you see:**
- **Bleeding Prevalence Heatmap** (per tooth) — gum bleeding sites
- **Any Bleeding %** — overall gingivitis prevalence
- **Pocket Depth Distribution** (donut) — healthy vs 4-5mm vs ≥6mm
- **Any Pocketing %** — periodontitis prevalence
- **Loss of Attachment** (bar) — cumulative gum/bone destruction

**How to use:** Any pocket ≥6mm = severe, needs urgent referral. Bleeding >50% = poor community oral hygiene. LOA increases with age — compare 35-44 vs 65-74.

---

### Tab 7: 🔬 Dental Conditions

**Purpose:** Non-caries conditions — fluorosis, erosion, trauma, and more.

**What you see:**
- **Enamel Fluorosis** (donut) — severity distribution
- **Dental Erosion** (donut) — acid wear from diet
- **Dental Abrasion** (donut) — wear from brushing
- **Dental Attrition** (donut) — tooth-to-tooth wear
- **Dental Trauma** (donut) — fractures and injuries
- **Fluorosis by Age Group** (grouped bar) — age patterns
- **Edentulous Status** (donut, 65-74 only) — complete tooth loss
- **DMH** (donut, 4-6 only) — deciduous molar enamel defects

**How to use:** Fluorosis + borewell water = environmental fluoride exposure. Moderate/severe fluorosis → test community water supply.

---

### Tab 8: 🦴 TMJ & Dentofacial Anomalies

**Purpose:** Jaw joint disorders and orthodontic problems.

**What you see:**
- **TMJ Findings** (bar) — clicking, tenderness, mobility, deviation
- **Crowding** (donut) — dental misalignment
- **Spacing** (donut) — gaps between teeth
- **Molar Relation** (donut) — normal vs half cusp vs full cusp
- **Overjet Distribution** (histogram) — front teeth protrusion in mm

**How to use:** Reduced jaw mobility (<30mm) + tobacco → suspect oral submucous fibrosis. Overjet >6mm → trauma risk in children.

---

### Tab 9: 💊 Oral Lesions & Prosthetics

**Purpose:** Oral cancer screening and denture needs.

**What you see:**
- **Mucosal Lesion Present** (donut) — any soft tissue abnormality
- **Lesion Types** (metrics) — malignant tumor, leukoplakia, lichen planus, ulceration, ANUG, candidiasis, abscess across 9 oral locations
- **Prosthetic Status** (donut) — existing dentures/bridges
- **Prosthetic Need** — unmet prosthetic requirements

**How to use:** ANY leukoplakia + tobacco = immediate referral for biopsy. Compare edentulous count (Tab 7) vs prosthetic status = treatment gap.

---

### Tab 10: 👨‍⚕️ Examiner Analysis ⭐

**Purpose:** Examiner performance, workload, and quality control.

**Key Feature: Examiner Selector**
- **Multiselect** at top — add/remove examiners to analyse
- Select 1 examiner → **individual analysis** (detail card auto-expanded)
- Select 2+ examiners → **group comparison** (side-by-side charts)
- **Select All** button → full team overview

**What you see:**
- **Summary Table** — participants, gender, age groups, days active, avg/day, DMFT, caries %, tobacco %, urgent cases per examiner
- **KPI Cards** — selected examiners count, total participants, most active, most productive
- **Gender × Examiner** (grouped bar) — gender balance per examiner
- **Age Group × Examiner** (grouped bar) — age distribution per examiner
- **Daily Productivity** (line chart) — patients per day over time
- **Productivity Distribution** (box plot) — consistency of daily workload
- **Individual Detail Cards** — expandable per-examiner breakdown with age/cluster/urgency tables and date range info
- **📥 Downloadable CSV Report** — full examiner metrics

**How to use:**
- Compare 2 examiners with very different DMFT → calibration issue
- Single examiner with >15 avg/day → check if rushing
- Examiner only doing 4-6 year olds → DMFT will naturally differ

---

### Tab 11: 🗺️ Cluster Analysis

**Purpose:** Geographic/sampling-unit level analysis.

**What you see:**
- **Cluster Summary Table** — participants, gender, age groups, examiners, DMFT, caries %, tobacco %, oral pain %, urgent cases
- **KPI Cards** — total clusters, largest cluster, highest DMFT cluster, total urgent cases
- **Participants per Cluster** (bar)
- **Gender × Cluster** (grouped bar)
- **Age Group × Cluster** (grouped bar)
- **DMFT by Cluster** (stacked bar) — D/M/F components per cluster
- **Treatment Urgency by Cluster** (grouped bar)
- **Enrolment Timeline per Cluster** (line chart)
- **Examiner × Cluster Cross-tab** — how many patients each examiner did per cluster
- **Detailed Breakdown** — expandable cards showing examiner-wise performance within each cluster
- **📥 Downloadable CSV Report**

**How to use:** Cluster with highest DMFT + lowest dentist visits → priority intervention area. Compare clusters to identify geographic health disparities.

---

### Tab 12: ⚕️ Treatment Needs

**Purpose:** Intervention urgency and cross-dimensional clinical summary.

**What you see:**
- **Intervention Urgency** (donut) — no treatment / preventive / prompt / immediate
- **Urgency by Age Group** (grouped bar)
- **Urgency by Gender** (grouped bar)
- **Clinical Summary Table** — DMFT, caries prevalence, fluorosis, tobacco, oral pain, dentist visits, oral lesion rates
- **Individual Status** (donut) — completion rates

**How to use:** "Immediate" cases need referral TODAY. High "prompt" in 12-15 group → school dental program needed. Zero treatment needed in 65-74 → likely edentulous (no teeth left to treat).

---

### Tab 13: 📋 Data Explorer

**Purpose:** Raw data access for custom analysis.

**What you see:**
- **Full filtered dataset** — scrollable table with all columns
- **📥 Download button** — export filtered data as CSV
- **Column Summary** — non-null count, missing %, dtype, unique values for every column

**How to use:** Use sidebar filters to create subsets (e.g., only females aged 12-15 from cluster 3), then download for analysis in Excel, SPSS, or R.

---

## 4. Tips & Best Practices

1. **Start with Overview** — understand your sample before diving into clinical tabs
2. **Use filters strategically** — compare age groups by deselecting others
3. **Examiner tab for quality control** — run weekly to check examiner consistency
4. **Download reports** — use CSV downloads for formal reporting
5. **Cross-reference tabs** — high fluorosis + borewell water + specific cluster = targeted intervention

---

## 5. Troubleshooting

| Problem | Solution |
|---------|----------|
| Dashboard won't load | Run `pip install -r requirements.txt` and try again |
| Charts show "No data" | Check sidebar filters — you may have filtered out all data |
| Cluster shows "Non-Identified" | Normal — records without cluster codes appear as Non-Identified |
| CSV upload fails | Ensure CSV is REDCap export with "Labels" option selected |
| Slow performance | Large CSVs (>10,000 rows) may take a few seconds to process |

---

*OralHealth Analytics Pro v1.0 — Central Delhi Oral Health Survey Dashboard*
