# COVID-19 India Data Analysis

An exploratory data analysis (EDA) project using COVID-19 case data from India. The notebook downloads the dataset from Kaggle, performs basic data inspection and cleaning, derives active cases and state-level recovery/death rates, and visualizes the states with the highest case counts and rates.

## Project Overview

This project analyzes COVID-19 data across Indian states and union territories to understand:

- The structure and quality of the dataset
- Confirmed, recovered, death, and active cases
- State-wise COVID-19 case counts
- Recovery rates by state/union territory
- Death rates by state/union territory
- States with the highest confirmed cases
- States with the highest death rates
- States with the highest recovery rates

## Dataset

The notebook uses the **COVID-19 in India** dataset available on Kaggle:

**Dataset:** `sudalairajkumar/covid19-in-india`

The dataset file used in the notebook is:

```text
covid_19_india.csv
```

The dataset is downloaded programmatically using `kagglehub`.

## Technologies & Libraries

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- KaggleHub
- Jupyter Notebook / Google Colab

## Analysis Workflow

### 1. Download the Dataset

The dataset is downloaded using KaggleHub:

```python
import kagglehub

path = kagglehub.dataset_download("sudalairajkumar/covid19-in-india")
```

### 2. Load the Data

The `covid_19_india.csv` file is loaded into a Pandas DataFrame.

```python
df = pd.read_csv(file_path)
```

### 3. Understand the Dataset

The notebook checks:

- Dataset shape
- Column names
- Data types
- Missing values
- Duplicate rows

```python
df.shape
df.columns
df.dtypes
df.isnull().sum()
df.duplicated().sum()
```

### 4. Date Conversion

The `Date` column is converted to Pandas datetime format:

```python
df["Date"] = pd.to_datetime(df["Date"])
```

### 5. Create the Recovered Column

The dataset's `Cured` column is copied into a new `Recovered` column:

```python
df["Recovered"] = df["Cured"]
```

### 6. Calculate Active Cases

Active cases are derived using:

```text
Active = Confirmed - Recovered - Deaths
```

Implemented as:

```python
df["Active"] = df["Confirmed"] - df["Recovered"] - df["Deaths"]
```

### 7. State-Level Aggregation

The data is grouped by `State/UnionTerritory`, and the maximum values of confirmed, death, recovered, and active cases are calculated:

```python
state_data = (
    df.groupby("State/UnionTerritory")
      [["Confirmed", "Deaths", "Recovered", "Active"]]
      .max()
      .reset_index()
)
```

This produces a state-level summary dataset.

### 8. Recovery Rate

The recovery rate is calculated as:

```text
Recovery Rate = (Recovered / Confirmed) × 100
```

```python
state_data["Recovery_Rate"] = (
    state_data["Recovered"] /
    state_data["Confirmed"].replace(0, np.nan) * 100
)
```

### 9. Death Rate

The death rate is calculated as:

```text
Death Rate = (Deaths / Confirmed) × 100
```

```python
state_data["Death_Rate"] = (
    state_data["Deaths"] /
    state_data["Confirmed"].replace(0, np.nan) * 100
)
```

Both rates are rounded to two decimal places.

## Visualizations

The notebook creates the following visualizations:

### Total Confirmed Cases by State

A bar chart comparing the maximum confirmed cases across states and union territories.

### Top 10 States by Confirmed Cases

A bar chart showing the 10 states/union territories with the highest confirmed case counts.

### Top 10 States by Death Rate

A bar chart showing the states/union territories with the highest calculated death rates.

### Top 10 States by Recovery Rate

A bar chart showing the states/union territories with the highest calculated recovery rates.

## Project Structure

```text
.
├── Untitled5 (1).ipynb
└── README.md
```

## How to Run

### 1. Clone the repository

```bash
git clone <your-repository-url>
cd <your-repository-name>
```

### 2. Install dependencies

```bash
pip install pandas numpy matplotlib seaborn kagglehub jupyter
```

### 3. Open the notebook

```bash
jupyter notebook
```

Open:

```text
Untitled5 (1).ipynb
```

Run the cells sequentially.

> **Note:** KaggleHub may require Kaggle access/authentication depending on the environment and dataset availability.

## Key Concepts Demonstrated

- Exploratory Data Analysis (EDA)
- Data loading with Pandas
- Data inspection
- Missing-value checking
- Duplicate checking
- Datetime conversion
- Feature/column creation
- GroupBy aggregation
- Rate calculation
- Sorting and filtering
- Data visualization
- Bar charts using Seaborn and Matplotlib

## Important Implementation Note

The notebook contains:

```python
df.drop_duplicates()
```

This expression does not modify `df` because the result is not assigned back to the DataFrame and `inplace=True` is not used.

To actually remove duplicates, it would need to be written as:

```python
df = df.drop_duplicates()
```

or:

```python
df.drop_duplicates(inplace=True)
```

This README describes the notebook as it currently exists rather than assuming that duplicate removal has been applied.

## Disclaimer

This project is intended for educational and exploratory data-analysis purposes. The calculated rates and visualizations reflect the data and processing performed in the notebook and should not be treated as medical or epidemiological conclusions.
