# Ex.No: 03   COMPUTE THE AUTO FUNCTION(ACF)
# NAME : Raghu Ram VR
# REG.NO : 212224220075
Date:  02/05/2026

### AIM:
To Compute the AutoCorrelation Function (ACF) of the data for the first 35 lags to determine the model
type to fit the data.

### ALGORITHM:
1. Import the necessary packages
2. Find the mean, variance and then implement normalization for the data.
3. Implement the correlation using necessary logic and obtain the results
4. Store the results in an array
5. Represent the result in graphical representation as given below.

### PROGRAM:
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt


data = pd.read_csv('marketing_campaign_performance_10000.csv')


date_col = None
for col in data.columns:
    if 'date' in col.lower() or 'time' in col.lower():
        date_col = col
        break


if date_col is None:
    data['Date'] = pd.date_range(start='2020-01-01', periods=len(data), freq='D')
    date_col = 'Date'

data[date_col] = pd.to_datetime(data[date_col], errors='coerce')
data = data.dropna(subset=[date_col])
data.sort_values(date_col, inplace=True)

numeric_cols = data.select_dtypes(include=[np.number]).columns
value_col = numeric_cols[0]   # first numeric column

values = data[value_col].dropna().values

N = len(values)
lags = range(min(35, N))

autocorr_values = []

mean_data = np.mean(values)
variance_data = np.var(values)

for lag in lags:
    if lag == 0:
        autocorr_values.append(1)
    else:
        auto_cov = np.sum((values[:-lag] - mean_data) * (values[lag:] - mean_data)) / N
        autocorr_values.append(auto_cov / variance_data)

plt.figure(figsize=(10, 6))
plt.stem(lags, autocorr_values)

plt.title(f'Autocorrelation of {value_col}')
plt.xlabel('Lag')
plt.ylabel('Autocorrelation')

plt.grid(True)
plt.show()
```

### OUTPUT:
<img width="846" height="547" alt="image" src="https://github.com/user-attachments/assets/20a918f5-23cc-43ab-8b34-972acc275871" />



### RESULT:
        Thus we have successfully implemented the auto correlation function in python.
