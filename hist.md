```python
subjects = ['spid10_2014_06_17_17_44', 'spid12_2014_06_20_15_11']
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
in_file = (subjects[0]+ "/"+ subjects[0]+ '_data.txt')
df= pd.read_csv(in_file, sep='\t')
df.isna().sum()
df= df.dropna(axis=0, how='any')
df.isna().sum()
df= df.iloc[::3]
df = df[df.block != 'practice']
df['rt']= 1000*df['rt']
```


```python
## All histograms are plotted with a solid line at the median, and dashed lines at 25th and 75th percentiles.
```


```python
# Normal Histogram
df['rt'].plot(kind='hist', color='r')
plt.axvline(df['rt'].describe()['25%'], 0, 1, color='turquoise', linestyle='--')
plt.axvline(df['rt'].median(), 0, 1, color='cyan', linestyle='-')
plt.axvline(df['rt'].describe()['75%'], 0, 1, color='turquoise', linestyle='--')
plt.title('Reaction Time')
plt.xlabel('Reaction Time (ms)')
plt.show()
```




<img width="717" alt="1" src="https://user-images.githubusercontent.com/69179367/90472698-83bd8100-e0f7-11ea-86e5-736339ee2bbd.png">




```python
# Cumulative Distribution Frequency Histogram
df['rt'].plot(kind='hist', cumulative=True, color='yellow')
plt.axvline(df['rt'].describe()['25%'], 0, 1, color='turquoise', linestyle='--')
plt.axvline(df['rt'].median(), 0, 1, color='cyan', linestyle='-')
plt.axvline(df['rt'].describe()['75%'], 0, 1, color='turquoise', linestyle='--')
plt.title('Cumulative Distribution Frequency of Reaction Time')
plt.xlabel('Reaction Time (ms)')
plt.show()
```




<img width="724" alt="2" src="https://user-images.githubusercontent.com/69179367/90472704-84eeae00-e0f7-11ea-89f1-b9dfbfda374f.png">




```python
# Log RT Histogram
df['logrt'] = np.log(df['rt'])
df['logrt'].plot(kind='hist', color='green')
plt.axvline(df['logrt'].describe()['25%'], 0, 1, color='turquoise', linestyle='--')
plt.axvline(df['logrt'].median(), 0, 1, color='cyan', linestyle='-')
plt.axvline(df['logrt'].describe()['75%'], 0, 1, color='turquoise', linestyle='--')
plt.title('Reaction Time (Log Transformed)')
plt.show()
```




<img width="717" alt="3" src="https://user-images.githubusercontent.com/69179367/90472705-85874480-e0f7-11ea-97f8-9499594249fa.png">





```python
# Inverse RT Histogram
df['invrt'] = 1/df['rt']
df['invrt'].plot(kind='hist')
plt.axvline(df['invrt'].describe()['25%'], 0, 1, color='turquoise', linestyle='--')
plt.axvline(df['invrt'].median(), 0, 1, color='cyan', linestyle='-')
plt.axvline(df['invrt'].describe()['75%'], 0, 1, color='turquoise', linestyle='--')
plt.title('Reaction Time (Inversely Transformed)')
plt.show()
```




<img width="717" alt="4" src="https://user-images.githubusercontent.com/69179367/90472706-86b87180-e0f7-11ea-9c84-bf5b6e0c47bb.png">


