# King County Housing EDA — Jacob Phillips

Exploratory data analysis of King County, WA house sales data to identify properties matching a specific client brief.

## Client Brief

**Client:** Jacob Phillips

**Requirements:**
- Unlimited budget
- 4+ bathrooms (or smaller house nearby)
- Large lot (room for tennis court & pool)
- Near a golf course
- Historic area
- No waterfront

## Project Structure

| File | Description |
|------|-------------|
| [00_Final-Notebook.ipynb](00_Final-Notebook.ipynb) | Main analysis notebook with all steps and conclusions |
| [03_fetching_the_data_eda.ipynb](03_fetching_the_data_eda.ipynb) | Connects to PostgreSQL and loads the dataset |
| [01_assignment.md](01_assignment.md) | Project brief and deliverables |
| [02_workflow.md](02_workflow.md) | EDA workflow guide |
| [column_names.md](column_names.md) | Data dictionary |
| [king_county_historic_golf_zips.csv](king_county_historic_golf_zips.csv) | Hand-curated list of zipcodes near golf courses |
| [data/](data/) | Dataset CSVs (not tracked in git) |

## Analysis Overview

### Step 1 — Data Preparation
- Loaded and merged two tables: house details and sales
- Cleaned missing values (`waterfront`, `view`, `sqft_basement`, `yr_renovated` filled with 0)
- Converted columns to appropriate types (`bedrooms`, `floors` to int)
- Plotted distributions of all features

### Step 2 — Feature Correlation
- Identified 5 features most correlated with price (correlation > 0.5): `sqft_living`, `grade`, `sqft_above`, `bathrooms`, `sqft_living15`
- Built scatter matrix and correlation heatmaps

### Step 3 — Hypotheses

**Hypothesis 1 — Waterfront premium**
> ZIP codes with waterfront have higher average price per sqft.

Introduced `px_per_sqft` metric. Waterfront adds the largest price premium. View rating has inconsistent effect, likely due to subjective/inconsistent data entry.

**Hypothesis 2 — Bathrooms correlate with historical areas**
> Areas with older housing stock tend to have more bathrooms.

Defined "historical area" as average year built per zipcode. Found moderate positive correlation (0.58 overall, 0.62 for renovated houses, 0.40 for non-renovated). Renovations drive the relationship — older homes that were renovated tend to gain bathrooms. Identified a dip in bathroom count for houses built 1944–1989 (peak dip ~1953), likely linked to post-war construction styles and economic conditions.

**Hypothesis 3 — Year built correlates with price per sqft**
> Older houses have lower price per sqft.

Not confirmed. Correlation is weak and slightly negative overall. Likely explained by older houses being located in currently premium, central, or landmark areas — location outweighs age.

### Step 4 — Property Suggestions
- Filtered to houses built before 1920: ~1,450 properties
- Applied client filters: no waterfront, 4+ bathrooms, large lot
- Cross-referenced with golf course zipcodes to find intersection of historical and golf-adjacent areas

## Setup

```bash
git clone <repo-url>
cd ds-eda-project-template
uv sync
cp .env.example .env  # fill in database credentials
code .
```

Select the `.venv` kernel when opening notebooks in VS Code.

## Data Source

[House Sales in King County — Kaggle](https://www.kaggle.com/datasets/harlfoxem/housesalesprediction)
