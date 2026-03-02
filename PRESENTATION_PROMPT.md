# 🎯 PRESENTATION GENERATION PROMPT
# Copy-paste this entire prompt into ChatGPT, Google Gemini, or any AI tool to generate a professional presentation

---

## PROMPT:

Create a professional PowerPoint/Google Slides presentation (20-25 slides) for the following oral health analytics dashboard software. The presentation is for a dental epidemiology team and public health researchers. Use a modern, clean medical/scientific theme with blue and teal accents.

---

### SLIDE 1 — Title Slide
**Title:** OralHealth Analytics Pro
**Subtitle:** A Comprehensive 13-Tab Interactive Dashboard for Oral Health Survey Analysis
**Organisation:** Central Delhi Oral Health Survey
**Technology:** Built with Streamlit, Plotly, Pandas

---

### SLIDE 2 — Problem Statement
**Title:** Why This Dashboard?
**Content:**
- Oral health surveys generate massive datasets with 300+ variables per participant
- Traditional analysis in Excel/SPSS is slow, manual, and not visual
- Field supervisors need real-time insights on examiner performance and data quality
- Decision-makers need instant geographic (cluster-wise) and demographic breakdowns
- No existing tool combines clinical dental indices (DMFT, CPI) with interactive filters

---

### SLIDE 3 — Solution Overview
**Title:** OralHealth Analytics Pro
**Content:**
- Web-based interactive dashboard — runs in any browser
- Supports WHO Oral Health Survey data format (REDCap CSV export)
- 13 dedicated analysis tabs covering every aspect of the survey
- Global filters: Date, Age Group, Gender, Cluster, Examiner
- Automatic DMFT/dmft index calculation
- Downloadable reports (CSV) for examiners and clusters
- Privacy-first: data stays in your browser, never uploaded to any server
- Deployed on Streamlit Cloud — accessible anywhere via link

---

### SLIDE 4 — Technology Architecture
**Title:** System Architecture
**Content:** Show a diagram with:
- User uploads CSV → Streamlit App processes it
- Data Pipeline: Load → Separate (Household vs Participant) → Auto-detect columns → Propagate cluster codes → Apply filters
- Output: 13 interactive tabs with Plotly charts, KPI cards, downloadable CSVs
- Tech Stack: Python, Streamlit, Plotly, Pandas, NumPy
- Deployment: Streamlit Community Cloud (link-only access)

---

### SLIDE 5 — Data Pipeline
**Title:** Smart Data Processing
**Content:**
- Auto-separates Household rows (SES data) from Participant rows (clinical data) using "Repeat Instrument" column
- Auto-detects 40+ column names using keyword matching — works with any REDCap export format
- Cluster codes propagated from Household to Participant rows via record_id
- Empty/missing cluster codes labeled as "Non-Identified"
- Cached data loading for instant re-filtering

---

### SLIDE 6 — Global Filters
**Title:** Interactive Sidebar Filters
**Content:**
- **Date Range** — filter by consent date (calendar picker)
- **Age Group** — WHO index ages: 4-6, 12-15, 35-44, 65-74 years
- **Gender** — Male, Female, Other
- **Cluster** — Sampling unit codes (Non-Identified for missing)
- **Examiner** — Filter by examiner number
- All filters apply globally across all 13 tabs simultaneously
- Real-time participant and household count shown in sidebar footer

---

### SLIDE 7 — Tab 1: Demographics Overview
**Title:** 📊 Overview & Demographics
**Content:**
- KPI cards: Participants, Households, Male %, Female %, Consent %
- Gender distribution (donut chart)
- Age group distribution (donut chart)
- Participants per examiner (bar chart)
- Gender × Age group cross-tabulation (grouped bar)
- Participants per cluster (bar chart)
- Enrolment timeline (area chart — daily trend)
**Key Insight Example:** "36 participants across 23 households, with 6 examiners covering 4 WHO age groups. Enrolment peaked on Feb 18-19."

---

