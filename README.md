# Ex.No: 1B                     CONVERSION OF NON STATIONARY TO STATIONARY DATA
# Date:19-08-2025

### AIM:
To perform regular differncing,seasonal adjustment and log transformatio on international airline passenger data
### ALGORITHM:
1. Import the required packages like pandas and numpy
2. Read the data using the pandas
3. Perform the data preprocessing if needed and apply regular differncing,seasonal adjustment,log transformation.
4. Plot the data according to need, before and after regular differncing,seasonal adjustment,log transformation.
5. Display the overall results.
### PROGRAM:
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from statsmodels.tsa.seasonal import seasonal_decompose
data = pd.read_csv("/content/DailyDelhiClimateTest.csv")
data.head()
data.index = pd.to_datetime(data.index)
data.set_index('date',inplace=True)
data['meantemp_diff'] = data['meantemp'] - data['meantemp'].shift(1)
result = seasonal_decompose(data['meantemp'], model='additive', period=7)
data['meantemp_sea_diff'] = result.resid
data['meantemp_log'] = np.log(data['meantemp'] + 1)  # adding 1 to avoid log(0)
data['meantemp_log_diff'] = data['meantemp_log'] - data['meantemp_log'].shift(1)
result = seasonal_decompose(data['meantemp_log_diff'].dropna(), model='additive', period=7)

# Create a DataFrame from the decomposition results to merge with the original data
residual_df = pd.DataFrame(result.resid, index=result.resid.index, columns=['meantemp_log_seasonal_diff'])

# Merge the residual component back to the original DataFrame based on the index
data = data.merge(residual_df, left_index=True, right_index=True, how='left')
plt.figure(figsize=(16, 16))
plt.subplot(6, 1, 1)
plt.plot(data['meantemp'], label='Original')
plt.legend(loc='best')
plt.title('Original Data')
plt.xlabel('Date')
plt.ylabel('Mean Temperature')
plt.subplot(6, 1, 2)
plt.plot(data['meantemp_diff'], label='Regular Difference')
plt.legend(loc='best')
plt.title('Regular Differencing')
plt.xlabel('Date')
plt.ylabel('Differenced Mean Temperature')

plt.subplot(6, 1, 4)
plt.plot(data['meantemp_sea_diff'], label='Seasonal Adjustment')
plt.legend(loc='best')
plt.title('Seasonal Adjustment')
plt.xlabel('Date')
plt.ylabel('Seasonally Adjusted Mean Temperature')
plt.subplot(6, 1, 2)
plt.plot(data['meantemp_log'], label='Log Transformation')
plt.legend(loc='best')
plt.title('Log Transformation')
plt.xlabel('Date')
plt.ylabel('Log(Mean Temperature)')
plt.subplot(6, 1, 5)
plt.plot(data['meantemp_log_diff'], label='Log Transformation and Regular Differencing')
plt.legend(loc='best')
plt.title('Log Transformation and Regular Differencing')
plt.xlabel('Date')
plt.ylabel('RDiff(Log(Mean Temperature))')
plt.subplot(6, 1, 6)
plt.plot(data['meantemp_log_seasonal_diff'], label='Log Transformation and Seasonal Differencing')
plt.legend(loc='best')
plt.title('Log Transformation and Regular + Seasonal Differencing')
plt.xlabel('Date')
plt.ylabel('SDiff(RDiff(Log(Mean Temperature)))')
plt.tight_layout()
plt.show()
data['meantemp'].plot(kind='line', figsize=(12,6))
plt.title('Mean Temperature Over Time')
plt.ylabel('Mean Temperature')
plt.show()
```
### OUTPUT:
### Unprocessed Data:
<img width="812" height="198" alt="image" src="https://github.com/user-attachments/assets/ddcfa94e-f64e-4db2-acb5-bc17fa27eab1" />

### After regular differencing:
<img width="802" height="194" alt="image" src="https://github.com/user-attachments/assets/fdc4623f-d99e-47ad-95cb-1b36359c47dc" />

### After seasonal adjustment:
<img width="835" height="187" alt="image" src="https://github.com/user-attachments/assets/de00e6e4-7f71-4fe4-a237-55592e7efecc" />

### After log transformation:
<img width="810" height="198" alt="image" src="https://github.com/user-attachments/assets/47e12827-3885-47a9-8560-984ca20354ca" />

### After log transformation and regular differencing:
<img width="823" height="184" alt="image" src="https://github.com/user-attachments/assets/41d75f0a-acf6-4485-a005-0e168e4f322e" />

### After log transformation, regular differencing and seasonal differencing:
<img width="805" height="186" alt="image" src="https://github.com/user-attachments/assets/04359955-bd76-464d-b3cb-5b7336409b98" />

### OVERALL VIEW:
<img width="1239" height="664" alt="image" src="https://github.com/user-attachments/assets/77ed99cc-ff73-4571-b22b-1f10705f7161" />

### RESULT:
Thus we have created the python code for the conversion of non stationary to stationary data on international airline passenger
data.
