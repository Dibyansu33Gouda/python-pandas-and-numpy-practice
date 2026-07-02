# Hospital Patient Records — Pandas Practice

## Overview
This notebook practices core Pandas data-cleaning and analysis skills on a
simulated hospital admissions dataset. It covers missing-value handling,
datetime parsing, groupby aggregation, binning with `pd.cut`, and pivot
tables.

## Dataset
A DataFrame of 12 patient records with the following columns:

| Column        | Description                                      |
|---------------|---------------------------------------------------|
| `Patient_ID`  | Unique patient identifier                         |
| `Name`        | Patient name (some missing)                        |
| `Department`  | Hospital department (Cardiology, Orthopedics, Neurology; some missing) |
| `Age`         | Patient age (some missing)                          |
| `Bill_Amount` | Total billed amount (some missing)                  |
| `Admit_Date`  | Admission date (string → converted to datetime)    |
| `Discharged`  | Boolean — whether the patient has been discharged  |

## What the notebook does

1. **Structure check** — inspects shape, dtypes, and missing-value counts
   with `df.info()` and `df.isnull()`.
2. **Missing value handling**
   - `Name` → filled with a placeholder string.
   - `Department` → filled with the column's mode (most frequent department).
   - `Age` → filled with the column median.
   - `Bill_Amount` → filled with the mean bill *per department*, using
     `groupby().transform('mean')` so each missing value gets a
     department-specific estimate rather than a single global average.
3. **Date handling**
   - Converts `Admit_Date` from string to proper `datetime64` using
     `pd.to_datetime()`.
   - Extracts the admission month name into a new `Admit_Month` column
     with `.dt.month_name()`.
4. **Filtering & sorting** — finds patients still admitted
   (`Discharged == False`) and sorts them by `Bill_Amount` descending.
5. **Aggregation**
   - Average bill amount per department (`groupby().mean()`).
   - Department generating the highest total revenue (`groupby().sum().idxmax()`).
6. **Binning** — creates an `Age_Group` column (`Young` / `Middle-aged` /
   `Senior`) using `pd.cut()` with custom bin edges.
7. **Pivot table** — cross-tabulates patient counts by `Department` vs.
   `Discharged` status, with row/column totals via `margins=True`.
8. **Visualization** — a bar chart of total bill amount by department.

## Key techniques practiced
- `fillna()` with mode, median, and group-wise mean (`transform`)
- `pd.to_datetime()` and `.dt` accessor methods
- `groupby()` aggregation (`mean`, `sum`, `idxmax`)
- `pd.cut()` for binning continuous data into categories
- `pivot_table()` with `margins=True`
- Boolean filtering combined with `sort_values()`

## Notes / things to watch for
- `.dt` accessor only works on columns already converted to a proper
  `datetime` dtype — running it on a string column raises
  `AttributeError: Can only use .dt accessor with datetimelike values`.
- `pd.cut()` bin edges are right-inclusive by default, so boundary values
  (e.g. exactly 60) need to be assigned deliberately, not left to default
  behavior.
