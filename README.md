# 🌍 Global Plastic Waste Analysis

A data analysis notebook exploring per-capita plastic waste generation and mismanagement across countries, using year-2010 data cross-referenced with GDP and population statistics.

---

## 📁 Dataset Requirements

This notebook requires **two CSV files** in the same directory as the notebook:

| File | Description |
|------|-------------|
| `per-capita-plastic-waste-vs-gdp-per-capita.csv` | Per-capita plastic waste (kg/person/day) by country, year, GDP, population, and continent |
| `per-capita-mismanaged-plastic-waste-vs-gdp-per-capita.csv` | Per-capita mismanaged plastic waste by country, year, GDP, and population |

> Both datasets are sourced from [Our World in Data](https://ourworldindata.org/plastic-pollution) and cover multiple years across 48,000+ entity-year rows.

---

## 🛠️ Dependencies

Install all required packages via pip:

```bash
pip install numpy pandas seaborn matplotlib
```

| Library | Version (recommended) | Purpose |
|---|---|---|
| `numpy` | ≥ 1.21 | Numerical operations and rounding |
| `pandas` | ≥ 1.3 | Data loading, cleaning, and merging |
| `seaborn` | ≥ 0.11 | Regression scatter plots |
| `matplotlib` | ≥ 3.4 | Figure rendering and customization |

**Python version:** 3.7+

---

## ▶️ How to Execute

### Option 1 — Jupyter Notebook (recommended)

```bash
# Install Jupyter if not already available
pip install notebook

# Launch the notebook
jupyter notebook global-plastic-waste.ipynb
```

Then run all cells sequentially via **Cell → Run All**, or step through each cell with **Shift + Enter**.

### Option 2 — JupyterLab

```bash
pip install jupyterlab
jupyter lab global-plastic-waste.ipynb
```

### Option 3 — VS Code

Open the `.ipynb` file in VS Code with the **Jupyter extension** installed and click **Run All**.

> ⚠️ Make sure both CSV files are in the **same working directory** as the notebook before running. If not, update the `pd.read_csv(...)` paths in cells 1 and 19 accordingly.

---

## 🔄 Pipeline Overview

The notebook executes the following steps in order:

```
Load CSVs → Inspect & Clean → Filter to Year 2010 → Merge Datasets → Feature Engineering → Visualize
```

1. **Load** both CSV files into DataFrames
2. **Inspect** shape, null counts, and column types
3. **Clean** — rename verbose column names, drop rows missing both GDP and population
4. **Filter** — subset to year 2010 for both datasets; assign continent labels from the 2015 slice
5. **Drop** rows with missing continent or missing waste values
6. **Merge** the two 2010-filtered DataFrames on common entity columns
7. **Engineer features** — compute `Total waste (kgs/year)` and `Total waste mismanaged (kgs/year)` by scaling per-capita values by population × 365
8. **Visualize** relationships between GDP and waste metrics

---

## 📊 Results

### Final Dataset — `df_plastic_waste`

The merged, cleaned dataset covers countries in the year 2010 with the following columns:

| Column | Description |
|--------|-------------|
| `Entity` | Country or region name |
| `Code` | ISO 3-letter country code |
| `Year` | Fixed at 2010 |
| `Waste per person(kg/day)` | Total plastic waste generated per capita |
| `Mismanaged waste per person(kg/day)` | Plastic waste not properly disposed of per capita |
| `GDP per capita in PPP` | GDP per capita (constant 2011 international $) |
| `Total Population` | Country population (integer) |
| `Continent` | Continent label |
| `Total waste(kgs/year)` | Derived — national total waste in kg per year |
| `Total waste mismanaged(kgs/year)` | Derived — national total mismanaged waste in kg per year |

---

### Key Findings

#### 1. Mismanaged Waste vs. GDP
A regression scatter plot reveals a **strong negative correlation** — countries with lower GDP per capita tend to have significantly higher rates of mismanaged plastic waste per person. As GDP rises, mismanagement rates drop sharply and then plateau at low levels.

#### 2. Total Waste Generated vs. GDP
A second regression plot shows a **positive correlation** — higher-income countries tend to generate more plastic waste per person overall. This reflects higher consumption patterns in wealthier economies, even if they manage it better.

#### 3. Null Value Distribution (raw data)
- `Waste per person(kg/day)`: ~99.6% null in the raw file — real values exist only for select country-years (primarily 2010)
- `Continent`: ~99.4% null — populated only for 2015 rows, which are transferred to the 2010 slice
- `GDP per capita in PPP`: ~86.7% null across all years

#### 4. Data Quality Notes
- Rows missing both GDP and population are dropped early
- Continent labels are imputed from the 2015 data slice into the 2010 slice by positional alignment
- Final analysis covers only countries with complete waste, continent, and population data

---

## 📂 File Structure

```
project/
│
├── global-plastic-waste.ipynb                               # Main analysis notebook
├── per-capita-plastic-waste-vs-gdp-per-capita.csv           # Dataset 1 (required)
├── per-capita-mismanaged-plastic-waste-vs-gdp-per-capita.csv # Dataset 2 (required)
└── README.md
```

---

## 📌 Notes

- The inline magic `%matplotlib inline` renders plots directly in the notebook output.
- `warnings.filterwarnings('ignore')` suppresses pandas/seaborn deprecation noise.
- Two cells at the end of the notebook are empty and can be used for further exploration.
