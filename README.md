# NAME:RITHIK RAM.S
# REGISTER NUMBER:212224230229

**EXP 3 - Delhi Air Quality Analysis**

**Aim**


To compare air quality parameters in Delhi across different stations and analyze the relationship between pollutants (e.g., PM2.5 and NO₂) using scatter plots and correlation analysis.


**Procedure / Algorithm**

1)Load the dataset using pandas.

2)Preprocess the data:

3)Convert the date column (period.datetimeFrom.utc) to datetime format.

4)Drop missing or invalid values.

5)Pivot the dataset so each pollutant (parameter) becomes a separate column.

6)Plot scatter plot between PM2.5 and NO₂ to study their relationship.

7)Plot correlation heatmap between all pollutants to identify relationships.

8)Interpret the results — identify which pollutants are correlated and which stations are most polluted.


**Program**
```
import matplotlib.pyplot as plt
import pandas as pd
import seaborn as sns
import numpy as np
df=pd.read_csv('delhi_pm25_aqi.csv')
df
```
<img width="1030" height="595" alt="image" src="https://github.com/user-attachments/assets/1f1fab0f-0c8a-4e4b-b745-40e027715b02" />


```
df.info()
```
<img width="763" height="281" alt="image" src="https://github.com/user-attachments/assets/cb8d0172-fb3f-4af0-918e-4eb2d4f74173" />
>

```
df.shape
```
<img width="618" height="50" alt="image" src="https://github.com/user-attachments/assets/432a7c52-48fe-46dc-8952-1c689709b793" />

```
df.describe()
```
<img width="754" height="423" alt="image" src="https://github.com/user-attachments/assets/7b54c820-c611-4365-8a1b-960d6fe3353c" />


```
df.head()
```
<img width="882" height="282" alt="image" src="https://github.com/user-attachments/assets/af1baf05-3352-4d93-ad71-55bfbbc1fa6f" />


```
df.tail()
```
<img width="1012" height="291" alt="image" src="https://github.com/user-attachments/assets/d75e6772-1e13-47e5-b893-86c70d072e5a" />

```
df.isnull().sum()
```
<img width="689" height="289" alt="image" src="https://github.com/user-attachments/assets/f65ef63c-389e-42ad-8a1a-9d09b0e5acf1" />


```
df['datetime'] = pd.to_datetime(df['period.datetimeFrom.utc'], errors='coerce')

df['datetime']
```
<img width="1071" height="631" alt="image" src="https://github.com/user-attachments/assets/4e548107-12b6-46b5-9ced-6d3094387221" />


```
df['pm25'] = pd.to_numeric(df['value'], errors='coerce')
df['pm25']
```
<img width="1058" height="628" alt="image" src="https://github.com/user-attachments/assets/c6c5122d-548e-4173-84d3-69117d1a68e6" />

```
df['date'] = df['datetime'].dt.strftime('%Y-%m-%d')
df['month'] = df['datetime'].dt.month
df['hour'] = df['datetime'].dt.hour
df.head(10)
```
<img width="1554" height="507" alt="image" src="https://github.com/user-attachments/assets/2bffb079-76e8-460e-80a5-99cfbc09e5b8" />

```
import matplotlib.pyplot as plt
import seaborn as sns
plt.figure(figsize=(10, 6))
sns.boxplot(x='month', y='pm25', data=df)
plt.xlabel('Month')
plt.ylabel('PM2.5')
plt.show()
```
<img width="1244" height="691" alt="image" src="https://github.com/user-attachments/assets/18f9ca6f-4601-4470-9e8a-6fdd2803b3be" />


```
monthly_avg = df.groupby('month')['pm25'].mean().reset_index()
plt.figure(figsize=(8, 5))
plt.plot(monthly_avg['month'], monthly_avg['pm25'], marker='o')
plt.xlabel('Month')
plt.ylabel('Average PM2.5')
plt.grid(True)
plt.show()
```
<img width="1017" height="582" alt="image" src="https://github.com/user-attachments/assets/45b45601-cf9d-48cc-90cb-f9b2052ffd78" />


```
daily_avg = df.groupby('date')['pm25'].mean().reset_index()
exceed_days = daily_avg[daily_avg['pm25'] > 25]
total_days = daily_avg['date'].nunique()
exceed_count = exceed_days.shape[0]
exceed_percent = (exceed_count / total_days) * 100

print(f"Days exceeding WHO limit: {exceed_count} days")
print(f"Percentage of days exceeding WHO limit: {exceed_percent:.2f}%")
```

<img width="886" height="60" alt="image" src="https://github.com/user-attachments/assets/6029f89e-e78e-41bc-94fe-08b78701e6e2" />

```
top5_days = daily_avg.sort_values(by='pm25', ascending=False).head(5)
print("Top 5 worst-polluted days (date and PM2.5):")
print(top5_days)
```
<img width="864" height="176" alt="image" src="https://github.com/user-attachments/assets/0488381e-36cd-4e6d-a943-86fb000bd5f9" />





**Interpretation**

1)PM2.5 and NO₂ show a strong positive correlation, suggesting that both pollutants increase together, likely due to vehicle and industrial emissions.

2) Over the analysis period, the data shows that about {exceed_percent:.2f}% of days had PM2.5 levels above the WHO recommended limit of 25 µg/m³.
In simpler terms, that means the air was unhealthy roughly one out of every three days.

This indicates that the area often experiences moderate to poor air quality, especially on days with calm weather or higher human activity.
If this trend continues, it could pose long-term health risks, particularly for children, the elderly, and people with breathing problems.

Seasonal patterns could also play a role — pollution levels often rise during winter months when pollutants get trapped close to the ground.
To improve conditions, stricter emission controls, cleaner transport options, and awareness campaigns could help reduce PM2.5 levels and make the air safer to breathe.

**Result**

The dataset was successfully loaded and processed to extract pollutant-wise and station-wise air quality data for Delhi.


