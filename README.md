# COVID-19 Data Analysis

This repository contains the analysis of COVID-19 data across different states in the USA, with a specific focus on New York. The analysis includes data fetching, cleaning, visualization, and deriving insights from the dataset.

## Dataset Description

The dataset spans from early 2020 to late 2021, providing a detailed view of COVID-19 metrics across different states and time periods. The data includes information about viral tests, confirmed and probable cases, and antigen tests, among other metrics.

### Columns in the Dataset

- `date`: Date on which data was collected
- `state`: Two-letter abbreviation for the state or territory
- `people_viral_positive`: Cumulative number of unique people with a positive PCR or other approved NAAT
- `tests_viral_positive`: Cumulative number of completed PCR tests or other approved NAAT that return positive
- `tests_viral_negative`: Cumulative number of completed PCR tests or other approved NAAT that return negative
- `encounters_viral_total`: Cumulative number of people tested per day via PCR testing or other approved NAAT
- `tests_viral_total`: Cumulative number of completed PCR tests or other approved NAAT
- `people_viral_total`: Cumulative number of unique people tested at least once via PCR testing or other approved NAAT
- `tests_combined_total`: Derived from other metrics
- `cases_conf_probable`: Total number of confirmed plus probable cases of COVID-19
- `people_antigen_positive`: Cumulative number of unique people with a completed antigen test that returned positive
- `people_antigen_total`: Cumulative number of unique people who have been tested at least once via antigen testing
- `cases_confirmed`: Total number of confirmed cases of COVID-19
- `cases_probable`: Total number of probable cases of COVID-19

## Analysis Overview

### Data Fetching and Preparation

1. **Fetching Data**:
   - The data is fetched from the JHU API and saved locally as a JSON file.
   - Example code snippet:
     ```python
     url = "https://jhucoronavirus.azureedge.net/api/v1/testing/daily.json"
     response = requests.get(url)
     if response.status_code == 200:
         data = response.json()
         with open("data.json", "w") as file:
             json.dump(data, file)
             print("Data saved successfully as data.json")
     else:
         print("Failed to fetch data from the API")
     ```

2. **Loading Data**:
   - The saved JSON file is loaded into a Pandas DataFrame for analysis.
   - Example code snippet:
     ```python
     with open("data.json", "r") as file:
         data = json.load(file)
     df = pd.DataFrame(data)
     ```

### Data Cleaning

- **Handling Missing Values**:
  - Missing values are visualized using the `missingno` library.
  - Example code snippet:
    ```python
    msno.matrix(df)
    plt.show()
    ```
  - Missing values are imputed using the `SimpleImputer` from `sklearn`.
  - Example code snippet:
    ```python
    imputer = SimpleImputer(strategy='mean')
    df_imputed = pd.DataFrame(imputer.fit_transform(df), columns=df.columns)
    ```

### Data Visualization

- **Trend Analysis**:
  - Daily trends in testing are visualized using `matplotlib` and `plotly`.
  - Example code snippet:
    ```python
    plt.figure(figsize=(10, 5))
    plt.plot(df['date'], df['tests'], label='Daily Tests')
    plt.xlabel('Date')
    plt.ylabel('Number of Tests')
    plt.title('Daily COVID-19 Tests in New York')
    plt.legend()
    plt.show()
    ```

- **Distribution Analysis**:
  - The distribution of test results is visualized using histograms and boxplots.
  - Example code snippet:
    ```python
    sns.histplot(df['tests'], kde=True)
    plt.title('Distribution of Daily Tests')
    plt.show()
    ```

### Statistical Analysis

- **Skewness and Normality**:
  - The skewness of the data is calculated to understand its distribution.
  - Example code snippet:
    ```python
    skewness = skew(df['tests'])
    print(f'Skewness: {skewness}')
    ```

### Focus on New York

A detailed analysis was conducted specifically for New York State, including:
- Filtering the dataset to include only data related to New York.
- Saving the cleaned New York data to a CSV file for further analysis.
- Plotting the cumulative number of tests and cases over time for New York.

## Notable Steps in the Process

- The choice of using the `SimpleImputer` for handling missing data is based on its simplicity and effectiveness for this dataset.
- Visualizations are created using both `matplotlib` and `plotly` to provide both static and interactive plots.
- The skewness calculation is used to check the normality of the data, which is important for certain statistical analyses.

## Repository Structure

- `NY_Corona_virus.ipynb`: Jupyter notebook containing the full analysis.
- `data.json`: JSON file containing the raw data fetched from the JHU API.
- `newyork_data_cleaned.csv`: CSV file containing the cleaned data for New York.
- `README.md`: This readme file, providing an overview of the project and instructions for replication.

## Instructions for Replication

1. **Install Required Packages**:
   ```bash
   pip install -r requirements.txt
