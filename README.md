# 🚖 Uber Fare Analysis Project

**Course:** Introduction to Big Data Analytics (INSY 8413)  
**Instructor:** Eric Maniraguha  
**Assignment Date:** 20 July 2025  
**Groups:** A, B & E  
**Tools Used:** Python (Jupyter), Power BI Desktop  

**Name:**  
**ID:**  

---

## 📌 Introduction
This project presents an analytical exploration of the Uber Fares Dataset using Python and Power BI. The goal is to extract meaningful insights into fare behavior, time patterns, ride frequency, and geographic distribution. The final deliverables include a cleaned dataset, a full Power BI dashboard, and a comprehensive analytical report. 

---

## 🌟 Objective
To perform data cleaning, transformation, feature engineering, and exploratory data analysis on Uber ride data, then build an interactive Power BI dashboard that highlights trends in fare amounts, trip durations, peak ride hours, and geographic pickup patterns.

---

## 🛠️ Methodology

### 1. Dataset Collection
- Source: [Kaggle - Uber Fares Dataset](https://www.kaggle.com/datasets/yasserh/uber-fares-dataset)
- Format: CSV

### 2. Python Environment Setup
Required Libraries:
```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np
```

### 3. Data Loading and Cleaning
```python
df = pd.read_csv("uber.csv")
df.dropna(inplace=True)
df['pickup_datetime'] = pd.to_datetime(df['pickup_datetime'], errors='coerce')
df.columns = [col.lower().replace(' ', '_') for col in df.columns]
```

### 4. Feature Engineering
```python
df['hour'] = df['pickup_datetime'].dt.hour
df['day'] = df['pickup_datetime'].dt.day
df['month'] = df['pickup_datetime'].dt.month
df['weekday'] = df['pickup_datetime'].dt.day_name()
df['peak_time'] = df['hour'].apply(lambda h: 'Peak' if h in [7, 8, 9, 17, 18, 19] else 'Off-Peak')
```

### 5. Haversine Formula to Calculate Distance
```python
def haversine_distance(lat1, lon1, lat2, lon2):
    lat1, lon1, lat2, lon2 = map(np.radians, [lat1, lon1, lat2, lon2])
    dlat = lat2 - lat1
    dlon = lon2 - lon1
    a = np.sin(dlat / 2)**2 + np.cos(lat1) * np.cos(lat2) * np.sin(dlon / 2)**2
    c = 2 * np.arcsin(np.sqrt(a))
    return 6371.0 * c

df['trip_distance_km'] = haversine_distance(
    df['pickup_latitude'], df['pickup_longitude'],
    df['dropoff_latitude'], df['dropoff_longitude']
)
```

---

## 🔍 Analysis

### Question 1: Distribution of Fares
- Tool: Python (seaborn)
- Visual: Histogram
```python
sns.histplot(df['fare_amount'], bins=50, kde=True)
```
- Insight: Most fares fall between $5 and $15, with a few high-value outliers.

### Question 2: Outlier Identification
- Tool: Python
- Visual: Boxplot
```python
sns.boxplot(x=df['fare_amount'])
```
- Insight: Several outliers above $50, most common fares between $5 and $20.

### Question 3: Fare vs. Hour of Day
- Tool: Power BI Clustered Column Chart
- Insight: Peak fare hours occur around 8 AM and 5–7 PM.

### Question 4: Fare vs. Day of Week
- Tool: Power BI Column Chart
- Insight: Fares are higher and more variable on Fridays and weekends.

### Question 5: Geographic Distribution
- Tool: Power BI Map Visual
- Insight: High ride activity in downtown areas with frequent pickups.

### Question 6: Seasonal Trends (Month & Hour)
- Tool: Power BI Matrix or Heatmap
- Insight: High activity in summer months, especially during morning/evening.

---

## 📊 DAX Measures Used in Power BI

### 1. Ride Count Measure
```DAX
Ride Count = COUNT(uber_cleaned_for_powerbi[key])
```
Used to count number of rides instead of summing fare values.

### 2. Hour, Day, Month Columns
```DAX
Hour = HOUR(uber_cleaned_for_powerbi[pickup_datetime])
Day = DAY(uber_cleaned_for_powerbi[pickup_datetime])
Month = FORMAT(uber_cleaned_for_powerbi[pickup_datetime], "MMMM")
MonthNum = MONTH(uber_cleaned_for_powerbi[pickup_datetime])
```
Sort "Month" by "MonthNum" for correct ordering in visuals.

### 3. Day of Week Column
```DAX
Weekday = FORMAT(uber_cleaned_for_powerbi[pickup_datetime], "dddd")
WeekdayNum = WEEKDAY(uber_cleaned_for_powerbi[pickup_datetime], 2)
```
Sort "Weekday" by "WeekdayNum" to get Monday through Sunday order.

---

## 🧠 Conclusion
This project successfully transformed raw Uber ride data into valuable business insights through data wrangling, visualization, and reporting. Using Python and Power BI, we identified key fare patterns, time-based trends, and spatial concentrations, providing a solid foundation for business decision-making.

---



---

## 📂 Files Included
- `dataset/uber_cleaned_for_powerbi.csv`
- `notebook/eda_cleaning_uber.ipynb`
- `powerbi/uber_dashboard.pbix`
- `screenshots/` (loading, cleaning, visual proof)
- `README.md` (this file)
- `report_summary.md` or `report.pdf` (optional)

---

## 🔗 Submission
- GitHub Repo: https://github.com/Alain12-techn/uber-data-insights-report