### SLIDE 8 — Tab 2: Household & SES
**Title:** 🏠 Household & Socioeconomic Analysis
**Content:**
- Drinking water source, cooking fuel, roof material, toilet facility (4 donut charts)
- Asset ownership: 15 items (pressure cooker, TV, fridge, smartphone, vehicle, etc.)
- Wealth index distribution (asset score histogram)
**Key Insight Example:** "82% use piped water (low fluorosis risk via water). 94% own smartphones — potential for mHealth oral health education delivery."

---

### SLIDE 9 — Tab 3: Oral Hygiene Behaviours
**Title:** 🪥 Oral Hygiene Behaviours
**Content:**
- Cleaning method (toothbrush, finger, twig, charcoal)
- Brushing frequency (once, twice, three times daily)
- Toothpaste type — CRITICAL: fluoridated vs non-fluoridated
- Sugar consumption frequency between meals
- Tobacco use prevalence (overall + by age group)
- Alcohol use prevalence (overall + by age group)
- Cleaning method by gender (grouped bar)
**Key Insight Example:** "42% use non-fluoridated toothpaste — this single factor explains high caries prevalence. Tobacco use highest in 35-44 age group (28%)."

---

### SLIDE 10 — Tab 4: Dental Service Utilisation
**Title:** 🏥 Dental Service Access & Barriers
**Content:**
- Oral pain in last 6 months
- Ever visited a dentist (Yes/No)
- Self-perception of oral health
- Reasons for visiting dentist (pain, checkup, filling, extraction)
- Barriers to dental care (cost, fear, distance, self-medication)
- Where treatment was sought (private vs government)
- Dental expenditure distribution
**Key Insight Example:** "67% have never visited a dentist. Among those who visited, 78% went due to pain — indicating emergency-driven care with no preventive culture."

---

### SLIDE 11 — Tab 5: Caries & DMFT
**Title:** 🦷 Caries Analysis & DMFT Index
**Content:**
- Automated DMFT calculation (Decayed + Missing + Filled Teeth)
- Mean DMFT by age group (stacked bar showing D/M/F components)
- DMFT by gender (comparison table)
- Caries prevalence by age group (% with D > 0)
- DMFT score distribution (histogram)
- Tooth-level caries heatmap — shows which specific teeth are most affected
**Key Insight Example:** "Mean DMFT = 3.8 (Moderate severity). First molars (teeth 16, 26, 36, 46) have 45% caries rate — confirms need for school-based sealant program. D component >> F component indicates massive unmet treatment need."

---

### SLIDE 12 — Tab 6: Periodontal Health
**Title:** 🩸 Periodontal (Gum) Disease Analysis
**Content:**
- Gingival bleeding prevalence heatmap (per tooth)
- Overall bleeding prevalence %
- Pocket depth distribution (healthy vs 4-5mm vs ≥6mm)
- Pocketing prevalence %
- Loss of attachment distribution (CPI scores)
**Key Insight Example:** "72% have gingival bleeding (reversible with better hygiene). 8% have pockets ≥6mm (severe periodontitis — needs specialist care)."

---

### SLIDE 13 — Tab 7: Dental Conditions
**Title:** 🔬 Non-Caries Dental Conditions
**Content:**
- Enamel fluorosis severity distribution
- Dental erosion, abrasion, attrition (3 donut charts)
- Dental trauma types
- Fluorosis by age group (grouped bar)
- Edentulous status (65-74 only)
- DMH — deciduous molar hypomineralization (4-6 only)
**Key Insight Example:** "Moderate-severe fluorosis in 12% of participants. Cross-referencing with Tab 2 shows these are from borewell-water households — confirms environmental fluoride as the cause."

---

### SLIDE 14 — Tab 8: TMJ & Orthodontics
**Title:** 🦴 TMJ & Dentofacial Anomalies
**Content:**
- TMJ findings: clicking, tenderness, reduced jaw mobility, jaw deviation
- Crowding and spacing (donut charts)
- Molar relation (normal, half cusp, full cusp)
- Overjet distribution (histogram in mm)
**Key Insight Example:** "Reduced jaw mobility found in 5 participants — all tobacco users. Possible oral submucous fibrosis — needs biopsy referral."

