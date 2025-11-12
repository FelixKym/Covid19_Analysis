# COVID-19 Analysis Project - AI Coding Agent Guide

## Project Overview
This is a **data analysis project** using Jupyter Notebook to explore US COVID-19 statistics by state. The analysis focuses on epidemiological patterns and resource distribution fairness using Python data science tools.

## Key Files & Architecture

- **`covid.ipynb`** - Main Jupyter Notebook containing 8 research questions explored through pandas, matplotlib, seaborn, and plotly visualizations
- **`covid_data.csv`** - Source dataset with COVID metrics per state (population, cases, deaths, tests, recovery rates, etc.)
- **`README.md`** - High-level project documentation with methodology overview

## Data Pipeline & Workflow

### Data Loading & Preparation (Early Notebook Cells)
1. Import libraries: numpy, pandas, seaborn, matplotlib, plotly
2. Load `covid_data.csv` with absolute paths (⚠️ **Note**: paths may differ locally)
3. Set `USA State` column as index for state-based analysis
4. Handle missing values with `fillna(0)` before analysis

### Derived Metrics Pattern
The notebook creates calculated columns from raw data:
- `case_fatality_rate` = Total Deaths / Total Cases
- `death_case_ratio(%)` = (Total Deaths / Total Cases) * 100
- `active_case_rate/1M` = Active Cases normalized per million
- `Recovery Rate %` = (Total Recovered / Total Cases) * 100
- `Tests per Capita` = Total Tests / Population

**Pattern**: Always apply calculations *after* setting State index and filling NaN values.

## Analysis Patterns & Common Methods

### Correlation Analysis
- Use `df["col1"].corr(df["col2"])` for Pearson coefficient
- Example: Testing rate vs death rates show **negative correlation** (-0.xx) = higher testing = lower deaths
- Interpret: 0.99 = very strong positive; 0.2 = weak positive; negative values = inverse relationship

### Visualization Hierarchy
1. **Bar Charts** (horizontal `barh`) - For ranking states by metric (CFR, death ratio, recovery rate)
2. **Scatter + Regression** - For correlation visualization with `sns.regplot()` (red trend line standard)
3. **Choropleth Maps** - For spatial distribution using `plotly.express.choropleth()` with state abbreviations
4. **Lorenz Curve** - For inequality analysis (testing fairness)

### State Abbreviation Mapping
The notebook includes a hardcoded dictionary `us_state_abbrev` mapping full state names to 2-letter codes. Use this for all choropleth maps via `df['State Abbrev'] = df.index.map(us_state_abbrev)`.

## Project-Specific Conventions

### Matplotlib/Figure Sizing
- Standard figure size: `figsize=(12, 8)` for general plots
- Tall bar charts: `figsize=(12, 15)` for all 50 states
- `plt.tight_layout()` always applied before `plt.show()`
- Grid styling: `linestyle='--', alpha=0.5-0.9` for consistency

### Plotly Choropleth Configuration
- Background: `geo=dict(bgcolor='black')` (dark theme)
- Title center alignment: `title_x=0.5`
- Standard size: `width=1200, height=800-900`

### Markdown Documentation
Each analysis section follows pattern:
1. Question as H2 heading (## Q#. Question?)
2. Code cells performing analysis
3. Summary markdown with key findings in #### heading

## Critical Developer Notes

- **Path Issue**: The notebook has hardcoded absolute path `D:\Projects\Covid\covid_data.csv`. Update to relative path `covid_data.csv` when running locally.
- **State Index Critical**: Many operations assume State is the index. Never reset index without updating dependent code.
- **Plotly Import**: Required for choropleth maps - already imported in notebook.
- **Correlation Interpretation**: Always document if finding is positive/negative and the practical meaning (e.g., "higher testing associated with lower deaths").

## When Extending Analysis
- Follow the 8-question structure for new analyses
- Derive new metrics from existing columns following the multiplication pattern
- Use appropriate visualization for metric type (ranking=bar, relationship=scatter, geography=choropleth)
- Always include interpretation markdown after visualizations
- Update README.md if adding major questions or findings
