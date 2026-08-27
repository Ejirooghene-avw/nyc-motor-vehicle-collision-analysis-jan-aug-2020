# nyc-motor-vehicle-collision-analysis-jan-aug-2020

An end-to-end data analysis project examining urban traffic collision patterns across New York City during the first eight months of 2020. This project analyzes seasonal fluctuations, high-risk spatial corridors, and drivers of crash severity using Python (`pandas`, `matplotlib`, `seaborn`).

---

## Executive Summary

* **Mid-Year Contraction:** Crash volume was highest during early winter (**January: 19.1%**, **February: 18.3%**) before dropping to an absolute trough in **April (5.5%)** due to municipal lockdown measures—representing a **71.2% reduction** from January baseline levels.
* **Commuter Risk Windows:** Collisions follow urban commuting patterns, with peak risk concentrated during the late afternoon homeward commute (**14:00–18:00**). **Friday at 16:00** recorded the absolute single highest risk point (**870 collisions**).
* **Expressway Spatial Risk:** Multi-lane, high-speed arterial expressways account for the highest spatial crash density across the five boroughs, led by **BELT PARKWAY** (**~2.24%** of total street-attributed crashes) and **LONG ISLAND EXPRESSWAY** (**~1.34%**).
* **Volume vs. Severity Factors (Excluding Unspecified Data):** Filtering out generic/unspecified records reveals that routine crash volume is driven by **Driver Inattention/Distraction (34.7%)**. In contrast, fatal incidents are dominated by high-velocity and signal non-compliance factors—specifically **Unsafe Speed (32.0%)** and **Traffic Control Disregarded (14.6%)**.

---

## Key Business Questions & Analytical Insights

### 1. Monthly Trends & Seasonal Variation
Incident frequency experienced a severe spring contraction rather than traditional seasonal cycles, directly aligning with NYC stay-at-home mandates.

* **Winter Peak:** January (**19.1%**) and February (**18.3%**) combined for **37.4%** of all reported crashes.
* **Spring Trough:** Volume dropped to **5.5%** in April (**71.2% drop** from January).
* **Summer Recovery:** Volume stabilized from June (**10.2%**) to August (**11.7%**), remaining below early-year baselines.

<img src="visuals/1. monthly_crash_trend.png" alt="Monthly Crash Trend" width="100%">

---

### 2. Temporal Risk Distribution (Day & Hour)
Traffic risk is heavily concentrated during late afternoon hours when commuter volume and driver fatigue intersect.

* **High-Risk Day:** **Friday** registered the highest total volume (**12,271 crashes**).
* **Peak Risk Hour:** **Friday at 16:00 (4:00 PM)** reached the peak incident count (**870 collisions**).
* **Commute Contrast:** Afternoon rush volume (**14:00–18:00**) is nearly **3x higher** than morning rush volume (**08:00–09:00**).

<img src="visuals/2. accident_frequency.png" alt="Accident Frequency Heatmap" width="100%">

---

### 3. Spatial Risk & High-Accident Corridors
High-speed parkways and expressways constitute the primary spatial risk corridors across the city:

| Rank | Street / Corridor | % Share of Attributed Crashes | Corridor Type |
| :--- | :--- | :--- | :--- |
| **1** | **BELT PARKWAY** | **2.24%** | Multi-lane arterial parkway |
| **2** | **LONG ISLAND EXPRESSWAY** | **1.34%** | Major inter-borough highway |
| **3** | **BROOKLYN QUEENS EXPRESSWAY** | **1.33%** | Dense freight and commuter highway |
| **4** | **FDR DRIVE** | **1.31%** | Manhattan waterfront express corridor |

<img src="visuals/3. top_high_accident.png" alt="Top High-Accident Corridors" width="100%">

---

### 4. Contributing Factors: Volume vs. Fatality
Filtering out uninformative entries (*Unspecified*) isolates true underlying crash behaviors:

* **Overall Collisions (Volume Driver):** **Driver Inattention/Distraction** leads overall incidents at **34.7%**.
* **Fatal Collisions (Severity Drivers):** **Unsafe Speed** (**32.0%**) and **Traffic Control Disregarded** (**14.6%**) account for nearly **46.6%** of all fatal events.

<img src="visuals/4. crash_factors.png" alt="Crash Factors Comparison" width="100%">

> **Key Takeaway:** Cognitive distraction drives overall crash volume, but excessive speed and red-light running drive lethality.

---

## Actionable Recommendations

1. **Targeted Speed Enforcement:** Deploy automated speed enforcement and traffic patrols along the **Belt Parkway** and **Long Island Expressway**, specifically during Friday afternoon peak hours (**14:00–18:00**).
2. **Intersection Infrastructure:** Upgrade high-risk arterial intersections with automated red-light cameras to mitigate signal non-compliance.
3. **Commute-Focused Campaigns:** Target public awareness campaigns toward driver distraction during evening homeward commutes.

---

## Repository Structure

```text
nyc-motor-vehicle-collision-analysis-jan-aug-2020/
├── data/
│   ├── cleaned.csv
│   └── raw.csv
├── exports/
│   ├── comp_df.csv
│   ├── day_hour_matrix_formatted.csv
│   ├── monthly_crash_pct.csv
│   └── top_streets.csv
├── notebooks/
│   ├── collision_analysis_final.ipynb
│   └── data_cleanup_final.ipynb
├── visuals/
│   ├── 1. monthly_crash_trend.png
│   ├── 2. accident_frequency.png
│   ├── 3. top_high_accident.png
│   └── 4. crash_factors.png
├── README.md
└── requirements.txt

```

---

## Technical Stack & Methodology

* **Environment:** Python 3.10+, Jupyter Notebook
* **Libraries:** `pandas`, `numpy`, `matplotlib`, `seaborn`
* **Data Processing Highlights:**
* Standardized and cleaned street names across all five boroughs.
* Extracted temporal attributes (Month, Day of Week, Hour) from collision timestamps.
* Isolated valid records by filtering out generic and uninformative contributing factor entries (`Unspecified`).



---

## Getting Started

### 1. Clone the Repository & Install Dependencies

```bash
git clone [https://github.com/Ejirooghene-avw/nyc-motor-vehicle-collision-analysis-jan-aug-2020.git](https://github.com/Ejirooghene-avw/nyc-motor-vehicle-collision-analysis-jan-aug-2020.git)
cd nyc-motor-vehicle-collision-analysis-jan-aug-2020
pip install -r requirements.txt

```

### 2. Choose Your Workflow Notebook

Launch Jupyter Notebook or open the workspace in your preferred IDE:

```bash
jupyter notebook

```

You can run either notebook depending on your analytical focus:

* **Data Cleaning Pipeline (`notebooks/data_cleanup_final.ipynb`):** Examines raw NYPD data, handles missing attributes, standardizes street naming conventions, and exports cleaned CSV datasets.
* **Core Analysis & Visualization (`notebooks/collision_analysis_final.ipynb`):** Analyzes seasonal shifts, hourly commute spikes, expressway spatial risk, and contributing factor severity.

> **Note on Data Access:** Both notebooks load datasets directly from the local `data/` directory using relative paths (`data/raw.csv` and `data/cleaned.csv`). Because pre-processed files are included in the repository, you can launch and execute `collision_analysis_final.ipynb` immediately without needing to run the cleaning pipeline first.
