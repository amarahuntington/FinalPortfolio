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




![png](hist_files/hist_2_0.png)




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




![png](hist_files/hist_3_0.png)




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




![png](hist_files/hist_4_0.png)




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




![png](hist_files/hist_5_0.png)