---

### SLIDE 15 — Tab 9: Oral Lesions & Prosthetics
**Title:** 💊 Oral Cancer Screening & Prosthetics
**Content:**
- Oral mucosal lesion prevalence
- 7 lesion types screened across 9 oral locations:
  - Malignant tumor, Leukoplakia, Lichen planus, Ulceration, ANUG, Candidiasis, Abscess
- Prosthetic status (existing dentures/bridges)
- Prosthetic need (unmet)
**Key Insight Example:** "Leukoplakia detected in 3 participants — all current tobacco users. Immediate biopsy referral initiated. Zero prosthetics among 8 edentulous elderly — 100% unmet prosthetic need."

---

### SLIDE 16 — Tab 10: Examiner Analysis ⭐
**Title:** 👨‍⚕️ Examiner Performance & Quality Control
**Content:**
HIGHLIGHT: Interactive Examiner Selector (add/remove examiners)
- **Individual Mode:** Select 1 examiner → detailed personal report with auto-expanded card
- **Group Mode:** Select 2+ examiners → side-by-side comparison
- **All Mode:** Select all → full team overview
- Summary table with 12+ metrics per examiner
- KPI cards: most active, most productive, total participants
- Gender × Examiner and Age Group × Examiner (grouped bars)
- Daily productivity trend (line chart)
- Productivity distribution (box plot)
- Individual examiner detail cards (expandable): age group, cluster, urgency breakdown, date range
- Downloadable examiner report CSV
**Key Insight Example:** "Examiner 3 has DMFT = 5.2 while team average is 3.8. Either this examiner is over-diagnosing caries, or they were assigned to a higher-risk cluster. Cross-reference with Cluster Analysis to verify."

---

### SLIDE 17 — Tab 11: Cluster Analysis
**Title:** 🗺️ Geographic/Cluster-wise Analysis
**Content:**
- Cluster summary table with all metrics (DMFT, caries %, tobacco %, urgent cases)
- KPI cards: total clusters, largest cluster, highest DMFT, total urgent
- Participants per cluster, gender × cluster, age group × cluster
- DMFT by cluster (stacked bar — D/M/F components)
- Treatment urgency by cluster (grouped bar)
- Enrolment timeline per cluster (line chart)
- **Examiner × Cluster cross-tabulation** — how many patients each examiner did per cluster
- Detailed expandable cards: per-cluster examiner breakdown
- Downloadable cluster report CSV
**Key Insight Example:** "Cluster 3 has highest DMFT (4.6) and lowest dentist visits (15%). This cluster should be the priority target for a mobile dental camp."

---

### SLIDE 18 — Tab 12: Treatment Needs
**Title:** ⚕️ Treatment Needs & Clinical Summary
**Content:**
- Intervention urgency distribution (no treatment / preventive / prompt / immediate)
- Urgency by age group and gender (grouped bars)
- Clinical summary table: DMFT, caries, fluorosis, tobacco, pain, dentist visits, lesion rates
- Individual completion status (donut)
**Key Insight Example:** "22% need immediate/prompt treatment. Combined with 67% never visiting a dentist, this indicates a massive treatment backlog requiring community-level intervention."

---

### SLIDE 19 — Tab 13: Data Explorer
**Title:** 📋 Raw Data Explorer
**Content:**
- Full scrollable filtered dataset
- One-click CSV download of filtered data
- Column summary: non-null count, missing %, data type, unique values
- Use with sidebar filters to create custom subsets for export
**Key Insight Example:** "Export all female participants aged 12-15 from Cluster 3 for a focused school dental health report."

---

### SLIDE 20 — Privacy & Security
**Title:** 🔒 Data Privacy & Security
**Content:**
- All processing happens locally in the browser — no data stored on servers
- CSV data is never uploaded to GitHub (excluded via .gitignore)
- GitHub repository is private — source code is not publicly visible
- Streamlit Cloud app is unlisted — only accessible via direct link
- No patient identifiers are displayed in the dashboard

