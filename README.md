# Data_Cleaning_Visualization_Project: Titanic 

## Introduction

The Titanic disaster is one of the most well-known maritime tragedies in history. This project analyzes passenger data from the Titanic dataset to understand factors that influenced survival outcomes.

The primary objective of this project is to perform data cleaning, preprocessing, exploratory analysis, and visualization using Python. Through this analysis, we aim to uncover meaningful patterns related to passenger demographics, ticket class, fare, and survival rates.

The project demonstrates key data analysis skills including:

* Handling missing values
* Identifying and managing data quality issues
* Detecting duplicates and outliers
* Visualizing data using Python libraries
* Extracting insights through exploratory analysis
* Communicating findings through data storytelling

---

# Dataset Information

**Source:** OpenML Titanic Dataset

The dataset contains information about Titanic passengers, including:

| Feature   | Description                       |
| --------- | --------------------------------- |
| pclass    | Passenger class (1st, 2nd, 3rd)   |
| survived  | Survival status                   |
| name      | Passenger name                    |
| sex       | Gender                            |
| age       | Age of passenger                  |
| sibsp     | Number of siblings/spouses aboard |
| parch     | Number of parents/children aboard |
| ticket    | Ticket number                     |
| fare      | Ticket fare                       |
| cabin     | Cabin number                      |
| embarked  | Port of embarkation               |
| boat      | Lifeboat number                   |
| body      | Body identification number        |
| home.dest | Home destination                  |

Initial dataset dimensions:

* Rows: 1309
* Columns: 14

---

# Tools and Technologies Used

## Programming Language

* Python

## Libraries

* Pandas
* NumPy
* Matplotlib
* Seaborn
* Scikit-Learn

## Development Environment

* Jupyter Notebook

---

# Requirements

Install the required libraries:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn
```

---

# Project Structure

```text
Data_Cleaning_Visualization_Project/
│
├── Titanic_Analysis.ipynb
├── README.md
├── images/
│   ├── missing_values_heatmap.png
│   ├── age_outliers.png
│   ├── fare_outliers.png
│   ├── survival_distribution.png
│   ├── survival_by_gender.png
│   ├── survival_by_class.png
│   ├── age_distribution.png
│   ├── fare_distribution.png
│   ├── correlation_heatmap.png
│   └── dashboard.png
│
└── requirements.txt
```

---

# Data Cleaning Process

## Step 1: Loading the Dataset

The dataset was loaded using OpenML:

```python
from sklearn.datasets import fetch_openml

titanic = fetch_openml("titanic", version=1, as_frame=True)
df = titanic.frame
```

---

## Step 2: Missing Value Analysis

Missing values were identified using:

```python
df.isnull().sum()
```

### Missing Value Summary

| Column    | Missing Values |
| --------- | -------------: |
| body      |           1188 |
| cabin     |           1014 |
| boat      |            823 |
| home.dest |            564 |
| age       |            263 |
| embarked  |              2 |
| fare      |              1 |

### Missing Percentage Analysis

| Column    | Missing % |
| --------- | --------: |
| body      |    90.76% |
| cabin     |    77.46% |
| boat      |    62.87% |
| home.dest |    43.09% |
| age       |    20.09% |
| embarked  |     0.15% |
| fare      |     0.08% |

### Missing Values Heatmap

Visualization:

```python
sns.heatmap(df.isnull(), cbar=False)
```

Observation:

The dataset contained missing values in multiple columns, particularly Cabin, Boat, Body, and Home Destination.

Insight:

Missing value analysis helped identify which columns required imputation and which columns were suitable for removal.

### Handling Missing Values

* The columns Cabin, Boat, Body, and Home Destination were removed due to a high percentage of missing values.
* Missing values in Age were replaced with the median.
* Missing values in Embarked were replaced with the mode.
* Missing values in Fare were replaced with the median.

```python
df.drop(columns=["cabin", "boat", "body", "home.dest"], inplace=True)

