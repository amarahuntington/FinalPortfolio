```python
# Importing
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns


# Reading in data as per A3
subjects = ['spid10_2014_06_17_17_44', 'spid12_2014_06_20_15_11', 'spid13_2014_07_03_15_06', 'spid14_2014_06_24_14_35', 'spid15_2014_06_22_16_31', 'spid16_2014_06_23_18_38', 'spid18_2014_07_02_19_03', 'spid19_2014_07_02_16_06', 'spid20_2014_07_08_12_34', 'spid21_2014_07_07_15_25', 'spid23_2014_07_11_10_24', 'spid24_2014_07_17_17_14', 'spid25_2014_07_14_17_50', 'spid26_2014_07_17_14_47', 'spid27_2014_07_29_13_57', 'spid28_2014_07_28_14_32', 'spid30_2014_08_07_17_39', 'spid31_2014_08_08_13_34', 'spid32_2014_08_07_14_22', 'spid34_2014_09_12_17_34', 'spid22sf_2014_07_10_15_04']

# Write loop to read in each file in subjects
df_list = []
for file in subjects:
    in_file = file +'/'+file+'_data.txt'
    df = pd.read_csv(in_file, sep='\t')
    df_list.append(df)

# Error dataframe: concat df_list into 1 dataframe (error)
df_error = pd.concat(df_list)

# Error dataframe: Have to drop practice
df_error = df_error.dropna()
pd.unique(df_error['block'])
df_error = df_error[df_error['block'] != 'practice']
df_error['rt'] = df_error['rt']*1000

# Error dataframe: Has to be changed into a loop so that each participant gets ztransform
from scipy.stats import zscore
for x in df_error['rt']: 
    ztransform = zscore(df_error['rt'])
    if [ztransform > 2]:
        x = df_error['rt'].mean()
# Rt dataframe: create a second dataframe that doesn't include error trials
df_rt= pd.concat(df_list)

# Rt dataframe: drop practice and error
df_rt=df_rt.dropna()
pd.unique(df_rt['block'])
df_rt=df_rt[df_rt['block'] != 'practice']
df_rt= df_rt[df_rt['error'] != True]
df_rt['rt']= df_rt['rt']*1000

# Rt dataframe: ztransform participants
for x in df_rt['rt']:
    xtransform=zscore(df_rt['rt'])
    if [ztransform > 2]:
        x= df_rt['rt'].mean()

        
# Adding simon column to rt dataframe
conditions = [
    (df_rt['mapping'] == 0) & (df_rt['target'] == 'black') & (df_rt['targetLocation'] == 'left'),
    (df_rt['mapping'] == 0) & (df_rt['target'] == 'black') & (df_rt['targetLocation'] == 'right'),
    (df_rt['mapping'] == 0) & (df_rt['target'] == 'white') & (df_rt['targetLocation'] == 'left'),
    (df_rt['mapping'] == 0) & (df_rt['target'] == 'white') & (df_rt['targetLocation'] == 'right'),
    (df_rt['mapping'] == 1) & (df_rt['target'] == 'black') & (df_rt['targetLocation'] == 'left'),
    (df_rt['mapping'] == 1) & (df_rt['target'] == 'black') & (df_rt['targetLocation'] == 'right'),
    (df_rt['mapping'] == 1) & (df_rt['target'] == 'white') & (df_rt['targetLocation'] == 'left'),
    (df_rt['mapping'] == 1) & (df_rt['target'] == 'white') & (df_rt['targetLocation'] == 'right')
    ]

mapping = ['incongruent', 
           'congruent', 
           'congruent', 
           'incongruent', 
           'congruent', 
           'incongruent', 
           'incongruent', 
           'congruent'
          ]

df_rt['simon'] = np.select(conditions, mapping, default='none')
# Adding simon column to error dataframe
conditions = [
    (df_error['mapping'] == 0) & (df_error['target'] == 'black') & (df_error['targetLocation'] == 'left'),
    (df_error['mapping'] == 0) & (df_error['target'] == 'black') & (df_error['targetLocation'] == 'right'),
    (df_error['mapping'] == 0) & (df_error['target'] == 'white') & (df_error['targetLocation'] == 'left'),
    (df_error['mapping'] == 0) & (df_error['target'] == 'white') & (df_error['targetLocation'] == 'right'),
    (df_error['mapping'] == 1) & (df_error['target'] == 'black') & (df_error['targetLocation'] == 'left'),
    (df_error['mapping'] == 1) & (df_error['target'] == 'black') & (df_error['targetLocation'] == 'right'),
    (df_error['mapping'] == 1) & (df_error['target'] == 'white') & (df_error['targetLocation'] == 'left'),
    (df_error['mapping'] == 1) & (df_error['target'] == 'white') & (df_error['targetLocation'] == 'right')
    ]

mapping = ['incongruent', 
           'congruent', 
           'congruent', 
           'incongruent', 
           'congruent', 
           'incongruent', 
           'incongruent', 
           'congruent'
          ]

df_error['simon'] = np.select(conditions, mapping, default='none')
```


