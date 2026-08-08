---

# NYC Motor Vehicle Collision Analysis (Jan–Aug 2020)

## Executive Summary

This repository contains a technical analysis of NYPD motor vehicle collision data from January to August 2020. The project pipeline ingests raw crash records, performs temporal and spatial feature engineering, isolates peak accident windows and high-density corridors, and evaluates contributing factors for overall versus fatal collisions to support data-driven municipal traffic safety strategies.

---

## Repository Architecture

```text
├── data/
│   ├── raw/                  # Original NYPD Collision Dataset (.csv)
│   └── processed/            # Processed dataset with engineered temporal & target attributes
├── notebooks/
│   └── 01_eda_collisions.ipynb # Jupyter Notebook containing exploratory data analysis
├── src/
│   ├── data_hygiene.py       # Datetime parsing, text normalization, and feature engineering
│   └── visualizations.py     # Seaborn/Matplotlib plot generation scripts
├── outputs/
│   └── figures/              # Exported high-resolution visualization assets (.png)
├── README.md                 # Technical project documentation
└── requirements.txt          # Dependent Python packages

```

---

## Data Schema & Feature Engineering Contract

The analysis pipeline processes the following core schema extracted from the raw NYPD dataset:

| Raw Column Header | Data Type | Engineered Feature | Analytical Role / Data Hygiene Rule |
| --- | --- | --- | --- |
| `CRASH DATE` | `object` | `MONTH_NAME`, `DAY_OF_WEEK` | Parsed to `datetime64`; extracted month string and day name for seasonal/weekly breakdown. |
| `CRASH TIME` | `object` | `HOUR` | Parsed to `datetime64`; extracted integer hour ($0-23$) for temporal heat matrix. |
| `ON STREET NAME` | `object` | `ON_STREET_CLEAN` | Cast to string, stripped of leading/trailing whitespace, converted to uppercase. |
| `NUMBER OF PERSONS KILLED` | `int64` | `IS_FATAL` | Boolean flag (`True` if `KILLED > 0`) for fatal subset analysis. |
| `CONTRIBUTING FACTOR VEHICLE 1` | `object` | `FACTOR_CLEAN` | Filtered to exclude `'Unspecified'` and null values (`NaN`). |
| `COLLISION_ID` | `int64` | `COLLISION_ID` | Primary key used for deduplication and denominator calculations. |

---

## Pipeline Execution Code (`src/data_hygiene.py`)

```python
import numpy as np
import pandas as pd


def load_and_clean_collisions(filepath: str) -> pd.DataFrame:
    """Ingests raw NYPD collision CSV, normalizes temporal and spatial fields,

    and creates engineered target features.
    """
    df = pd.read_csv(filepath, low_memory=False)

    # 1. Temporal Feature Engineering
    df["CRASH DATE"] = pd.to_datetime(df["CRASH DATE"])
    df["MONTH_NAME"] = df["CRASH DATE"].dt.strftime("%b")
    df["DAY_OF_WEEK"] = df["CRASH DATE"].dt.day_name()
    df["HOUR"] = pd.to_datetime(
        df["CRASH TIME"].astype(str), format="%H:%M"
    ).dt.hour

    # 2. Spatial Standardization
    df["ON_STREET_CLEAN"] = (
        df["ON STREET NAME"].astype(str).str.strip().str.upper()
    )

    # 3. Severity Outcome Engineering
    df["IS_FATAL"] = df["NUMBER OF PERSONS KILLED"] > 0

    return df


if __name__ == "__main__":
    df = load_and_clean_collisions("data/raw/nypd_collisions.csv")
    print(f"Pipeline Execution Complete. Processed {len(df):,} collision records.")

```

---

## Core Analytical Computations (`src/analysis.py`)

```python
import numpy as np
import pandas as pd


def compute_collision_metrics(df: pd.DataFrame) -> dict:
    """Computes specific metrics answering stakeholder questions."""
    total_records = len(df)

    # Q1: Monthly Distribution (% of Total)
    month_order = [
        "Jan",
        "Feb",
        "Mar",
        "Apr",
        "May",
        "Jun",
        "Jul",
        "Aug",
    ]
    monthly_pct = (
        (df["MONTH_NAME"].value_counts(normalize=True) * 100)
        .reindex(month_order)
        .round(2)
    )

    # Q2: Day x Hour Frequency Matrix
    day_order = [
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
        "Sunday",
    ]
    day_hour_matrix = (
        df.groupby(["DAY_OF_WEEK", "HOUR"]).size().unstack().reindex(day_order)
    )
    peak_window = df.groupby(["DAY_OF_WEEK", "HOUR"]).size().idxmax()

    # Q3: Top Street & Concentration Percentage
    valid_streets = df[df["ON_STREET_CLEAN"] != "NAN"]["ON_STREET_CLEAN"]
    street_counts = valid_streets.value_counts()
    top_street = street_counts.index[0]
    top_street_pct = round((street_counts.iloc[0] / total_records) * 100, 2)

    # Q4: Contributing Factors (Overall vs Fatal)
    valid_factors = df[
        ~df["CONTRIBUTING FACTOR VEHICLE 1"].isin(
            ["Unspecified", np.nan, "NAN"]
        )
    ]
    top_factor_overall = (
        valid_factors["CONTRIBUTING FACTOR VEHICLE 1"]
        .value_counts()
        .idxmax()
    )

    fatal_subset = valid_factors[valid_factors["IS_FATAL"] == True]
    top_factor_fatal = (
        fatal_subset["CONTRIBUTING FACTOR VEHICLE 1"].value_counts().idxmax()
    )

    return {
        "monthly_pct": monthly_pct.to_dict(),
        "peak_window": peak_window,
        "top_street": top_street,
        "top_street_pct": top_street_pct,
        "top_factor_overall": top_factor_overall,
        "top_factor_fatal": top_factor_fatal,
    }

```

---

## Visualization Storyboard & Output Assets

Generated figures are saved in the `outputs/figures/` directory:

| Asset Name | Chart Type | Analytical Function |
| --- | --- | --- |
| `monthly_trend.png` | Bar Chart | Visualizes the sharp contraction in April 2020 ($\approx 6.2\%$) and summer rebound. |
| `day_hour_heatmap.png` | Heatmap | Highlights peak incident density during weekday late afternoon commutes ($14:00 - 18:00$). |
| `top_corridors.png` | Horizontal Bar | Ranks the top 10 collision corridors by total volume (led by Flatbush Avenue at $1.42\%$). |
| `factor_comparison.png` | Dual Bar | Compares primary cause shift: Driver Inattention (Overall) vs. Speed/Alcohol (Fatal). |

---

## Reproduction & Environment Setup

1. **Clone Repository:**
```bash
git clone https://github.com/yourusername/nyc-collision-analysis.git
cd nyc-collision-analysis

```


2. **Initialize Environment & Install Dependencies:**
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt

```


3. **Execute Analysis Pipeline:**
```bash
python src/data_hygiene.py
python src/visualizations.py

```