df["age"] = df["age"].fillna(df["age"].median())
df["embarked"] = df["embarked"].fillna(df["embarked"].mode()[0])
df["fare"] = df["fare"].fillna(df["fare"].median())
```

---

## Step 3: Duplicate Detection

Duplicate records were checked using:

```python
df.duplicated().sum()
```

Result:

```text
Duplicates: 0
```

No duplicate records were found in the dataset.

---

## Step 4: Data Type Inspection

Data types were reviewed to ensure proper analysis.

Categorical variables such as:

* Sex
* Embarked
* Passenger Class
* Survival Status

were treated as categorical features.

---

## Step 5: Outlier Analysis

Outliers were examined using boxplots.

### Age Outliers

```python
sns.boxplot(x=df["age"])
```

Observation:

* A small number of passengers had exceptionally high ages.
* These values were retained because they are realistic observations.

### Fare Outliers

```python
sns.boxplot(x=df["fare"])
```

Observation:

* Several high-fare tickets were detected.
* These likely correspond to premium first-class passengers.

Using the IQR method:

```python
Number of fare outliers: 171
```

These observations were retained because they represent genuine passenger records rather than errors.

---

# Data Visualization and Insights

## 1. Survival Distribution

Visualization:

```python
sns.countplot(data=df, x="survived")
```

### Observation

The number of passengers who did not survive was greater than the number who survived.

### Insight

The disaster resulted in a high mortality rate among passengers.

---

## 2. Survival by Gender

Visualization:

```python
sns.countplot(data=df, x="sex", hue="survived")
```

### Observation

Female passengers had significantly higher survival counts than male passengers.

### Insight

Gender was one of the strongest factors influencing survival.

This aligns with the historical evacuation policy of "women and children first."

---

## 3. Survival by Passenger Class

Visualization:

```python
sns.countplot(data=df, x="pclass", hue="survived")
```

### Observation

* First-class passengers had more survivors than deaths.
* Second-class passengers showed a relatively balanced outcome.
* Third-class passengers experienced the highest number of deaths.

### Insight

Passenger class had a strong impact on survival probability.

Passengers in higher classes had better access to lifeboats and evacuation routes.

---

## 4. Age Distribution

Visualization:

```python
sns.histplot(df["age"], bins=30, kde=True)
```

### Observation

Most passengers were between 20 and 40 years old.

### Insight

The Titanic passenger population consisted primarily of young and middle-aged adults.

---

## 5. Fare Distribution

Visualization:

```python
sns.histplot(df["fare"], bins=30, kde=True)
```

### Observation

The distribution is heavily right-skewed.

### Insight

Most passengers paid relatively low fares, while a small group paid exceptionally high ticket prices.

This indicates substantial socioeconomic differences among passengers.

---

## 6. Correlation Analysis

Visualization:

```python
sns.heatmap(numeric_df.corr(), annot=True, cmap="coolwarm")
```

### Observation

Notable relationships were observed between:

* Passenger class and fare
* Fare and survival
* Passenger class and survival

### Insight

Economic status appears to have influenced survival outcomes.

Higher-paying passengers generally had better chances of survival.

---

# Dashboard

A consolidated dashboard was created to summarize the most important findings from the Titanic dataset.

The dashboard includes:

* Missing Values Heatmap
* Age Outliers
* Fare Outliers
* Survival Distribution
* Survival by Gender
* Survival by Passenger Class
* Age Distribution
* Fare Distribution
* Correlation Heatmap

This dashboard provides a single-view summary of the complete analysis workflow.

---

# Key Findings

1. The dataset initially contained substantial missing values, particularly in Cabin, Boat, Body, and Home Destination fields.

2. No duplicate records were found in the dataset.

3. Female passengers had significantly higher survival rates than male passengers.

4. First-class passengers were much more likely to survive than passengers in lower classes.

5. Third-class passengers experienced the highest number of fatalities.

6. Most passengers were between 20 and 40 years old.

7. Fare distribution was highly skewed, indicating a large gap between lower- and higher-income passengers.

8. Passenger class and fare showed strong relationships with survival outcomes.

9. Approximately 171 fare outliers were identified, but they were retained because they represented genuine premium ticket purchases.

---

# Conclusion

This project successfully demonstrated the end-to-end process of cleaning, preprocessing, analyzing, and visualizing real-world data using Python.

Several important factors influencing survival were identified. Gender emerged as one of the strongest predictors of survival, with female passengers surviving at significantly higher rates. Passenger class also played a major role, as first-class passengers were more likely to survive than those in second or third class.

The analysis further revealed socioeconomic disparities reflected through ticket fares and class categories. These findings suggest that access to resources and location aboard the ship likely influenced evacuation outcomes.

Overall, the project highlights how data cleaning and visualization can transform raw data into meaningful insights and demonstrates practical applications of Python-based data analysis techniques.
