<a href="https://amarahuntington.github.io/FinalPortfolio/">Home</a>


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