```python
#Explore the dataframe we will be subsetting
df_rt.head()
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
      <th>simon</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>144</th>
      <td>spid10</td>
      <td>2014.0</td>
      <td>6.0</td>
      <td>17.0</td>
      <td>17.0</td>
      <td>44.0</td>
      <td>0.0</td>
      <td>13.251555</td>
      <td>1</td>
      <td>1.0</td>
      <td>left</td>
      <td>white</td>
      <td>incongruent</td>
      <td>551.714007</td>
      <td>white</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>0.068436</td>
      <td>congruent</td>
    </tr>
    <tr>
      <th>145</th>
      <td>spid10</td>
      <td>2014.0</td>
      <td>6.0</td>
      <td>17.0</td>
      <td>17.0</td>
      <td>44.0</td>
      <td>0.0</td>
      <td>13.251555</td>
      <td>1</td>
      <td>1.0</td>
      <td>left</td>
      <td>white</td>
      <td>incongruent</td>
      <td>551.714007</td>
      <td>white</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>0.068436</td>
      <td>congruent</td>
    </tr>
    <tr>
      <th>146</th>
      <td>spid10</td>
      <td>2014.0</td>
      <td>6.0</td>
      <td>17.0</td>
      <td>17.0</td>
      <td>44.0</td>
      <td>0.0</td>
      <td>13.251555</td>
      <td>1</td>
      <td>1.0</td>
      <td>left</td>
      <td>white</td>
      <td>incongruent</td>
      <td>551.714007</td>
      <td>white</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>0.068436</td>
      <td>congruent</td>
    </tr>
    <tr>
      <th>147</th>
      <td>spid10</td>
      <td>2014.0</td>
      <td>6.0</td>
      <td>17.0</td>
      <td>17.0</td>
      <td>44.0</td>
      <td>0.0</td>
      <td>13.251555</td>
      <td>1</td>
      <td>2.0</td>
      <td>right</td>
      <td>black</td>
      <td>congruent</td>
      <td>344.355373</td>
      <td>black</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>0.068915</td>
      <td>congruent</td>
    </tr>
    <tr>
      <th>148</th>
      <td>spid10</td>
      <td>2014.0</td>
      <td>6.0</td>
      <td>17.0</td>
      <td>17.0</td>
      <td>44.0</td>
      <td>0.0</td>
      <td>13.251555</td>
      <td>1</td>
      <td>2.0</td>
      <td>right</td>
      <td>black</td>
      <td>congruent</td>
      <td>344.355373</td>
      <td>black</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>0.068915</td>
      <td>congruent</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create a new dataframe of mean RT values grouped by flanker and simon conditions:
dfmean=df_rt.groupby(['flankers', 'simon'])[['rt']].mean().round(2)
dfmean.columns=['Mean RT']
dfmean.head()
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
      <th></th>
      <th>Mean RT</th>
    </tr>
    <tr>
      <th>flankers</th>
      <th>simon</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">congruent</th>
      <th>congruent</th>
      <td>431.12</td>
    </tr>
    <tr>
      <th>incongruent</th>
      <td>464.14</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">incongruent</th>
      <th>congruent</th>
      <td>498.73</td>
    </tr>
    <tr>
      <th>incongruent</th>
      <td>507.72</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create a new dataframe of the standard deviation of RT values grouped by flanker and simon conditions:
dfstd= df_rt.groupby(['flankers', 'simon'])[['rt']].std().round(2)
dfstd.columns=['RT STD']
dfstd.head()
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
      <th></th>
      <th>RT STD</th>
    </tr>
    <tr>
      <th>flankers</th>
      <th>simon</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">congruent</th>
      <th>congruent</th>
      <td>104.96</td>
    </tr>
    <tr>
      <th>incongruent</th>
      <td>99.56</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">incongruent</th>
      <th>congruent</th>
      <td>110.04</td>
    </tr>
    <tr>
      <th>incongruent</th>
      <td>98.38</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create a new dataframe of error count grouped by flanker and simon conditions:
df_incorrect= df_error[df_error['error'] == True]
df_incorrect.groupby(['flankers', 'simon'])[[ 'error']].count()
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
      <th></th>
      <th>error</th>
    </tr>
    <tr>
      <th>flankers</th>
      <th>simon</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">congruent</th>
      <th>congruent</th>
      <td>116</td>
    </tr>
    <tr>
      <th>incongruent</th>
      <td>196</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">incongruent</th>
      <th>congruent</th>
      <td>318</td>
    </tr>
    <tr>
      <th>incongruent</th>
      <td>417</td>
    </tr>
  </tbody>
</table>
</div>


