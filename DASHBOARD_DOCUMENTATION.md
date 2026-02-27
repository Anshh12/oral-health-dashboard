# 🦷 OralHealth Analytics Pro — Complete Documentation

## Table of Contents
1. [Architecture Overview](#architecture-overview)
2. [Data Pipeline](#data-pipeline)
3. [Sidebar Filters](#sidebar-filters)
4. [Tab 1: Overview & Demographics](#tab-1-overview--demographics)
5. [Tab 2: Household & SES Analysis](#tab-2-household--ses-analysis)
6. [Tab 3: Oral Hygiene Behaviours](#tab-3-oral-hygiene-behaviours)
7. [Tab 4: Dental Service Utilisation](#tab-4-dental-service-utilisation)
8. [Tab 5: Caries & DMFT Analysis](#tab-5-caries--dmft-analysis)
9. [Tab 6: Periodontal Health](#tab-6-periodontal-health)
10. [Tab 7: Dental Conditions](#tab-7-dental-conditions)
11. [Tab 8: TMJ & Dentofacial Anomalies](#tab-8-tmj--dentofacial-anomalies)
12. [Tab 9: Oral Lesions & Prosthetics](#tab-9-oral-lesions--prosthetics)
13. [Tab 10: Examiner Analysis](#tab-10-examiner-analysis)
14. [Tab 11: Treatment Needs & Cross-Analysis](#tab-11-treatment-needs--cross-analysis)
15. [Tab 12: Data Explorer](#tab-12-data-explorer)
16. [Technical Reference](#technical-reference)

---

## Architecture Overview

### Technology Stack

| Component      | Technology                          |
| -------------- | ----------------------------------- |
| **Framework**  | Streamlit (Python web framework)    |
| **Charting**   | Plotly Express + Plotly Graph Objects|
| **Data**       | Pandas, NumPy                       |
| **Styling**    | Custom CSS (Inter + Playfair Display fonts) |
| **Caching**    | `@st.cache_data` for CSV reads      |

### System Architecture

```
┌──────────────────────────────────────────────────────────────┐
│                     STREAMLIT APP                             │
│                                                              │
│  ┌─────────────┐    ┌────────────────┐    ┌───────────────┐  │
│  │  SIDEBAR     │    │  DATA PIPELINE  │    │  MAIN AREA    │  │
│  │             │    │                │    │               │  │
│  │ • CSV Upload │───►│ • Load CSV     │───►│ • KPI Cards   │  │
│  │ • Date Range │    │ • Separate HH  │    │ • 12 Tabs     │  │
│  │ • Age Group  │    │   vs Participant│    │ • Charts      │  │
│  │ • Gender    │    │ • Auto-detect  │    │ • Tables      │  │
│  │ • Cluster   │    │   columns      │    │ • Downloads   │  │
│  │ • Examiner  │    │ • Propagate    │    │               │  │
│  │             │    │   cluster codes │    │               │  │
│  └─────────────┘    │ • Apply filters │    └───────────────┘  │
│                     └────────────────┘                        │
└──────────────────────────────────────────────────────────────┘
```

### Data Flow

1. **Upload** → User uploads REDCap CSV export via sidebar
2. **Load** → `@st.cache_data` reads CSV once, caches in memory
3. **Separate** → Rows split into Household (no Repeat Instrument) and Participant (Repeat Instrument = "Participant")
4. **Auto-Detect** → ~40+ column names are matched using keyword search
5. **Propagate** → Cluster codes from household rows are joined to participant rows via `record_id`
6. **Filter** → Sidebar filters (date, age, gender, cluster, examiner) are applied sequentially
7. **Render** → 12 tabs generate visualisations from the filtered `df`

---

## Data Pipeline

### Record Type Separation

The uploaded CSV contains **two interleaved record types**:

| Row Type          | Identified By                               | Contains                                      |
| ----------------- | -------------------------------------------- | --------------------------------------------- |
| **Household**     | `Repeat Instrument` is empty/NaN             | SES data, assets, water, fuel, toilet, roof   |
| **Participant**   | `Repeat Instrument` = "Participant"          | Demographics, clinical exam, dental status     |

Multiple participants can belong to the same household (same `record_id`).

### Column Auto-Detection

The `find_col(keywords, columns)` function scans all column names and returns the first match containing any keyword (case-insensitive). This makes the dashboard work with different REDCap exports without manually mapping columns.

**Example:** `find_col(["Examiner Number", "Examiner"], columns)` → finds `"Examiner Number"` column.

### Cluster Code Propagation

Since `cluster code` is only stored in household-level rows, the pipeline:
1. Extracts `{record_id: cluster_code}` from household rows
2. Maps these values onto participant rows via `record_id`
3. Preserves any existing cluster values in participant rows

---

## Sidebar Filters

All filters are **global** — they affect every tab simultaneously.

| Filter       | Type          | Behaviour                                                                                  |
| ------------ | ------------- | ------------------------------------------------------------------------------------------ |
| **Date Range** | Date picker | Filters participants by consent date within `[start, end]`                                 |
| **Age Group** | Multi-select | Shows only selected age groups (4-6, 12-15, 35-44, 65-74). All selected by default.       |
| **Gender**   | Multi-select  | Shows only selected genders. All selected by default.                                      |
| **Cluster**  | Multi-select  | Shows only selected clusters. Participants without cluster assignment are always included.  |
| **Examiner** | Multi-select  | Shows only selected examiners. All selected by default.                                    |

**Note:** If cluster data is unavailable in the dataset, a caption "ℹ️ No cluster data available" is shown instead of a broken dropdown.

---

## Tab 1: Overview & Demographics

### Purpose
Provides a **bird's-eye view** of the entire survey population — who was surveyed, where, by whom, and when.

### KPI Cards (Top Row)

| Card               | What It Shows                                                                 | Why It Matters                                                    |
| ------------------- | ----------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| **Participants**    | Total number of participants after filters                                    | Core sample size — affects statistical confidence of all analyses |
| **Households**      | Total household records in the raw data                                       | Shows reach of household-level data collection                    |
| **Male %**          | Percentage of male participants                                               | Gender balance is critical for representative epidemiological data|
| **Female %**        | Percentage of female participants                                             | Complements male percentage                                       |
| **Consent %**       | Percentage who consented to participate                                       | Indicates participation rate and potential selection bias          |

### Charts

#### 1. Gender Distribution (Donut Chart)
- **What:** Proportions of Male, Female, Others
- **Analysis:** In oral health surveys, gender is a key confounder. Males often have higher tobacco use and lower dental visit rates, while females may have higher caries rates due to hormonal factors and earlier eruption of teeth. An imbalanced gender ratio may bias prevalence estimates.

#### 2. Age Group Distribution (Donut Chart)
- **What:** Proportions across WHO-standard age groups (4-6, 12-15, 35-44, 65-74)
- **Analysis:** These are WHO Index Age Groups for oral health surveys:
  - **4-6 years:** Deciduous (primary) dentition — assesses early childhood caries and DMH
  - **12-15 years:** Permanent dentition begins — global standard for comparing caries across countries
  - **35-44 years:** Adult population — full exposure to risk factors, peak of periodontal disease
  - **65-74 years:** Elderly — edentulism, prosthetic needs, cumulative effects of lifelong oral disease

#### 3. Participants per Examiner (Horizontal Bar)
- **What:** Count of participants examined by each examiner
- **Analysis:** Uneven workload distribution may indicate logistical issues. Large differences between examiners could also introduce inter-examiner variability in clinical findings.

#### 4. Gender × Age Group (Grouped Bar)
- **What:** Cross-tabulation showing male vs female counts within each age group
- **Analysis:** Reveals if any age group has gender imbalance. For example, if the 65-74 group is 80% female, prevalence estimates for that age group reflect predominantly female oral health.

#### 5. Participants per Cluster (Bar Chart)
- **What:** Number of participants in each cluster (sampling unit)
- **Analysis:** Clusters should ideally have similar participant counts for a balanced cluster sample survey. Large variation suggests some clusters were harder to access or had lower response rates.

#### 6. Enrolment Timeline (Area Chart)
- **What:** Daily count of new participants enrolled over time
- **Analysis:** Shows the pace of data collection. Spikes may indicate high-activity field days. Gaps may indicate weekends, holidays, or logistical issues. A declining trend might indicate survey fatigue.

---

## Tab 2: Household & SES Analysis

### Purpose
Analyses **socioeconomic determinants** of oral health at the household level. SES is one of the strongest predictors of oral disease.

### Charts

#### 1. Drinking Water Source (Donut Chart)
- **What:** Distribution of water sources (piped, borewell, bottled, RO, dug well, etc.)
- **Analysis:** Water source is directly linked to fluoride exposure. Borewell/groundwater may contain high fluoride levels leading to dental fluorosis. Piped water may be fluoridated. This chart helps explain fluorosis prevalence patterns.

#### 2. Cooking Fuel (Donut Chart)
- **What:** Type of fuel used (LPG, wood, kerosene, etc.)
- **Analysis:** A proxy for socioeconomic status. LPG/electricity use indicates higher SES. Households using biomass fuels (wood, dung) are typically lower SES and may have poorer access to dental care.

#### 3. Roof Material (Donut Chart)
- **What:** Material of household roof (RCC/concrete, tin, thatch, etc.)
- **Analysis:** Another SES proxy. Concrete/RCC roofs indicate better economic status. Combined with other asset data, helps build a composite wealth index.

#### 4. Toilet Facility (Donut Chart)
- **What:** Type of toilet (flush, pit latrine, open defecation, etc.)
- **Analysis:** Sanitation access correlates with overall health awareness and health-seeking behaviour, including dental care utilisation.

#### 5. Asset Ownership (Horizontal Bar)
- **What:** Percentage of households owning each asset (TV, fridge, motorcycle, smartphone, etc.)
- **Analysis:** High ownership of items like smartphones, refrigerators, and washing machines indicates higher SES. Lower ownership of tractors, livestock, and land indicates urban population. This profile helps contextualise oral health findings within the community's economic reality.

#### 6. Wealth Index / Asset Score Distribution (Histogram)
- **What:** Computed score = count of "Yes" answers across 15 asset questions per household
- **Analysis:** Creates a simple wealth index. A left-skewed distribution means most households are relatively well-off; a right-skewed distribution indicates widespread poverty. This score can later be correlated with dental outcomes (higher wealth → more dental care access → better outcomes).

---

## Tab 3: Oral Hygiene Behaviours

### Purpose
Analyses **behavioural risk factors** — the habits that directly cause or prevent oral disease.

### Charts

#### 1. Teeth Cleaning Method (Donut Chart)
- **What:** How participants clean teeth (toothbrush, finger, datun/twig, charcoal, none)
- **Analysis:** Toothbrush use is the gold standard. Finger cleaning is very common in lower SES communities and is far less effective, leading to higher plaque accumulation and caries/gingivitis.

#### 2. Brushing Frequency (Donut Chart)
- **What:** How many times per day participants brush (once, twice, thrice+, none)
- **Analysis:** WHO recommends brushing at least twice daily. "Once" brushing is suboptimal; "none" is a significant risk factor. A high proportion of once-daily brushers suggests need for oral health education interventions.

#### 3. Toothpaste Type (Donut Chart)
- **What:** Fluoridated vs non-fluoridated vs no toothpaste
- **Analysis:** **Critical chart.** Fluoridated toothpaste is the single most effective caries prevention measure. If a large proportion use non-fluoridated paste or no paste, this directly explains high caries prevalence. This has major public health intervention implications.

#### 4. Sugar Consumption Frequency (Donut Chart)
- **What:** How often sugary foods/drinks are consumed between meals (once, twice, thrice+, never)
- **Analysis:** Sugar is the primary dietary cause of dental caries. Frequency matters more than quantity — each sugar exposure creates a 20-minute acid attack on teeth. "Thrice or more" daily puts individuals at very high caries risk.

#### 5. Cleaning Material Usage (Horizontal Bar)
- **What:** Percentage using each cleaning material (toothpaste, tooth powder, charcoal, none)
- **Analysis:** Shows the market penetration of modern oral hygiene products vs traditional methods. Charcoal and tobacco toothpaste use is noteworthy — tobacco-based dental powders are a unique cultural practice that paradoxically increases oral cancer risk while cleaning teeth.

#### 6. Tobacco Use (Donut + Age Group Bar)
- **What:** Current tobacco use prevalence overall and broken down by age group
- **Analysis:** Tobacco (smoked and smokeless) is the #1 risk factor for oral cancer, periodontal disease, staining, and halitosis. Smokeless tobacco (gutka, pan masala) is extremely common in Delhi and directly causes oral submucous fibrosis, leukoplakia, and oral cancer. Age-group breakdown shows which populations need targeted cessation programs.

#### 7. Alcohol Consumption (Donut + Age Group Bar)
- **What:** Current alcohol use prevalence
- **Analysis:** Alcohol synergises with tobacco to dramatically increase oral cancer risk (multiplicative, not additive). Combined tobacco + alcohol users have 15× higher oral cancer risk.

#### 8. Cleaning Method by Gender (Grouped Bar)
- **What:** Cross-tab of cleaning method vs gender
- **Analysis:** Reveals gender disparities in oral hygiene. In many Indian communities, women may use finger/traditional methods more than men, or vice versa. This informs gender-targeted health education strategies.

---

## Tab 4: Dental Service Utilisation

### Purpose
Measures **access to and use of dental care** — the demand side of oral health systems.

### Charts

#### 1. Oral Pain in Last 6 Months (Donut Chart)
- **What:** Whether participants experienced mouth/teeth problems recently
- **Analysis:** Oral pain is a key unmet need indicator. A high "Yes" percentage means significant untreated dental disease. Combined with dentist visit rates, shows the gap between need and utilisation.

#### 2. Ever Visited a Dentist (Donut Chart)
- **What:** Whether participants have ever visited a dentist
- **Analysis:** In India, dental visit rates are typically 20-30% in urban areas. A high "No" rate indicates lack of access or awareness. This is the most basic measure of dental care utilisation.

#### 3. Self-Perception of Oral Health (Donut Chart)
- **What:** How participants rate their own teeth and gums (Excellent → Poor)
- **Analysis:** Self-perception often poorly correlates with clinical findings. People with active caries may report "Good" oral health because they've normalised dental problems. Discrepancy between self-perception and clinical findings (DMFT) reveals low oral health literacy.

#### 4. Reasons for Last Dental Visit (Horizontal Bar)
- **What:** Why participants visited the dentist (pain, checkup, scaling, filling, extraction, etc.)
- **Analysis:** **Pain-driven visits dominate** in most Indian populations, meaning people only see dentists in emergencies. A high proportion visiting for "checkup" would indicate preventive care culture — which is rare. This chart directly shows whether care is curative vs preventive.

#### 5. Barriers to Dental Care (Horizontal Bar)
- **What:** Why participants did NOT visit a dentist (cost, fear, distance, self-medication, etc.)
- **Analysis:** Identifies the biggest systemic barriers. "Cost too high" and "self-medication" are typically the top barriers in India. This data directly informs policy interventions — e.g., if distance is a top barrier, mobile dental clinics are needed.

#### 6. Treatment Facility Type (Donut Chart)
- **What:** Where treatment was sought (private clinic, government hospital, etc.)
- **Analysis:** Private vs public sector utilisation shows the dental care ecosystem. In India, 70-80% of dental care is private. Heavy private sector dependence means cost is a major barrier for lower SES groups.

#### 7. Dental Expenditure (Histogram + Box Plot)
- **What:** How much participants spent on their last dental visit (in ₹)
- **Analysis:** Shows the financial burden of dental care. The mean/median difference reveals skew — a few very expensive treatments (root canals, crowns) pull the mean up. High out-of-pocket expenditure confirms the need for dental insurance or public dental services.

---

## Tab 5: Caries & DMFT Analysis

### Purpose
The **core clinical analysis** — measures dental caries (tooth decay), the most common chronic disease worldwide.

### DMFT Index Explained
DMFT = **Decayed + Missing + Filled Teeth** (for permanent teeth, age 12+)
dmft = same index for deciduous/primary teeth (age 4-6)

Each component means:
- **D (Decayed):** Untreated active caries — indicates unmet treatment need
- **M (Missing):** Teeth lost due to caries — indicates irreversible damage
- **F (Filled):** Successfully treated caries — indicates access to dental care
- **DMFT (Total):** Lifetime cumulative caries experience

### Charts

#### 1. KPI Cards (Mean DMFT, D, M, F)
- **What:** Average values across all filtered participants
- **Analysis:** WHO benchmarks per DMFT:
  - **< 1.2** = Very low caries prevalence
  - **1.2 - 2.6** = Low
  - **2.7 - 4.4** = Moderate
  - **4.5 - 6.5** = High
  - **> 6.5** = Very high
  
  A high D component with low F means people have caries but can't get treatment. A high M component in younger age groups is alarming — teeth should not be lost in adulthood.

#### 2. DMFT by Age Group (Stacked Bar)
- **What:** Mean D, M, and F components stacked by age group
- **Analysis:** DMFT naturally increases with age because it's cumulative. But the **composition** changes are key:
  - **12-15 year olds:** Should have mostly D (new caries), some F (if treated), and zero M
  - **35-44 year olds:** Mix of all three. High M suggests delayed treatment
  - **65-74 year olds:** M dominates (lifetime of tooth loss). High D at this age means ongoing untreated disease

#### 3. DMFT by Gender (Table)
- **What:** Mean D, M, F, DMFT for males vs females
- **Analysis:** Females often have slightly higher DMFT (earlier tooth eruption = more exposure time). But if males have higher D, it may indicate poorer hygiene. This table enables gender-specific intervention planning.

#### 4. DMFT Score Distribution (Histogram)
- **What:** Frequency distribution of individual DMFT scores
- **Analysis:** A right-skewed distribution (many people at 0-2, few at 10+) is typical. The "tail" identifies the high-risk group. If the distribution is bimodal (peaks at 0 and at 8+), there are two distinct populations: those with access to care and those without.

#### 5. Caries Prevalence by Age Group (Bar)
- **What:** Percentage of people with at least one decayed tooth (D > 0)
- **Analysis:** A simpler metric than mean DMFT. If 60% of 12-year-olds have caries, this is directly comparable to international benchmarks. WHO targets <50% caries-free in 12-year-olds by 2030.

#### 6. Tooth-Level Caries Heatmap
- **What:** Percentage of caries for each individual permanent tooth (18-48)
- **Analysis:** **Highly clinically valuable.** Shows which specific teeth are most affected. Typically:
  - **First molars (16, 26, 36, 46):** Highest caries rate because they erupt first (age 6) and have deep fissures
  - **Anterior teeth (11-23, 31-43):** Lower caries rate
  - **Third molars (18, 28, 38, 48):** Often missing/unerupted
  - This pattern guides sealant programs (target first molars) and helps validate the examiner's diagnostic consistency.

---

## Tab 6: Periodontal Health

### Purpose
Analyses **gum disease** — the second most common oral disease, and the leading cause of tooth loss in adults.

### Charts

#### 1. Bleeding Prevalence Heatmap (Per Tooth)
- **What:** Percentage of participants with gingival bleeding at each tooth site
- **Analysis:** Bleeding on probing is the earliest sign of gum inflammation (gingivitis). Higher bleeding rates indicate:
  - Poor oral hygiene (plaque accumulation)
  - Active inflammatory process
  - Universal bleeding (>50% of sites) suggests generalised gingivitis in the population
  - Higher rates at lower anteriors (31-42) is typical due to lingual calculus deposits.

#### 2. Any Bleeding Prevalence (Statistic)
- **What:** Percentage of participants with bleeding at ANY tooth
- **Analysis:** Population-level gingivitis prevalence. In India, typically 50-90%. This single number captures the overall burden of gum inflammation.

#### 3. Pocket Depth Distribution (Donut Chart)
- **What:** Proportions of "No pocket", "4-5mm", "≥6mm" across all examined teeth
- **Analysis:** 
  - **No pocket:** Healthy or gingivitis only (reversible)
  - **4-5mm pockets:** Moderate periodontitis — requires scaling/root planing
  - **≥6mm pockets:** Severe periodontitis — may require surgery, risk of tooth loss
  - Any % in the ≥6mm category is clinically significant and requires urgent intervention.

#### 4. Any Pocketing Prevalence (Statistic)
- **What:** % of participants with at least one pocket ≥4mm
- **Analysis:** Direct measure of periodontitis prevalence in the population. Combined with smoking data, this predicts future tooth loss burden.

#### 5. Loss of Attachment Distribution (Bar)
- **What:** CPI (Community Periodontal Index) scores for loss of attachment at sextant level
- **Analysis:** LOA measures the cumulative destruction of the tooth's supporting structures. Higher LOA categories (9-11mm, ≥12mm) indicate advanced periodontal disease. This is primarily relevant for the 35-44 and 65-74 age groups.

---

## Tab 7: Dental Conditions

### Purpose
Analyses **non-caries dental conditions** — fluorosis, erosion, abrasion, attrition, and trauma.

### Charts

#### 1. Enamel Fluorosis Severity (Donut Chart)
- **What:** Distribution of fluorosis severity (Normal, Questionable, Very Mild, Mild, Moderate, Severe)
- **Analysis:** Fluorosis is caused by excess fluoride during tooth development (age 0-8). Severity indicates:
  - **Normal/Questionable:** No concern
  - **Very Mild/Mild:** Cosmetic white spots — acceptable, indicates optimal fluoride
  - **Moderate:** Brown staining, pitting — may need cosmetic treatment
  - **Severe:** Structural damage — indicates environmental fluoride contamination (water >1.5 ppm)
  - Cross-reference with drinking water source (borewell/groundwater) to identify the cause.

#### 2. Dental Erosion (Donut Chart)
- **What:** Erosion severity distribution
- **Analysis:** Erosion is chemical (non-bacterial) acid wear from diet (citrus, carbonated drinks, GERD). Increasing prevalence in urban populations due to acidic beverages. Important emerging condition.

#### 3. Dental Abrasion (Donut Chart)
- **What:** Presence and severity of abrasion
- **Analysis:** Abrasion is mechanical wear from incorrect brushing technique (aggressive horizontal scrubbing) or abrasive dentifrice. "Present" cases need brushing technique counselling.

#### 4. Dental Attrition (Donut Chart)
- **What:** Tooth wear from tooth-to-tooth contact
- **Analysis:** Natural age-related phenomenon. Excessive attrition in younger age groups may indicate bruxism (teeth grinding/clenching). Important for the 35-44 and 65-74 groups.

#### 5. Dental Trauma (Donut Chart)
- **What:** Types of dental injuries (enamel fracture, enamel+dentine fracture, pulp exposure, missing due to trauma)
- **Analysis:** Trauma predominantly affects upper front teeth in children/adolescents. High prevalence may indicate sports injuries or falls. "Missing due to trauma" in the 35-44 group may reflect untreated childhood injuries.

#### 6. Fluorosis by Age Group (Grouped Bar)
- **What:** Cross-tab of fluorosis severity within each age group
- **Analysis:** If fluorosis is concentrated in younger age groups, it may indicate a recent water source change. If evenly distributed, it suggests a longstanding environmental exposure.

#### 7. Edentulous Status (Donut Chart) — 65-74 only
- **What:** Whether elderly participants have any natural teeth remaining
- **Analysis:** "Completely edentulous" is the end-stage of oral disease. WHO target is <5% edentulous in 65-74 by 2030. High rates indicate a lifetime of poor oral care.

#### 8. DMH — Deciduous Molar Hypomineralization (Donut Chart) — 4-6 only
- **What:** Presence of enamel defects in deciduous (baby) molars
- **Analysis:** DMH is increasingly recognised as a precursor to MIH. Affected teeth are weaker and more susceptible to caries. Early identification allows preventive measures.

---

## Tab 8: TMJ & Dentofacial Anomalies

### Purpose
Assesses **jaw joint disorders** and **orthodontic (alignment) problems**.

### Charts

#### 1. TMJ Findings Prevalence (Horizontal Bar)
- **What:** % with clicking, tenderness, reduced mobility, jaw deviation
- **Analysis:** TMJ disorders affect chewing function and quality of life:
  - **Clicking:** Most common, often benign but may indicate disc displacement
  - **Tenderness:** Indicates active inflammation, possibly related to bruxism
  - **Reduced mobility (<30mm):** Indicates trismus or fibrosis (may be related to tobacco/OSMF)
  - **Jaw deviation:** Structural asymmetry

#### 2. Crowding Distribution (Donut Chart)
- **What:** Presence of dental crowding (misaligned/overlapping teeth)
- **Analysis:** Crowding in upper and/or lower arches. Common in Indian population (>50% prevalence). Crowded teeth are harder to clean → higher caries and periodontal disease risk. Important for orthodontic treatment planning.

#### 3. Spacing Distribution (Donut Chart)
- **What:** Gaps between teeth
- **Analysis:** Spacing can cause aesthetic concerns and food impaction. Relevant for orthodontic need assessment.

#### 4. Molar Relation (Donut Chart)
- **What:** Antero-posterior relationship of molars (Normal, Half cusp, Full cusp)
- **Analysis:** 
  - **Normal:** Class I occlusion — ideal
  - **Half cusp:** Mild malocclusion
  - **Full cusp:** Severe Class II or III — significant orthodontic treatment need

#### 5. Overjet Distribution (Histogram)
- **What:** Distance between upper and lower front teeth (in mm)
- **Analysis:** Normal overjet is 2-4mm. Excessive overjet (>6mm) increases trauma risk. Negative overjet (mandibular prognathism) may need surgical intervention.

---

## Tab 9: Oral Lesions & Prosthetics

### Purpose
Screens for **oral cancer-related conditions** and assesses **prosthetic (denture) status**.

### Data Points

#### 1. Oral Mucosal Lesion Present (Donut Chart)
- **What:** Whether any soft tissue abnormality was found
- **Analysis:** Presence of ANY oral mucosal lesion is a red flag requiring follow-up. In tobacco-heavy populations, even a 5% prevalence represents significant cancer risk.

#### 2. Lesion Type Prevalence (Text Metrics)
- **What:** Prevalence of specific lesion types across 9 oral locations each:
  - **Malignant tumor (oral cancer):** Confirmed cancer requiring immediate referral
  - **Leukoplakia:** White patch — most common potentially malignant disorder. 5-17% malignant transformation rate
  - **Lichen planus:** Chronic inflammatory condition with ~1% malignant transformation
  - **Ulceration:** May be traumatic (denture sores) or pathological
  - **ANUG:** Acute infection of gums (trench mouth) — indicates severe neglect
  - **Candidiasis:** Fungal infection — may indicate immunocompromise
  - **Abscess:** Acute dental infection requiring emergency care
- **Analysis:** Any non-zero prevalence for malignant tumor or leukoplakia is grounds for a community-level oral cancer screening program. Cross-reference with tobacco use data.

#### 3. Prosthetic Status (Donut Charts)
- **What:** Whether participant has upper/lower prosthesis (bridge, partial denture, full denture)
- **Analysis:** Combined with edentulous status, shows the "treatment gap" — how many edentulous people actually have dentures. A large gap indicates unmet prosthetic need.

---

## Tab 10: Examiner Analysis

### Purpose
Evaluates **examiner performance, workload, and consistency** — critical for data quality assurance.

### Charts & Tables

#### 1. Examiner Summary Table
- **What:** All examiners with total participants, male/female counts, per age group counts, days active, average patients per day, mean DMFT
- **Analysis:** The central performance dashboard. Key metrics to monitor:
  - **Participants per examiner:** Should be roughly balanced for inter-examiner reliability
  - **Avg/Day:** Too high (>15) may indicate rushed examinations; too low (<3) suggests underperformance
  - **Mean DMFT:** Should be similar across examiners. A significantly higher/lower DMFT for one examiner may indicate diagnostic bias (over/under-detection)

#### 2. KPI Cards
- **Most Active Examiner:** Highest participant count — may need rest/rotation
- **Highest Productivity:** Best avg/day — verify quality isn't sacrificed
- **Total Examiners:** Overall team size

#### 3. Gender × Examiner (Grouped Bar)
- **What:** Male vs female participants per examiner
- **Analysis:** Checks if any examiner disproportionately examined one gender. In some cultural contexts, female participants may prefer female examiners. Imbalance here may bias gender-specific findings.

#### 4. Age Group × Examiner (Grouped Bar)
- **What:** Age group distribution per examiner
- **Analysis:** If one examiner only examined 4-6 year olds (children), their DMFT will naturally be different from one who examined mostly 65-74 (elderly). This chart identifies such assignment patterns.

#### 5. Daily Patients per Examiner (Line Chart)
- **What:** Time series of daily patient count per examiner
- **Analysis:** Shows activity over time. Examiners present on all survey days indicate consistency. Gaps may indicate illness or rotation schedules.

#### 6. Productivity Distribution (Box Plot)
- **What:** Box plot of daily patient counts per examiner
- **Analysis:** The median shows typical daily workload. IQR shows consistency. Outliers (very high or very low days) may need investigation. A consistent examiner has a tight box.

#### 7. Downloadable Examiner Report (CSV)
- **What:** Comprehensive table with all metrics + caries prevalence + urgent cases per examiner
- **Analysis:** This report can be shared with field supervisors for quality control. Key red flags:
  - Very different DMFT between examiners → calibration needed
  - Very different caries prevalence → diagnostic threshold differences
  - Disproportionate urgent cases → may indicate a particularly disadvantaged cluster

---

## Tab 11: Treatment Needs & Cross-Analysis

### Purpose
Analyses **intervention urgency** and provides **cross-dimensional clinical summaries**.

### Charts

#### 1. Intervention Urgency Distribution (Donut Chart)
- **What:** Proportions of urgency categories:
  - **No treatment needed:** Oral health is satisfactory
  - **Preventive/routine:** Needs scaling, fluoride, sealants
  - **Prompt treatment:** Needs fillings, extractions within 2 weeks
  - **Immediate/urgent:** Active pain or infection — needs treatment TODAY
- **Analysis:** The most actionable chart in the entire dashboard. "Immediate" cases need referral to dental services immediately. The ratio of "prompt" to "preventive" shows whether the population needs curative or preventive interventions.

#### 2. Urgency by Age Group (Grouped Bar)
- **What:** Cross-tab of urgency × age group
- **Analysis:** 
  - If 4-6 year olds have high "immediate" rates → early childhood caries epidemic
  - If 65-74 year olds are mostly "no treatment needed" → they're edentulous (no teeth to treat, but actually worst outcome)
  - 35-44 year olds with high "prompt" → peak unmet dental need

#### 3. Urgency by Gender (Grouped Bar)
- **What:** Cross-tab of urgency × gender
- **Analysis:** Gender disparities in treatment urgency may reflect differences in care-seeking behaviour or disease severity.

#### 4. Clinical Summary Table
- **What:** Consolidated single-row metrics for all key clinical indicators
- **Analysis:** Quick reference table showing:
  - Mean DMFT, Caries prevalence
  - Fluorosis rate, Tobacco use, Oral pain rate
  - Dentist visit rate, Oral lesion rate
  - This table can be directly used in research publications and reports.

---

## Tab 12: Data Explorer

### Purpose
Provides **raw data access** for custom analysis and export.

### Features

#### 1. Filtered Data Table
- **What:** Full dataset after all sidebar filters, with internal columns (starting with `_`) removed
- **Analysis:** Enables manual inspection of individual records, verification of anomalies, and spot-checking of clinical entries.

#### 2. Download Filtered Data (CSV)
- **What:** Download button for the currently filtered and displayed data
- **Analysis:** Allows researchers to export subsets (e.g., only 12-15 year old females) for external statistical analysis in SPSS, R, or Excel.

#### 3. Column Summary Table
- **What:** For every column in the raw data: non-null count, missing %, data type, unique values
- **Analysis:** Data quality assessment. Columns with high missing % may be:
  - Age-specific (e.g., deciduous tooth status is empty for 35-44 year olds — expected)
  - Skipped by examiners (e.g., LOA not recorded — concerning)
  - Truly missing data (needs follow-up)

---

## Technical Reference

### File Structure

```
patient dashboard/
├── patient_dashboard (2).py          # Main Streamlit dashboard application
├── CentralDelhi1_DATA_LABELS_*.csv   # Data file (uploaded at runtime)
├── CentralDelhi1_DataDictionary_*.csv# Data dictionary (reference)
└── DASHBOARD_DOCUMENTATION.md        # This documentation file
```

### Dependencies

```
streamlit >= 1.30
pandas >= 2.0
plotly >= 5.15
numpy >= 1.24
```

### Running the Dashboard

```bash
python -m streamlit run "patient_dashboard (2).py"
```

### Color Palette

| Color     | Hex       | Usage                        |
| --------- | --------- | ---------------------------- |
| Blue      | `#3b82f6` | Primary, KPIs, main charts   |
| Green     | `#10b981` | Positive metrics, households |
| Purple    | `#8b5cf6` | Examiner, secondary charts   |
| Amber     | `#f59e0b` | Warning, female metrics      |
| Red       | `#ef4444` | Urgent, tobacco, caries      |
| Cyan      | `#06b6d4` | Cluster, periodontal         |
| Pink      | `#ec4899` | Supplementary                |
| Teal      | `#14b8a6` | Supplementary                |
| Orange    | `#f97316` | Supplementary                |
| Indigo    | `#6366f1` | Supplementary                |

### Key Clinical Terms Glossary

| Term                | Definition                                                            |
| ------------------- | --------------------------------------------------------------------- |
| **DMFT/dmft**       | Decayed, Missing, Filled Teeth index (permanent/deciduous)            |
| **CPI**             | Community Periodontal Index — standard measure of gum disease         |
| **LOA**             | Loss of Attachment — cumulative periodontal destruction               |
| **DMH**             | Deciduous Molar Hypomineralization — enamel defect in baby teeth      |
| **MIH**             | Molar Incisor Hypomineralization — enamel defect in first molars      |
| **TMJ**             | Temporomandibular Joint — jaw joint                                   |
| **OSMF**            | Oral Submucous Fibrosis — precancer from smokeless tobacco            |
| **ANUG**            | Acute Necrotizing Ulcerative Gingivitis — severe gum infection        |
| **Edentulous**      | Having no natural teeth                                               |
| **Malocclusion**    | Misalignment of teeth/jaws                                           |
| **Overjet**         | Horizontal distance between upper and lower front teeth               |
| **Sealant**         | Protective plastic coating applied to tooth fissures                  |
| **Scaling**         | Professional cleaning to remove calculus/tartar from teeth            |

---

*Document generated on 25 February 2026 for the Central Delhi Oral Health Survey Analytics Dashboard.*
