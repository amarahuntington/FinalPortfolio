```python
# Importing Modules and Data files
import pandas as pd
import numpy as np


subjects = ['spid10_2014_06_17_17_44', 'spid12_2014_06_20_15_11']
in_file = (subjects[0]+ "/"+ subjects[0]+ '_data.txt')
df= pd.read_csv(in_file, sep='\t')
```


```python
#Inspecting data that we will be working with
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>year</th>
      <th>month</th>
      <th>day</th>
      <th>hour</th>
      <th>minute</th>
      <th>mapping</th>
      <th>messageViewingTime</th>
      <th>block</th>
      <th>trialNum</th>
      <th>targetLocation</th>
      <th>target</th>
      <th>flankers</th>
      <th>rt</th>
      <th>response</th>
      <th>error</th>
      <th>anticipation</th>
      <th>feedbackResponse</th>
      <th>targetOnError</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>spid10</td>
      <td>2014</td>
      <td>6</td>
      <td>17</td>
      <td>17</td>
      <td>44</td>
      <td>0</td>
      <td>4.395697</td>
      <td>practice</td>
      <td>1</td>
      <td>right</td>
      <td>white</td>
      <td>congruent</td>
      <td>0.723172</td>
      <td>white</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>0.069474</td>
    </tr>
    <tr>
      <th>1</th>
      <td>spid10</td>
      <td>2014</td>
      <td>6</td>
      <td>17</td>
      <td>17</td>
      <td>44</td>
      <td>0</td>
      <td>4.395697</td>
      <td>practice</td>
      <td>1</td>
      <td>right</td>
      <td>white</td>
      <td>congruent</td>
      <td>0.723172</td>
      <td>white</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>0.069474</td>
    </tr>
    <tr>
      <th>2</th>
      <td>spid10</td>
      <td>2014</td>
      <td>6</td>
      <td>17</td>
      <td>17</td>
      <td>44</td>
      <td>0</td>
      <td>4.395697</td>
      <td>practice</td>
      <td>1</td>
      <td>right</td>
      <td>white</td>
      <td>congruent</td>
      <td>0.723172</td>
      <td>white</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>0.069474</td>
    </tr>
    <tr>
      <th>3</th>
      <td>spid10</td>
      <td>2014</td>
      <td>6</td>
      <td>17</td>
      <td>17</td>
      <td>44</td>
      <td>0</td>
      <td>4.395697</td>
      <td>practice</td>
      <td>2</td>
      <td>right</td>
      <td>white</td>
      <td>incongruent</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>False</td>
      <td>True</td>
      <td>0.066798</td>
    </tr>
    <tr>
      <th>4</th>
      <td>spid10</td>
      <td>2014</td>
      <td>6</td>
      <td>17</td>
      <td>17</td>
      <td>44</td>
      <td>0</td>
      <td>4.395697</td>
      <td>practice</td>
      <td>2</td>
      <td>right</td>
      <td>white</td>
      <td>incongruent</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>False</td>
      <td>True</td>
      <td>0.066798</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Exploring how many values are missing within our data
df.isna().sum()
```




    id                    0
    year                  0
    month                 0
    day                   0
    hour                  0
    minute                0
    mapping               0
    messageViewingTime    0
    block                 0
    trialNum              0
    targetLocation        0
    target                0
    flankers              0
    rt                    3
    response              3
    error                 3
    anticipation          0
    feedbackResponse      0
    targetOnError         0
    dtype: int64




```python
# Drop rows of dataframe that contain NaN values, reinspect to ensure this worked
df= df.dropna(axis=0, how='any')
df.isna().sum()
```




    id                    0
    year                  0
    month                 0
    day                   0
    hour                  0
    minute                0
    mapping               0
    messageViewingTime    0
    block                 0
    trialNum              0
    targetLocation        0
    target                0
    flankers              0
    rt                    0
    response              0
    error                 0
    anticipation          0
    feedbackResponse      0
    targetOnError         0
    dtype: int64




```python
# Drop every 3rd row as this data unnecessarily repeats. Reinspect
df= df.iloc[::3]
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>year</th>
      <th>month</th>
      <th>day</th>
      <th>hour</th>
      <th>minute</th>
      <th>mapping</th>
      <th>messageViewingTime</th>
      <th>block</th>
      <th>trialNum</th>
      <th>targetLocation</th>
      <th>target</th>
      <th>flankers</th>
      <th>rt</th>
      <th>response</th>
      <th>error</th>
      <th>anticipation</th>
      <th>feedbackResponse</th>
      <th>targetOnError</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>spid10</td>
      <td>2014</td>
      <td>6</td>
      <td>17</td>
      <td>17</td>
      <td>44</td>
      <td>0</td>
      <td>4.395697</td>
      <td>practice</td>
      <td>1</td>
      <td>right</td>
      <td>white</td>
      <td>congruent</td>
      <td>0.723172</td>
      <td>white</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>0.069474</td>
    </tr>
    <tr>
      <th>6</th>
      <td>spid10</td>
      <td>2014</td>
      <td>6</td>
      <td>17</td>
      <td>17</td>
      <td>44</td>
      <td>0</td>
      <td>4.395697</td>
      <td>practice</td>
      <td>3</td>
      <td>left</td>
      <td>black</td>
      <td>congruent</td>
      <td>0.342453</td>
      <td>black</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>0.071909</td>
    </tr>
    <tr>
      <th>9</th>
      <td>spid10</td>
      <td>2014</td>
      <td>6</td>
      <td>17</td>
      <td>17</td>
      <td>44</td>
      <td>0</td>
      <td>4.395697</td>
      <td>practice</td>
      <td>4</td>
      <td>right</td>
      <td>white</td>
      <td>incongruent</td>
      <td>0.311569</td>
      <td>white</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>0.069050</td>
    </tr>
    <tr>
      <th>12</th>
      <td>spid10</td>
      <td>2014</td>
      <td>6</td>
      <td>17</td>
      <td>17</td>
      <td>44</td>
      <td>0</td>
      <td>4.395697</td>
      <td>practice</td>
      <td>5</td>
      <td>left</td>
      <td>white</td>
      <td>congruent</td>
      <td>0.328021</td>
      <td>black</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>0.067132</td>
    </tr>
    <tr>
      <th>15</th>
      <td>spid10</td>
      <td>2014</td>
      <td>6</td>
      <td>17</td>
      <td>17</td>
      <td>44</td>
      <td>0</td>
      <td>4.395697</td>
      <td>practice</td>
      <td>6</td>
      <td>right</td>
      <td>black</td>
      <td>congruent</td>
      <td>0.353063</td>
      <td>black</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>0.068103</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Inspect possible values within the block column of df
df['block'].unique()
```




    array(['practice', '1', '2', '3', '4', '5'], dtype=object)




```python
# Remove practice trials from df
df = df[df.block != 'practice']
```


```python
# Confirm there are no remaining practice trials in df
df['block'].unique()
```




    array(['1', '2', '3', '4', '5'], dtype=object)