---

### SLIDE 21 — Use Cases
**Title:** Who Benefits from This Dashboard?
**Content:**
| User | How They Use It |
|------|----------------|
| **Principal Investigator** | Overall survey progress, clinical outcomes, publication-ready metrics |
| **Field Supervisors** | Daily examiner monitoring, workload balancing, quality control |
| **Public Health Officers** | Cluster-level disparities, intervention prioritisation |
| **Policy Makers** | Treatment need estimates, barrier analysis, resource allocation |
| **Dental Academicians** | Teaching tool for dental public health, DMFT methodology |

---

### SLIDE 22 — Key Differentiators
**Title:** What Makes This Dashboard Unique?
**Content:**
1. ✅ **13 comprehensive tabs** — most oral health tools have 2-3
2. ✅ **Auto-column detection** — works with any REDCap CSV export
3. ✅ **Clinical indices calculated automatically** — DMFT, CPI, LOA
4. ✅ **Examiner quality control** — add/remove examiners for comparison
5. ✅ **Cluster-level analysis** — geographic health equity assessment
6. ✅ **Tooth-level caries heatmap** — pinpoints which teeth are most affected
7. ✅ **Privacy-first** — no data leaves the user's device
8. ✅ **Downloadable reports** — CSV exports for examiners, clusters, filtered data
9. ✅ **Zero installation** — runs in any web browser via Streamlit Cloud

---

### SLIDE 23 — Technical Specifications
**Title:** Technical Details
**Content:**
| Specification | Detail |
|--------------|--------|
| Language | Python 3.9+ |
| Framework | Streamlit |
| Charts | Plotly Express + Graph Objects |
| Data Processing | Pandas, NumPy |
| Deployment | Streamlit Community Cloud |
| Repository | GitHub (Private) |
| Source Lines | ~950 lines of Python |
| Columns Detected | 40+ automatic mappings |
| Chart Types | Donut, Bar, Grouped Bar, Stacked Bar, Line, Area, Box Plot, Histogram, Heatmap |
| Export Formats | CSV |

---

### SLIDE 24 — Future Roadmap
**Title:** Future Enhancements
**Content:**
1. 📊 Statistical testing (chi-square, t-test) with p-values on charts
2. 📈 Automated PDF report generation with all charts
3. 🗺️ GIS mapping of clusters on actual Delhi map
4. 🔄 Live REDCap API integration for real-time data sync
5. 🤖 AI-powered anomaly detection for data quality alerts
6. 📱 Mobile-responsive layout for field use on tablets
7. 🔐 Role-based access control (PI vs Field Supervisor vs Examiner views)

---

### SLIDE 25 — Thank You / Demo
**Title:** Thank You
**Subtitle:** Live Demo Available
**Content:**
- Dashboard URL: [Your Streamlit Cloud URL]
- GitHub: github.com/Anshh12/oral-health-dashboard (private)
- Built with ❤️ for the Central Delhi Oral Health Survey
- Contact: [Your contact information]

---

## DESIGN INSTRUCTIONS FOR THE AI TOOL:

1. **Theme:** Modern medical/scientific — use navy blue (#0f172a), teal (#06b6d4), and white backgrounds
2. **Fonts:** Use clean sans-serif fonts (Inter, Montserrat, or Roboto)
3. **Icons:** Use medical/dental emojis as shown (🦷, 📊, 🏥, etc.)
4. **Charts:** For slides showing dashboard features, include sample chart mockups (donut charts, bar charts, heatmaps)
5. **Layout:** Use 2-column layouts where applicable (text left, visual right)
6. **Transitions:** Subtle fade transitions between slides
7. **Footer:** Add slide numbers and "OralHealth Analytics Pro" as running footer
8. **Color-code tabs:** Each tab should have its own accent color matching the dashboard's color palette
