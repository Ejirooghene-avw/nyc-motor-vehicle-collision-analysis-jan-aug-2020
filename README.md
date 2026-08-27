# NYC Motor Vehicle Collisions Analysis (2020)

An end-to-end data analysis project examining urban traffic collision patterns in New York City during 2020. This project explores temporal trends, high-risk spatial corridors, and key drivers of crash severity using Python (`pandas`, `matplotlib`, `seaborn`).

---

## Executive Summary

An analysis of NYPD motor vehicle collision data reveals critical patterns in citywide traffic risk:

* **Mid-Year Contraction:** Crash volume peaked in early winter (**January: 19.1%**, **February: 18.3%**) before experiencing a severe drop in **April (5.5%)** due to municipal lockdown measures—a **71.2% reduction** from January.
* **Peak Risk Windows:** Collisions follow a commuting structure, with the late afternoon homeward commute (**14:00–18:00**) generating the highest volume. **Friday at 16:00** recorded the absolute peak (**870 crashes**).
* **High-Risk Corridors:** Expressways dominate collision counts, led by **Belt Parkway**, which alone accounts for **~1.7%** of all citywide crashes.
* **Volume vs. Severity Drivers:** While **Driver Inattention** is the leading cause of overall collision volume (**34.7%**), severe outcomes are driven by speed and signal violations—**Unsafe Speed (32.0%)** and **Traffic Control Disregarded (14.6%)** are the leading factors in fatal accidents.

---

## Key Business Questions & Analytical Insights

### 1. Monthly Trends & Seasonal Patterns

Incident frequency was heavily front-loaded before contracting during spring lockdowns. Volume steadily recovered from May through July before stabilizing in August well below pre-pandemic baseline levels.

* **January & February:** Accounted for **37.4%** of total reported accidents.
* **April Trough:** Dropped to **5.5%** of annual volume (**71.2% reduction** from January).
* **Recovery Phase:** May (**8.2%**), June (**10.7%**), July (**12.3%**), and August (**11.7%**).

---

### 2. Temporal Distribution (Day & Hour)

Accident frequency is concentrated during evening commute hours, where traffic density and driver fatigue intersect.

* **High-Risk Day:** **Friday** registered the highest overall crash total (**12,271 collisions**).
* **Peak Hour:** **16:00 (4:00 PM)** on Friday experienced the highest single concentration (**870 collisions**).
* **Commute Contrast:** Late afternoon volume (**14:00–18:00**) is nearly **3x higher** than morning rush-hour volume (**08:00–09:00**).

---

### 3. Spatial Risk & High-Accident Corridors

Multi-lane, high-speed roadways account for a disproportionate share of collisions across the five boroughs.

| Rank | Street / Corridor | % of Total Crashes | Risk Profile |
| --- | --- | --- | --- |
| **1** | **BELT PARKWAY** | **~1.66%** | High-speed multi-lane parkway |
| **2** | **LONG ISLAND EXPRESSWAY** | **0.99%** | Major inter-borough commuter arterial |
| **3** | **BROOKLYN QUEENS EXPRESSWAY** | **0.84%** | Dense freight and passenger corridor |

---

### 4. Contributing Factors: Volume vs. Fatality

Filtering out uninformative entries reveals a clear distinction between factors that cause general accidents versus those that result in fatalities:

* **Overall Collisions:** **Driver Inattention/Distraction** (**34.7%**) dominates general crash incidents.
* **Fatal Collisions:** **Unsafe Speed** (**32.0%**) and **Traffic Control Disregarded** (**14.6%**) combine for nearly **46.6%** of all fatal events.

> **Takeaway:** Routine distraction drives overall collision frequency, but velocity and signal non-compliance drive lethality.

---

## Actionable Recommendations

1. **Targeted Speed Enforcement:** Deploy automated speed monitoring and patrol units along the **Belt Parkway** and **Long Island Expressway**, specifically during Friday afternoon peak windows (**14:00–18:00**).
2. **Infrastructure & Signal Safety:** Re-engineer high-risk arterial intersections to deter red-light running, directly addressing the secondary driver of fatal crashes.
3. **Public Awareness Campaigns:** Focus safety campaigns on driver distraction during evening commutes rather than morning travel.

---

## Repository Structure

```text
nyc-vehicle-collisions-2020/
├── data/
│   └── cleaned_nyc_collisions_2020.csv
├── exports/
│   ├── monthly_crash_trends.csv
│   ├── top_corridors.csv
│   ├── day_hour_matrix.csv
│   └── contributing_factors_summary.csv
├── visuals/
│   ├── 1_monthly_trajectory.png
│   ├── 2_top_corridors.png
│   ├── 3_risk_heatmap.png
│   └── 4_contributing_factors.png
├── notebooks/
│   └── nyc_collision_analysis.ipynb
├── README.md
└── requirements.txt

```

---

## Technical Stack & Methodology

* **Language & Environment:** Python 3.10+, Jupyter Notebook
* **Data Manipulation:** `pandas`, `numpy`
* **Visualization:** `matplotlib`, `seaborn`
* **Data Processing Highlights:**
* Cleaned and standardized street names across multi-borough entries.
* Extracted temporal attributes (Month, Day of Week, Hour) from standard timestamp strings.
* Filtered out generic/unspecified causes (`Unspecified`, `Paved Skids`, etc.) to isolate actionable contributing factors.



---

## Getting Started

1. **Clone the repository:**
```bash
git clone https://github.com/your-username/nyc-vehicle-collisions-2020.git
cd nyc-vehicle-collisions-2020

```


2. **Install dependencies:**
```bash
pip install -r requirements.txt

```


3. **Run the Notebook:**
```bash
jupyter notebook notebooks/nyc_collision_analysis.ipynb

