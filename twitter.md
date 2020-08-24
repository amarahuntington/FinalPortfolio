<a href="https://amarahuntington.github.io/FinalPortfolio/">Home</a>


```python
# Import Modules
from collections import Counter
import matplotlib.pyplot as plt
import pandas as pd
import json

# Open and read JSON files and assign variable names
WW_trends = json.loads(open('WWTrends.json').read())
US_trends = json.loads(open('USTrends.json').read())

# Load trending topic information from WeLoveTheEarth.json Dataset folder
tweets = json.loads(open('WeLoveTheEarth.json').read())
```


```python
# Extracting the text of all the tweets from the tweet object
texts = [tweet['text'] 
                 for tweet in tweets ]

# Extracting screen names of users tweeting about #WeLoveTheEarth
names = [user_mention['screen_name'] 
                 for tweet in tweets
                     for user_mention in tweet['entities']['user_mentions']]
# Extracting all the hashtags being used when talking about this topic
hashtags = [hashtag['text'] 
             for tweet in tweets
                 for hashtag in tweet['entities']['hashtags']]
```


```python
# Counting occcurrences/ getting frequency distribution of all names and hashtags
for item in [names, hashtags]:
    c = Counter(item)    
    # Inspecting the 10 most common items in c
    print (c.most_common(10), "\n")
```

    [('lildickytweets', 102), ('LeoDiCaprio', 44), ('ShawnMendes', 33), ('halsey', 31), ('ArianaGrande', 30), ('justinbieber', 29), ('Spotify', 26), ('edsheeran', 26), ('sanbenito', 25), ('SnoopDogg', 25)] 
    
    [('WeLoveTheEarth', 313), ('4future', 12), ('19aprile', 12), ('EARTH', 11), ('fridaysforfuture', 10), ('EarthMusicVideo', 3), ('ConCalmaRemix', 3), ('Earth', 3), ('aliens', 2), ('AvengersEndgame', 2)] 
    



```python
# Extracting useful information from retweets
retweets = [
             (tweet['retweet_count'], 
              tweet['retweeted_status']['favorite_count'],
              tweet['retweeted_status']['user']['followers_count'],
              tweet['retweeted_status']['user']['screen_name'],
              tweet['text']) 
            
            for tweet in tweets 
                if 'retweeted_status' in tweet
           ]
```


```python
# Create a DataFrame and visualize the data in a pretty and insightful format
df = pd.DataFrame(
    retweets, 
    columns=['Retweets','Favorites','Followers','ScreenName','Text']).groupby(
    ['ScreenName','Text','Followers']).sum().sort_values(by=['Followers'], ascending=False)

df.style.background_gradient()
df.head(10)
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
      <th></th>
      <th>Retweets</th>
      <th>Favorites</th>
    </tr>
    <tr>
      <th>ScreenName</th>
      <th>Text</th>
      <th>Followers</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">katyperry</th>
      <th rowspan="2" valign="top">RT @katyperry: Sure, the Mueller report is out, but @lildickytweets’ "Earth" but will be too tonight. Don't say I never tried to save the w…</th>
      <th>107195569</th>
      <td>2338</td>
      <td>10557</td>
    </tr>
    <tr>
      <th>107195568</th>
      <td>2338</td>
      <td>10556</td>
    </tr>
    <tr>
      <th>TheEllenShow</th>
      <th>RT @TheEllenShow: .@lildickytweets, @justinbieber, @MileyCyrus, @katyperry, @ArianaGrande and more, all in one music video. My head might e…</th>
      <th>77474826</th>
      <td>2432</td>
      <td>10086</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">LeoDiCaprio</th>
      <th>RT @LeoDiCaprio: The @dicapriofdn partners that will benefit from this collaboration include @SharkRayFund, @SolutionsProj, @GreengrantsFun…</th>
      <th>18988898</th>
      <td>28505</td>
      <td>78137</td>
    </tr>
    <tr>
      <th>RT @LeoDiCaprio: Thank you to @lildickytweets and all the artists that came together to make this happen. Net profits from the song, video,…</th>
      <th>18988898</th>
      <td>149992</td>
      <td>416018</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">halsey</th>
      <th rowspan="2" valign="top">RT @halsey: 🐯🐯🐯 meeeeeeeow 👅 #WeLoveTheEarth https://t.co/JJr6vmKG7U</th>
      <th>10564842</th>
      <td>7742</td>
      <td>69684</td>
    </tr>
    <tr>
      <th>10564841</th>
      <td>7742</td>
      <td>69682</td>
    </tr>
    <tr>
      <th>scooterbraun</th>
      <th>RT @scooterbraun: Watch #EARTH NOW! #WeLoveTheEarth — https://t.co/OtRfIgcSZL</th>
      <th>3885179</th>
      <td>5925</td>
      <td>14635</td>
    </tr>
    <tr>
      <th>Spotify</th>
      <th>RT @Spotify: This is epic. @lildickytweets got @justinbieber, @arianagrande, @halsey, @sanbenito, @edsheeran, @SnoopDogg, @ShawnMendes, @Kr…</th>
      <th>2973277</th>
      <td>107222</td>
      <td>237259</td>
    </tr>
    <tr>
      <th>TomHall</th>
      <th>RT @TomHall: 🐆\n\nLook Out!\n\n🐆\n\n#Cheetah @LeoDiCaprio  #FridayFeeling #WeLoveTheEarth \n\nhttps://t.co/w1O1NyTbwX</th>
      <th>590841</th>
      <td>82</td>
      <td>252</td>
    </tr>
  </tbody>
</table>
</div>




```python

```
