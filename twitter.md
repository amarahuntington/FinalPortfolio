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
```




<style  type="text/css" >
#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row0_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row1_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row2_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row17_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row32_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row47_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row70_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row70_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row195_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row195_col1{
            background-color:  #fdf5fa;
            color:  #000000;
        }#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row0_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row1_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row2_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row32_col1{
            background-color:  #fbf4f9;
            color:  #000000;
        }#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row3_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row3_col1{
            background-color:  #dedcec;
            color:  #000000;
        }#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row4_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row4_col1{
            background-color:  #023858;
            color:  #f1f1f1;
        }#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row5_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row6_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row162_col0{
            background-color:  #f7f0f7;
            color:  #000000;
        }#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row5_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row6_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row10_col0{
            background-color:  #e3e0ee;
            color:  #000000;
        }#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row7_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row162_col1{
            background-color:  #f9f2f8;
            color:  #000000;
        }#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row7_col1{
            background-color:  #faf2f8;
            color:  #000000;
        }#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row8_col0{
            background-color:  #1278b4;
            color:  #f1f1f1;
        }#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row8_col1{
            background-color:  #529bc7;
            color:  #000000;
        }#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row9_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row9_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row13_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row13_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row14_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row14_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row15_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row15_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row16_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row18_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row19_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row20_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row20_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row21_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row21_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row22_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row22_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row23_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row23_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row24_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row24_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row25_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row25_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row28_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row28_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row29_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row29_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row30_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row30_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row31_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row31_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row33_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row33_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row34_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row34_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row35_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row36_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row36_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row37_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row37_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row38_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row38_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row39_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row39_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row40_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row40_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row41_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row42_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row42_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row43_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row43_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row45_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row45_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row46_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row46_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row48_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row48_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row49_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row49_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row50_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row50_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row51_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row51_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row52_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row52_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row53_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row53_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row54_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row54_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row56_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row56_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row57_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row57_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row58_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row58_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row59_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row59_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row60_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row60_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row61_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row61_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row62_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row62_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row64_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row65_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row65_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row66_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row66_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row67_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row67_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row68_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row68_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row69_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row69_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row71_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row71_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row72_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row72_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row73_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row73_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row74_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row74_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row75_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row75_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row76_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row76_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row77_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row77_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row78_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row78_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row79_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row79_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row80_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row81_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row81_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row83_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row83_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row84_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row84_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row85_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row85_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row86_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row86_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row87_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row87_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row88_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row88_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row89_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row89_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row90_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row90_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row91_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row91_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row92_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row92_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row93_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row93_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row95_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row95_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row96_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row96_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row97_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row97_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row98_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row98_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row99_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row99_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row100_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row100_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row101_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row101_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row102_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row102_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row103_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row103_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row104_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row104_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row106_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row106_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row107_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row107_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row108_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row108_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row109_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row109_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row110_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row110_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row111_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row111_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row112_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row112_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row113_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row113_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row114_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row114_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row115_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row115_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row116_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row116_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row117_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row117_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row118_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row118_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row120_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row120_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row121_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row121_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row122_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row122_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row123_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row123_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row124_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row124_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row125_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row125_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row126_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row126_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row127_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row127_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row128_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row128_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row129_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row129_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row130_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row130_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row131_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row131_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row132_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row132_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row133_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row133_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row134_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row134_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row135_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row135_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row136_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row136_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row137_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row137_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row138_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row138_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row139_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row139_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row140_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row140_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row141_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row141_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row142_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row142_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row143_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row144_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row144_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row145_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row145_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row146_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row146_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row147_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row147_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row148_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row148_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row149_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row149_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row150_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row150_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row151_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row151_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row152_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row152_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row153_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row153_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row154_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row154_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row155_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row155_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row156_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row156_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row157_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row157_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row158_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row158_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row159_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row159_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row160_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row160_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row161_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row163_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row163_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row164_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row164_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row165_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row165_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row166_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row166_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row167_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row167_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row168_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row168_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row169_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row169_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row170_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row170_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row171_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row171_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row172_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row172_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row173_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row173_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row174_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row174_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row175_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row175_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row176_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row176_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row177_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row177_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row178_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row178_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row179_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row179_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row180_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row180_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row181_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row181_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row182_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row182_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row183_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row183_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row184_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row184_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row186_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row186_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row187_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row187_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row188_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row188_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row189_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row189_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row190_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row190_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row192_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row192_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row193_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row193_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row194_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row194_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row196_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row196_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row197_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row197_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row198_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row198_col1{
            background-color:  #fff7fb;
            color:  #000000;
        }#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row10_col1{
            background-color:  #d8d7e9;
            color:  #000000;
        }#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row11_col0{
            background-color:  #0570b0;
            color:  #f1f1f1;
        }#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row11_col1{
            background-color:  #7dacd1;
            color:  #000000;
        }#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row12_col0{
            background-color:  #0567a2;
            color:  #f1f1f1;
        }#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row12_col1{
            background-color:  #6da6cd;
            color:  #000000;
        }#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row16_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row18_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row19_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row26_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row27_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row27_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row35_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row41_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row55_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row63_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row63_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row64_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row80_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row94_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row119_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row119_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row143_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row161_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row185_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row185_col1{
            background-color:  #fef6fb;
            color:  #000000;
        }#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row17_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row44_col1{
            background-color:  #fcf4fa;
            color:  #000000;
        }#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row26_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row47_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row55_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row82_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row82_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row94_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row105_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row105_col1,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row191_col0,#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row191_col1{
            background-color:  #fef6fa;
            color:  #000000;
        }#T_567d0e8a_e102_11ea_a0a7_e66463a2af66row44_col0{
            background-color:  #fbf3f9;
            color:  #000000;
        }</style><table id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66" ><thead>    <tr>        <th class="blank" ></th>        <th class="blank" ></th>        <th class="blank level0" ></th>        <th class="col_heading level0 col0" >Retweets</th>        <th class="col_heading level0 col1" >Favorites</th>    </tr>    <tr>        <th class="index_name level0" >ScreenName</th>        <th class="index_name level1" >Text</th>        <th class="index_name level2" >Followers</th>        <th class="blank" ></th>        <th class="blank" ></th>    </tr></thead><tbody>
                <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row0" class="row_heading level0 row0" rowspan=2>katyperry</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row0" class="row_heading level1 row0" rowspan=2>RT @katyperry: Sure, the Mueller report is out, but @lildickytweets’ "Earth" but will be too tonight. Don't say I never tried to save the w…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row0" class="row_heading level2 row0" >107195569</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row0_col0" class="data row0 col0" >2338</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row0_col1" class="data row0 col1" >10557</td>
            </tr>
            <tr>
                                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row1" class="row_heading level2 row1" >107195568</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row1_col0" class="data row1 col0" >2338</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row1_col1" class="data row1 col1" >10556</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row2" class="row_heading level0 row2" >TheEllenShow</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row2" class="row_heading level1 row2" >RT @TheEllenShow: .@lildickytweets, @justinbieber, @MileyCyrus, @katyperry, @ArianaGrande and more, all in one music video. My head might e…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row2" class="row_heading level2 row2" >77474826</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row2_col0" class="data row2 col0" >2432</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row2_col1" class="data row2 col1" >10086</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row3" class="row_heading level0 row3" rowspan=2>LeoDiCaprio</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row3" class="row_heading level1 row3" >RT @LeoDiCaprio: The @dicapriofdn partners that will benefit from this collaboration include @SharkRayFund, @SolutionsProj, @GreengrantsFun…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row3" class="row_heading level2 row3" >18988898</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row3_col0" class="data row3 col0" >28505</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row3_col1" class="data row3 col1" >78137</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row4" class="row_heading level1 row4" >RT @LeoDiCaprio: Thank you to @lildickytweets and all the artists that came together to make this happen. Net profits from the song, video,…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row4" class="row_heading level2 row4" >18988898</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row4_col0" class="data row4 col0" >149992</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row4_col1" class="data row4 col1" >416018</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row5" class="row_heading level0 row5" rowspan=2>halsey</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row5" class="row_heading level1 row5" rowspan=2>RT @halsey: 🐯🐯🐯 meeeeeeeow 👅 #WeLoveTheEarth https://t.co/JJr6vmKG7U</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row5" class="row_heading level2 row5" >10564842</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row5_col0" class="data row5 col0" >7742</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row5_col1" class="data row5 col1" >69684</td>
            </tr>
            <tr>
                                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row6" class="row_heading level2 row6" >10564841</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row6_col0" class="data row6 col0" >7742</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row6_col1" class="data row6 col1" >69682</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row7" class="row_heading level0 row7" >scooterbraun</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row7" class="row_heading level1 row7" >RT @scooterbraun: Watch #EARTH NOW! #WeLoveTheEarth — https://t.co/OtRfIgcSZL</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row7" class="row_heading level2 row7" >3885179</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row7_col0" class="data row7 col0" >5925</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row7_col1" class="data row7 col1" >14635</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row8" class="row_heading level0 row8" >Spotify</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row8" class="row_heading level1 row8" >RT @Spotify: This is epic. @lildickytweets got @justinbieber, @arianagrande, @halsey, @sanbenito, @edsheeran, @SnoopDogg, @ShawnMendes, @Kr…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row8" class="row_heading level2 row8" >2973277</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row8_col0" class="data row8 col0" >107222</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row8_col1" class="data row8 col1" >237259</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row9" class="row_heading level0 row9" >TomHall</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row9" class="row_heading level1 row9" >RT @TomHall: 🐆

Look Out!

🐆

#Cheetah @LeoDiCaprio  #FridayFeeling #WeLoveTheEarth 

https://t.co/w1O1NyTbwX</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row9" class="row_heading level2 row9" >590841</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row9_col0" class="data row9 col0" >82</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row9_col1" class="data row9 col1" >252</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row10" class="row_heading level0 row10" rowspan=3>lildickytweets</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row10" class="row_heading level1 row10" >RT @lildickytweets: Earth. 4/18 at 9PM PST. #WeLoveTheEarth pre-save link in my bio https://t.co/7HD2xlSFYQ</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row10" class="row_heading level2 row10" >503112</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row10_col0" class="data row10 col0" >25098</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row10_col1" class="data row10 col1" >90974</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row11" class="row_heading level1 row11" rowspan=2>RT @lildickytweets: 🌎 out now #WeLoveTheEarth https://t.co/L22XsoT5P1</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row11" class="row_heading level2 row11" >503112</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row11_col0" class="data row11 col0" >112230</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row11_col1" class="data row11 col1" >199773</td>
            </tr>
            <tr>
                                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row12" class="row_heading level2 row12" >503111</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row12_col0" class="data row12 col0" >119712</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row12_col1" class="data row12 col1" >213072</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row13" class="row_heading level0 row13" >greenpeacefr</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row13" class="row_heading level1 row13" >RT @greenpeacefr: Happening NOW in France : today, to show how much #WeLoveTheEarth, more than 2000 citizens are blocking the headquarters…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row13" class="row_heading level2 row13" >420789</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row13_col0" class="data row13 col0" >128</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row13_col1" class="data row13 col1" >192</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row14" class="row_heading level0 row14" >rlthingy</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row14" class="row_heading level1 row14" >RT @rlthingy: /rlt/ YO GUYS LISTEN TO THIS SONGGGGG #WeLoveTheEarth https://t.co/trUgRG7QBH</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row14" class="row_heading level2 row14" >225977</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row14_col0" class="data row14 col0" >11</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row14_col1" class="data row14 col1" >23</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row15" class="row_heading level0 row15" >SB_Projects</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row15" class="row_heading level1 row15" >RT @SB_Projects: #WeLoveTheEarth out now with @lildickytweets, @justinbieber, @ArianaGrande, @zacbrownband, and 25+ of your other faves htt…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row15" class="row_heading level2 row15" >98418</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row15_col0" class="data row15 col0" >271</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row15_col1" class="data row15 col1" >730</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row16" class="row_heading level0 row16" >biebernovidade</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row16" class="row_heading level1 row16" >RT @biebernovidade: Rio de Janeiro: PRESENTE! #WeLoveTheEarth https://t.co/IJrypKqzSf</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row16" class="row_heading level2 row16" >83281</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row16_col0" class="data row16 col0" >651</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row16_col1" class="data row16 col1" >1499</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row17" class="row_heading level0 row17" >MendesCrewInfo</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row17" class="row_heading level1 row17" >RT @MendesCrewInfo: .@ShawnMendes represents rhinos in the ‘Earth’ music video and feature. His line, “We’re just some rhinos, horny as hec…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row17" class="row_heading level2 row17" >61015</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row17_col0" class="data row17 col0" >2815</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row17_col1" class="data row17 col1" >9599</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row18" class="row_heading level0 row18" rowspan=4>shawnxniallx</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row18" class="row_heading level1 row18" >RT @shawnxniallx: La gente piensa que el calentamiento global no es real, ya abran los ojos y actuemos de una manera real, dime ¿qué te cue…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row18" class="row_heading level2 row18" >53713</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row18_col0" class="data row18 col0" >812</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row18_col1" class="data row18 col1" >1254</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row19" class="row_heading level1 row19" >RT @shawnxniallx: La humanidad es un asco y nunca me cansaré de decirlos ¿no creen que es momento de cambiar? Salvemos nuestro planeta, sal…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row19" class="row_heading level2 row19" >53713</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row19_col0" class="data row19 col0" >604</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row19_col1" class="data row19 col1" >1034</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row20" class="row_heading level1 row20" >RT @shawnxniallx: Vengo a recomendarles a todos esta miniserie que realizó Netflix, en la cual nos muestra cada parte de las especies, lo q…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row20" class="row_heading level2 row20" >53713</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row20_col0" class="data row20 col0" >138</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row20_col1" class="data row20 col1" >299</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row21" class="row_heading level1 row21" >RT @shawnxniallx: Espero que con este video se pueda crear un poco más de conciencia propia, simeplemnte estamos acabando con nuestro plane…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row21" class="row_heading level2 row21" >53713</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row21_col0" class="data row21 col0" >300</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row21_col1" class="data row21 col1" >494</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row22" class="row_heading level0 row22" rowspan=5>SMendesQandA</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row22" class="row_heading level1 row22" >RT @SMendesQandA: #WeLoveTheEarth https://t.co/KDIKYVHFWU</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row22" class="row_heading level2 row22" >53282</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row22_col0" class="data row22 col0" >56</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row22_col1" class="data row22 col1" >339</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row23" class="row_heading level1 row23" >RT @SMendesQandA: Let’s help save the Earth. We’re the ones with the power of change. 🌍  #WeLoveTheEarth</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row23" class="row_heading level2 row23" >53282</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row23_col0" class="data row23 col0" >122</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row23_col1" class="data row23 col1" >343</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row24" class="row_heading level1 row24" >RT @SMendesQandA: So you can basically say you have a song with many of the artists we have asked you to collaborate with...smart move mend…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row24" class="row_heading level2 row24" >53282</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row24_col0" class="data row24 col0" >116</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row24_col1" class="data row24 col1" >717</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row25" class="row_heading level1 row25" >RT @SMendesQandA: “We’re just some rhinos, horny as heck” — Shawn’s part on #WeLoveTheEarth</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row25" class="row_heading level2 row25" >53282</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row25_col0" class="data row25 col0" >244</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row25_col1" class="data row25 col1" >1420</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row26" class="row_heading level1 row26" >RT @SMendesQandA: “We’re just some rhinos horny as heck” — Shawn Mendes #WeLoveTheEarth https://t.co/URnHb0DTWN</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row26" class="row_heading level2 row26" >53282</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row26_col0" class="data row26 col0" >768</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row26_col1" class="data row26 col1" >3309</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row27" class="row_heading level0 row27" >ShawnUpdatesSA</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row27" class="row_heading level1 row27" >RT @ShawnUpdatesSA: Shawn Mendes: rinocerontes.
#WeLoveTheEarth

'La población de rinocerontes negros está casi extinta, y las otras cuatro…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row27" class="row_heading level2 row27" >52424</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row27_col0" class="data row27 col0" >850</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row27_col1" class="data row27 col1" >1807</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row28" class="row_heading level0 row28" >gtbriel</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row28" class="row_heading level1 row28" >RT @gtbriel: vimos o cu do justin bieber

logo em seguida a ariana sair do cu do justin 

e logo em seguuda a ariana sendo morta pela halse…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row28" class="row_heading level2 row28" >49215</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row28_col0" class="data row28 col0" >581</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row28_col1" class="data row28 col1" >970</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row29" class="row_heading level0 row29" >MileySmilerNews</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row29" class="row_heading level1 row29" >RT @MileySmilerNews: Miley’s cameo in the Earth song is so cute #WeLoveTheEarth https://t.co/Byk9rkhMpG</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row29" class="row_heading level2 row29" >47840</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row29_col0" class="data row29 col0" >15</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row29_col1" class="data row29 col1" >116</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row30" class="row_heading level0 row30" >ArianatorFallen</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row30" class="row_heading level1 row30" >RT @ArianatorFallen: Ariana vocals had saved the earth . This song is so important #WeLoveTheEarth https://t.co/Lg9jsPRD5z</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row30" class="row_heading level2 row30" >44426</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row30_col0" class="data row30 col0" >84</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row30_col1" class="data row30 col1" >258</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row31" class="row_heading level0 row31" rowspan=2>MendesNotified</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row31" class="row_heading level1 row31" >RT @MendesNotified: Rhinos - Shawn Mendes🦏🖤
“The black rhino population is nearly extinct, and the four other species of rhinos found in Af…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row31" class="row_heading level2 row31" >39726</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row31_col0" class="data row31 col0" >258</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row31_col1" class="data row31 col1" >858</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row32" class="row_heading level1 row32" >RT @MendesNotified: shawn reading his line for the song #WeLoveTheEarth:  https://t.co/dcWJFDlVMd</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row32" class="row_heading level2 row32" >39726</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row32_col0" class="data row32 col0" >2465</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row32_col1" class="data row32 col1" >10475</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row33" class="row_heading level0 row33" >badweputation</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row33" class="row_heading level1 row33" >RT @badweputation: n adianta vcs comentarem sobre a importância da preservação hj e amanhã já esquecerem e tbm lembrando q o atual presiden…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row33" class="row_heading level2 row33" >38787</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row33_col0" class="data row33 col0" >119</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row33_col1" class="data row33 col1" >183</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row34" class="row_heading level0 row34" rowspan=2>isbreakls</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row34" class="row_heading level1 row34" >RT @isbreakls: Queria dizer que na música do Lil Dicky tem 28 artistas incríveis mais quem eu amo mesmo é um babuíno chamado Justin Bieber.…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row34" class="row_heading level2 row34" >37473</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row34_col0" class="data row34 col0" >95</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row34_col1" class="data row34 col1" >215</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row35" class="row_heading level1 row35" >RT @isbreakls: A música e o clipe tá incrível vamos espalhar o verdadeiro motivo da música que é fazer com que as pessoas pensem nos seus a…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row35" class="row_heading level2 row35" >37473</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row35_col0" class="data row35 col0" >684</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row35_col1" class="data row35 col1" >1068</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row36" class="row_heading level0 row36" >NoticiasSmilers</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row36" class="row_heading level1 row36" >RT @NoticiasSmilers: Soy un elefante, tengo basura en mi trompa. -Pequeño cameo de Miley en la canción Earth de Lil Dicky- #WeLoveTheEarth…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row36" class="row_heading level2 row36" >32649</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row36_col0" class="data row36 col0" >7</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row36_col1" class="data row36 col1" >4</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row37" class="row_heading level0 row37" >ThrowbacksBTS</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row37" class="row_heading level1 row37" >RT @ThrowbacksBTS: Friendly reminder to use less plastic, recycle and contribute in some way to benefit humanity. The world is dying we nee…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row37" class="row_heading level2 row37" >32498</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row37_col0" class="data row37 col0" >370</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row37_col1" class="data row37 col1" >816</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row38" class="row_heading level0 row38" >btsargento</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row38" class="row_heading level1 row38" >RT @btsargento: ▪ estas son varias cosas que podemos hacer para construir un planeta mejor, sin contaminación y un lugar donde todos aporte…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row38" class="row_heading level2 row38" >31453</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row38_col0" class="data row38 col0" >67</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row38_col1" class="data row38 col1" >143</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row39" class="row_heading level0 row39" >biebersmaniabrs</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row39" class="row_heading level1 row39" >RT @biebersmaniabrs: SAIU! Ouçam e assistam “Earth”, música de Lil Dicky com Justin Bieber e mais 29 artistas. 🌏🎶

Video: https://t.co/oHRR…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row39" class="row_heading level2 row39" >26097</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row39_col0" class="data row39 col0" >341</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row39_col1" class="data row39 col1" >463</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row40" class="row_heading level0 row40" >landsrauhl</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row40" class="row_heading level1 row40" >RT @landsrauhl: I loved what lil dicky did. There’s so many popular artists on the track which hopefully means more of a variety of people…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row40" class="row_heading level2 row40" >25071</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row40_col0" class="data row40 col0" >128</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row40_col1" class="data row40 col1" >306</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row41" class="row_heading level0 row41" >biebsrell</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row41" class="row_heading level1 row41" >RT @biebsrell: J: ¿Vamos a morir?
L: ¿Sabes qué, Bieber? Podríamos morir. No voy a mentirte. Quiero decir, hay mucha gente ahí afuera que n…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row41" class="row_heading level2 row41" >23301</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row41_col0" class="data row41 col0" >730</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row41_col1" class="data row41 col1" >1344</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row42" class="row_heading level0 row42" rowspan=3>ShawnNewsPoland</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row42" class="row_heading level1 row42" >RT @ShawnNewsPoland: “We’re just some rhinos, horny as heck” — Shawna tekst w piosence #WeLoveTheEarth https://t.co/NsOmYfoe7u</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row42" class="row_heading level2 row42" >22067</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row42_col0" class="data row42 col0" >97</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row42_col1" class="data row42 col1" >523</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row43" class="row_heading level1 row43" >RT @ShawnNewsPoland: Wyjaśnienie tekstu Shawna #WeLoveTheEarth https://t.co/NiShcC8WGW</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row43" class="row_heading level2 row43" >22067</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row43_col0" class="data row43 col0" >105</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row43_col1" class="data row43 col1" >440</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row44" class="row_heading level1 row44" >RT @ShawnNewsPoland: Słuchajcie wszędzie gdzie się da! #WeLoveTheEarth dochód z piosenki zostanie przekazany fundacji założonej przez DiCap…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row44" class="row_heading level2 row44" >22067</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row44_col0" class="data row44 col0" >4410</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row44_col1" class="data row44 col1" >8720</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row45" class="row_heading level0 row45" >NLiddle16</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row45" class="row_heading level1 row45" >RT @NLiddle16: #Earth isn’t supposed to break records or chart. This song is to bring awareness. We have to change now. We have to love mor…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row45" class="row_heading level2 row45" >21180</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row45_col0" class="data row45 col0" >27</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row45_col1" class="data row45 col1" >134</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row46" class="row_heading level0 row46" >ArianaRenewsFR</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row46" class="row_heading level1 row46" >RT @ArianaRenewsFR: #WeLoveTheEarth est maintenant disponible sur toutes les plateformes ! 

YouTube : https://t.co/zSpCdE74lV

iTunes : ht…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row46" class="row_heading level2 row46" >20794</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row46_col0" class="data row46 col0" >5</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row46_col1" class="data row46 col1" >20</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row47" class="row_heading level0 row47" >iBeliebersMx</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row47" class="row_heading level1 row47" >RT @iBeliebersMx: Justin: ¿Vamos a morir?

Lil Dicky: "Sabes Bieber... podemos morir. No te voy a mentir a ti, hay muchas personas que pien…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row47" class="row_heading level2 row47" >19745</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row47_col0" class="data row47 col0" >2236</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row47_col1" class="data row47 col1" >3872</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row48" class="row_heading level0 row48" >JBCrewdotcom</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row48" class="row_heading level1 row48" >RT @JBCrewdotcom: #WeLoveTheEarth Merchandise available to purchase with proceeds going to The Leonardo DiCaprio Foundation dedicated to th…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row48" class="row_heading level2 row48" >17812</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row48_col0" class="data row48 col0" >89</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row48_col1" class="data row48 col1" >186</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row49" class="row_heading level0 row49" >dimplecabello</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row49" class="row_heading level1 row49" >RT @dimplecabello: “¿Que vamos a defender? El amor. Y nosotros amamos la tierra.” Este video es precioso, últimamente estamos dañando tanto…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row49" class="row_heading level2 row49" >17590</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row49_col0" class="data row49 col0" >286</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row49_col1" class="data row49 col1" >445</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row50" class="row_heading level0 row50" >TeamAGPoland</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row50" class="row_heading level1 row50" >RT @TeamAGPoland: Utwór "Earth" jest już dostępny! Ariana wcieliła się w postać zebry, a cały dochód ze sprzedaży utworu zostanie przekazan…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row50" class="row_heading level2 row50" >17157</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row50_col0" class="data row50 col0" >44</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row50_col1" class="data row50 col1" >142</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row51" class="row_heading level0 row51" >bieberpakiss</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row51" class="row_heading level1 row51" >RT @bieberpakiss: HI IM A BABOON IM LIKE A MAN JUST LESS ADVANCED AND MY ANUS IS HUGE #WeLoveTheEarth https://t.co/tqhfvv7PKf</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row51" class="row_heading level2 row51" >16288</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row51_col0" class="data row51 col0" >148</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row51_col1" class="data row51 col1" >286</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row52" class="row_heading level0 row52" >yourbiebernews</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row52" class="row_heading level1 row52" >RT @yourbiebernews: Justin Bieber: Baboon

#WeLoveTheEarth https://t.co/dlvl1M28Mg</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row52" class="row_heading level2 row52" >14121</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row52_col0" class="data row52 col0" >198</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row52_col1" class="data row52 col1" >404</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row53" class="row_heading level0 row53" >bizzleslovej</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row53" class="row_heading level1 row53" >RT @bizzleslovej: Parliamo di Leo DiCaprio che entra in scena sulla nave del Titanic stile Jack #WeLoveTheEarth https://t.co/VxZscAiOhZ</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row53" class="row_heading level2 row53" >13151</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row53_col0" class="data row53 col0" >5</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row53_col1" class="data row53 col1" >8</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row54" class="row_heading level0 row54" >holdtightlive</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row54" class="row_heading level1 row54" >RT @holdtightlive: THIS SONG IS SO BEAUTIFUL THE VIDEO IS ABSOLUTELY AMAZING AND IT’S FOR A GOOD CAUSE SO LETS GOOO STREAM IT BUY IT WATCH…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row54" class="row_heading level2 row54" >10331</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row54_col0" class="data row54 col0" >35</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row54_col1" class="data row54 col1" >83</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row55" class="row_heading level0 row55" >needyellie</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row55" class="row_heading level1 row55" >RT @needyellie: please keep steaming this song, all proceeds go to an extremely important cause. save planet earth!!
#WeLoveTheEarth https:…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row55" class="row_heading level2 row55" >10319</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row55_col0" class="data row55 col0" >1240</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row55_col1" class="data row55 col1" >2884</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row56" class="row_heading level0 row56" >gladwithmyidols</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row56" class="row_heading level1 row56" >RT @gladwithmyidols: So... Halsey killed Ariana #WeLoveTheEarth https://t.co/Qquu4YfgzE</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row56" class="row_heading level2 row56" >9987</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row56_col0" class="data row56 col0" >23</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row56_col1" class="data row56 col1" >91</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row57" class="row_heading level0 row57" >TJOfficial_</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row57" class="row_heading level1 row57" >RT @TJOfficial_: We love the earth it is our planet 🌍 🎶 #WeLoveTheEarth It’s so beautiful.</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row57" class="row_heading level2 row57" >9693</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row57_col0" class="data row57 col0" >6</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row57_col1" class="data row57 col1" >12</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row58" class="row_heading level0 row58" rowspan=2>MCyrus__forever</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row58" class="row_heading level1 row58" >RT @MCyrus__forever: Cały dochód z tej piosenki trafi do fundacji Leonardo DiCaprio, poświęconej ochronie i dobrobytowi wszystkich mieszkań…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row58" class="row_heading level2 row58" >9680</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row58_col0" class="data row58 col0" >46</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row58_col1" class="data row58 col1" >79</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row59" class="row_heading level1 row59" >RT @MCyrus__forever: Ten teledysk jest świetny, a piosenka ma naprawdę bardzo ważne przesłanie 🌎🌱 #WeLoveTheEarth</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row59" class="row_heading level2 row59" >9680</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row59_col0" class="data row59 col0" >1</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row59_col1" class="data row59 col1" >5</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row60" class="row_heading level0 row60" >justinxrespect</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row60" class="row_heading level1 row60" >RT @justinxrespect: “are we gonna die” ay loco. #WeLoveTheEarth https://t.co/zwof29P4Uw</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row60" class="row_heading level2 row60" >9257</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row60_col0" class="data row60 col0" >9</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row60_col1" class="data row60 col1" >21</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row61" class="row_heading level0 row61" rowspan=2>cyruseyes_</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row61" class="row_heading level1 row61" >RT @cyruseyes_: Qui non stiamo parlando abbastanza di Leonardo DiCaprio e di questa scena 
#WeLoveTheEarth https://t.co/zUfOWBy7ct</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row61" class="row_heading level2 row61" >9148</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row61_col0" class="data row61 col0" >22</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row61_col1" class="data row61 col1" >148</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row62" class="row_heading level1 row62" >RT @cyruseyes_: Questo video deve diventare un film. Il messaggio arriverebbe veramente a tutti. @ Disney sai cosa fare 
#WeLoveTheEarth ht…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row62" class="row_heading level2 row62" >9148</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row62_col0" class="data row62 col0" >10</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row62_col1" class="data row62 col1" >52</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row63" class="row_heading level0 row63" >ChartBTS</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row63" class="row_heading level1 row63" >RT @ChartBTS: "We love the earth it is our planet  
We love the earth it is our home" 

ARMYs, support this important cause, it is our home…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row63" class="row_heading level2 row63" >8527</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row63_col0" class="data row63 col0" >705</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row63_col1" class="data row63 col1" >1731</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row64" class="row_heading level0 row64" >MileyDimension</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row64" class="row_heading level1 row64" >RT @MileyDimension: I think everyone should see this! 
It is about time to do something. #WeLoveTheEarth 
https://t.co/WmMDhsBq19</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row64" class="row_heading level2 row64" >7745</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row64_col0" class="data row64 col0" >645</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row64_col1" class="data row64 col1" >969</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row65" class="row_heading level0 row65" >cabeyoomoon</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row65" class="row_heading level1 row65" >RT @cabeyoomoon: Ta piosenka to bop,  wpada w ucho  i dochody z niej idą na dobry cel,  warto słuchać w kółko i w kółko gdziekolwiek się ty…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row65" class="row_heading level2 row65" >7732</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row65_col0" class="data row65 col0" >9</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row65_col1" class="data row65 col1" >23</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row66" class="row_heading level0 row66" >stonyproud</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row66" class="row_heading level1 row66" >RT @stonyproud: agora que percebi que o clipe todo foi no cu do Justin pqp que cu arrombado  #WeLoveTheEarth https://t.co/I2edu6J9be</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row66" class="row_heading level2 row66" >7728</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row66_col0" class="data row66 col0" >49</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row66_col1" class="data row66 col1" >115</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row67" class="row_heading level0 row67" >hoe4exhoee</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row67" class="row_heading level1 row67" >RT @hoe4exhoee: Lil Dicky - Earth (Official Music Video) https://t.co/XT4xdm9XoN  

KRIS WU's PART IS ON 5:43. It is short but I'm so glad…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row67" class="row_heading level2 row67" >6870</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row67_col0" class="data row67 col0" >10</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row67_col1" class="data row67 col1" >8</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row68" class="row_heading level0 row68" >KCUnidosBR</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row68" class="row_heading level1 row68" >RT @KCUnidosBR: Quem diria em KatyCats? 2 músicas no mesmo dia #ConCalmaRemix #WeLoveTheEarth https://t.co/FF7Be15lhz</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row68" class="row_heading level2 row68" >6458</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row68_col0" class="data row68 col0" >59</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row68_col1" class="data row68 col1" >96</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row69" class="row_heading level0 row69" >Ari_grandeJP</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row69" class="row_heading level1 row69" >RT @Ari_grandeJP: . @lildickytweets の
新曲 #EARTH が公開されました💙🌏

アリアナはシマウマとして参加してます！👀💗✨

 #WeLoveTheEarth https://t.co/5g1clvAZoO</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row69" class="row_heading level2 row69" >6141</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row69_col0" class="data row69 col0" >44</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row69_col1" class="data row69 col1" >306</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row70" class="row_heading level0 row70" >becauseihadyou</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row70" class="row_heading level1 row70" >RT @becauseihadyou: Don’t know why people are being nasty about ‘Earth’. It’s not a song meant to chart or break records, it’s to raise awa…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row70" class="row_heading level2 row70" >5647</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row70_col0" class="data row70 col0" >2484</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row70_col1" class="data row70 col1" >6444</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row71" class="row_heading level0 row71" rowspan=2>biebersmybro</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row71" class="row_heading level1 row71" >RT @biebersmybro: beliebers are promoting the song the hardest lol wbk we are the best fandom ever #WeLoveTheEarth</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row71" class="row_heading level2 row71" >5314</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row71_col0" class="data row71 col0" >14</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row71_col1" class="data row71 col1" >49</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row72" class="row_heading level1 row72" >RT @biebersmybro: WE FINALLY HEARD JUSTIN AND ARIANA IN ONE SONG #WeLoveTheEarth https://t.co/RqHTRButmx</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row72" class="row_heading level2 row72" >5314</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row72_col0" class="data row72 col0" >350</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row72_col1" class="data row72 col1" >996</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row73" class="row_heading level0 row73" >imagiwation</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row73" class="row_heading level1 row73" >RT @imagiwation: entre tanta coisa ruim e tóxica que tá tendo ultimamente a gente tem a chance de se unir por uma causa boa, então não vamo…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row73" class="row_heading level2 row73" >5208</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row73_col0" class="data row73 col0" >88</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row73_col1" class="data row73 col1" >129</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row74" class="row_heading level0 row74" rowspan=2>triedsadlonel</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row74" class="row_heading level1 row74" >RT @triedsadlonel: piosenka mega mi sie podoba,taka fajna wesoła(pod katem melodii) natomiast calosc mnie hitnela mocno,bo to serio jest pr…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row74" class="row_heading level2 row74" >4941</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row74_col0" class="data row74 col0" >10</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row74_col1" class="data row74 col1" >34</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row75" class="row_heading level1 row75" >RT @triedsadlonel: kiedy kazdy byl przekonany ze Shawn bedzie żyrafa a tu jeb nosorożcem #WeLoveTheEarth</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row75" class="row_heading level2 row75" >4941</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row75_col0" class="data row75 col0" >1</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row75_col1" class="data row75 col1" >10</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row76" class="row_heading level0 row76" >neznayuchtotut</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row76" class="row_heading level1 row76" >RT @neznayuchtotut: вау, так много артистов объединились, чтобы поднять насущную проблему нашей планеты о том, что ее нужно защищать и люби…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row76" class="row_heading level2 row76" >4621</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row76_col0" class="data row76 col0" >8</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row76_col1" class="data row76 col1" >150</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row77" class="row_heading level0 row77" >demisfakesmile</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row77" class="row_heading level1 row77" >RT @demisfakesmile: stream. stream. stream this song. its for a good cause, let’s save the earth 🌍 💙💚 #WeLoveTheEarth https://t.co/vgABUfce…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row77" class="row_heading level2 row77" >4421</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row77_col0" class="data row77 col0" >504</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row77_col1" class="data row77 col1" >984</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row78" class="row_heading level0 row78" >SoundOfSeries</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row78" class="row_heading level1 row78" >RT @SoundOfSeries: #INFOSOS : 
'WeLoveTheEarth' est disponible sur YouTube. Le but de cette chanson n’est pas de devenir le tube de l’été,…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row78" class="row_heading level2 row78" >4393</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row78_col0" class="data row78 col0" >80</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row78_col1" class="data row78 col1" >72</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row79" class="row_heading level0 row79" >carawcciolo</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row79" class="row_heading level1 row79" >RT @carawcciolo: Halsey zjada Ariane na śniadanie (koloryzowane) 😅 #WeLoveTheEarth https://t.co/FV4MZSOXlu</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row79" class="row_heading level2 row79" >4246</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row79_col0" class="data row79 col0" >63</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row79_col1" class="data row79 col1" >194</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row80" class="row_heading level0 row80" rowspan=2>gcldendays</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row80" class="row_heading level1 row80" >RT @gcldendays: Be careful who you call ugly in middle school 😉
#WeLoveTheEarth https://t.co/4jvr8Xn397</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row80" class="row_heading level2 row80" >4122</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row80_col0" class="data row80 col0" >394</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row80_col1" class="data row80 col1" >1698</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row81" class="row_heading level1 row81" >RT @gcldendays: No one: 
Not even a soul:

Me:
#WeLoveTheEarth https://t.co/i4BxsXBwny</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row81" class="row_heading level2 row81" >4122</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row81_col0" class="data row81 col0" >408</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row81_col1" class="data row81 col1" >1189</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row82" class="row_heading level0 row82" rowspan=2>biebercentineo</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row82" class="row_heading level1 row82" >RT @biebercentineo: Justin : are we gonna die? 
Lil dicky: you know bieber we might die 

BTCH IM CRYING #EARTH #WeLoveTheEarth #WELOVEEART…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row82" class="row_heading level2 row82" >4038</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row82_col0" class="data row82 col0" >1551</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row82_col1" class="data row82 col1" >3555</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row83" class="row_heading level1 row83" >RT @biebercentineo: THIS SONG IS SO GOOD 😭 #EARTH  #WeLoveTheEarth https://t.co/3hM7ZU2eYh</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row83" class="row_heading level2 row83" >4038</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row83_col0" class="data row83 col0" >79</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row83_col1" class="data row83 col1" >215</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row84" class="row_heading level0 row84" >Bizzbiebsz</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row84" class="row_heading level1 row84" >RT @Bizzbiebsz: So after so many years we finally got Justin x Arina collab and they even gave us zebra and baboon

Stream #WeloveTheEarth…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row84" class="row_heading level2 row84" >3889</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row84_col0" class="data row84 col0" >297</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row84_col1" class="data row84 col1" >879</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row85" class="row_heading level0 row85" >jvkejams</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row85" class="row_heading level1 row85" >RT @jvkejams: I feel like the song will showcase the animals in a very comedic way (bc its how most lil dicky’s songs are) to raise awarene…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row85" class="row_heading level2 row85" >3861</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row85_col0" class="data row85 col0" >20</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row85_col1" class="data row85 col1" >104</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row86" class="row_heading level0 row86" >larryxcolours</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row86" class="row_heading level1 row86" >RT @larryxcolours: ¿Cómo pueden ayudar a la organización? 
-Compren la mercancia, las ganancias irán directamente para ayudar a la tierra.…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row86" class="row_heading level2 row86" >3766</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row86_col0" class="data row86 col0" >330</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row86_col1" class="data row86 col1" >612</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row87" class="row_heading level0 row87" >sitevolts</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row87" class="row_heading level1 row87" >RT @sitevolts: CROSSOVER MAIOR QUE VINGADORES 😵 Justin Bieber, Ariana Grande, Katy Perry, Halsey, Sia, Ed Sheeran, Meghan Trainor e Miley C…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row87" class="row_heading level2 row87" >3707</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row87_col0" class="data row87 col0" >130</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row87_col1" class="data row87 col1" >366</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row88" class="row_heading level0 row88" >JDBieberPol</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row88" class="row_heading level1 row88" >RT @JDBieberPol: Gwiazdy jako zwierzęta w teledysku #Earth #WeLoveTheEarth https://t.co/3aA4mE56Y3</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row88" class="row_heading level2 row88" >3475</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row88_col0" class="data row88 col0" >34</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row88_col1" class="data row88 col1" >99</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row89" class="row_heading level0 row89" rowspan=2>nervous_taste</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row89" class="row_heading level1 row89" >RT @nervous_taste: Have u thought that we will have:

SHAWN MENDES FT ARIANA GRANDE 
SHAWN MENDES FT JUSTIN BIEBER 
SHAWN MENDES FT HALSEY…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row89" class="row_heading level2 row89" >3283</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row89_col0" class="data row89 col0" >2</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row89_col1" class="data row89 col1" >9</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row90" class="row_heading level1 row90" >RT @nervous_taste: Shawn isn’t the giraffe 

#WeLoveTheEarth https://t.co/0PJdZnkP87</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row90" class="row_heading level2 row90" >3283</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row90_col0" class="data row90 col0" >2</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row90_col1" class="data row90 col1" >0</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row91" class="row_heading level0 row91" >nopromisesv</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row91" class="row_heading level1 row91" >RT @nopromisesv: A ogólnie sam teledysk jest świetny, nie spodziewałam się tego #WeLoveTheEarth</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row91" class="row_heading level2 row91" >3283</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row91_col0" class="data row91 col0" >7</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row91_col1" class="data row91 col1" >77</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row92" class="row_heading level0 row92" >INLOVEWITHDS</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row92" class="row_heading level1 row92" >RT @INLOVEWITHDS: Questo video è semplicemente spettacolare e il messaggio che vuole trasmettere è bellissimo
wow non ho parole
Tutti gli a…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row92" class="row_heading level2 row92" >3153</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row92_col0" class="data row92 col0" >14</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row92_col1" class="data row92 col1" >49</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row93" class="row_heading level0 row93" >agbkissy</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row93" class="row_heading level1 row93" >RT @agbkissy: all of yall saying earth is a bad song are immature... its not supposed to be the next big hit, it was made to spread awarene…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row93" class="row_heading level2 row93" >3098</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row93_col0" class="data row93 col0" >584</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row93_col1" class="data row93 col1" >1356</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row94" class="row_heading level0 row94" rowspan=2>dreamsiinflate</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row94" class="row_heading level1 row94" >RT @dreamsiinflate: #WeLoveTheEarth “i am a fat fucking pig” okay brendon urie https://t.co/FdJmq31xZc</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row94" class="row_heading level2 row94" >3045</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row94_col0" class="data row94 col0" >1107</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row94_col1" class="data row94 col1" >3426</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row95" class="row_heading level1 row95" >RT @dreamsiinflate: #WeLoveTheEarth they are all these informative cards on the website which are pretty cool https://t.co/qq0GWJ0q0G</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row95" class="row_heading level2 row95" >3045</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row95_col0" class="data row95 col0" >208</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row95_col1" class="data row95 col1" >840</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row96" class="row_heading level0 row96" >SMendesMedia</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row96" class="row_heading level1 row96" >RT @SMendesMedia: . @ShawnMendes’ part on l “Earth” 🌎 

#WeLoveTheEarth (@lildickytweets)
• https://t.co/XrpJbZ6zY7 https://t.co/o2dO7V5cz4</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row96" class="row_heading level2 row96" >2815</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row96_col0" class="data row96 col0" >238</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row96_col1" class="data row96 col1" >1120</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row97" class="row_heading level0 row97" rowspan=2>kyzztin</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row97" class="row_heading level1 row97" >RT @kyzztin: The whole concept of this is super dope. lil dicky really got so many huge artists to be part of this amazing project and give…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row97" class="row_heading level2 row97" >2641</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row97_col0" class="data row97 col0" >21</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row97_col1" class="data row97 col1" >79</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row98" class="row_heading level1 row98" >RT @kyzztin: same energy #WeloveTheEarth https://t.co/YfTTS6BRiC</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row98" class="row_heading level2 row98" >2641</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row98_col0" class="data row98 col0" >10</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row98_col1" class="data row98 col1" >27</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row99" class="row_heading level0 row99" rowspan=2>xKittyMendes</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row99" class="row_heading level1 row99" >RT @xKittyMendes: SHAWN JEST NOSOROŻCEM
Jego od razu poznałam, bo z innymi miałam problem XD #WeLoveTheEarth https://t.co/21ICMTDdmn</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row99" class="row_heading level2 row99" >2597</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row99_col0" class="data row99 col0" >48</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row99_col1" class="data row99 col1" >426</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row100" class="row_heading level1 row100" >RT @xKittyMendes: Nie sądziłam, że potrzebuję usłyszeć z ust Shawna słowa horny XDD #WeLoveTheEarth https://t.co/TkMA07iNj5</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row100" class="row_heading level2 row100" >2597</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row100_col0" class="data row100 col0" >326</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row100_col1" class="data row100 col1" >1074</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row101" class="row_heading level0 row101" rowspan=2>kajagafelgaja</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row101" class="row_heading level1 row101" >RT @kajagafelgaja: macie tu moją przemowe #WeLoveTheEarth https://t.co/1KKrPIRllX</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row101" class="row_heading level2 row101" >2526</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row101_col0" class="data row101 col0" >1</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row101_col1" class="data row101 col1" >0</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row102" class="row_heading level1 row102" >RT @kajagafelgaja: czy to Was nie dociera że ta piosenka nie ma być dobra, tylko prosta, trafiająca do wszystkich i wpadająca w ucho? i moż…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row102" class="row_heading level2 row102" >2526</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row102_col0" class="data row102 col0" >30</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row102_col1" class="data row102 col1" >32</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row103" class="row_heading level0 row103" >luvmyburito</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row103" class="row_heading level1 row103" >RT @luvmyburito: głosy ariany i justina tak do siebie pasują ze i'm in shook
#WeLoveTheEarth https://t.co/MLmyex9t1t</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row103" class="row_heading level2 row103" >2520</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row103_col0" class="data row103 col0" >90</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row103_col1" class="data row103 col1" >405</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row104" class="row_heading level0 row104" >lordefurler</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row104" class="row_heading level1 row104" >RT @lordefurler: Sia did that! #WeLoveTheEarth https://t.co/aETvluquXM</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row104" class="row_heading level2 row104" >2488</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row104_col0" class="data row104 col0" >4</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row104_col1" class="data row104 col1" >26</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row105" class="row_heading level0 row105" >sinsnvices</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row105" class="row_heading level1 row105" >RT @sinsnvices: utożsamiam się z brendonem  #WeLoveTheEarth https://t.co/bSLoyWD4hd</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row105" class="row_heading level2 row105" >2478</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row105_col0" class="data row105 col0" >1646</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row105_col1" class="data row105 col1" >3466</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row106" class="row_heading level0 row106" >kingoftheclout</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row106" class="row_heading level1 row106" >RT @kingoftheclout: STOP SCROLLING AND WATCH THIS VIDEO‼️ please take some time out of your day to watch this video &amp; possibly share it. it…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row106" class="row_heading level2 row106" >2124</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row106_col0" class="data row106 col0" >178</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row106_col1" class="data row106 col1" >242</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row107" class="row_heading level0 row107" >izrnsrdn</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row107" class="row_heading level1 row107" >RT @izrnsrdn: Justin Drew Bieber as a baboon 
#WeLoveTheEarth https://t.co/Wz8YK6r21q</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row107" class="row_heading level2 row107" >2034</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row107_col0" class="data row107 col0" >223</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row107_col1" class="data row107 col1" >615</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row108" class="row_heading level0 row108" >marquezduo</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row108" class="row_heading level1 row108" >RT @marquezduo: jakby ktoś się zastanawiał jak wygląda moje życie  #WeLoveTheEarth https://t.co/SLvLE6eY0V</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row108" class="row_heading level2 row108" >1906</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row108_col0" class="data row108 col0" >582</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row108_col1" class="data row108 col1" >1389</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row109" class="row_heading level0 row109" >5SecOfMendess</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row109" class="row_heading level1 row109" >RT @5SecOfMendess: Ejjj ale mi się ta piosenka serio podoba i ma genialne przesłanie!💕
#WeLoveTheEarth</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row109" class="row_heading level2 row109" >1870</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row109_col0" class="data row109 col0" >8</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row109_col1" class="data row109 col1" >35</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row110" class="row_heading level0 row110" >JBwakeup</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row110" class="row_heading level1 row110" >RT @JBwakeup: Ogolnie to serio glos Justina i Ariany pasuja do siebie.. chce ich colab #WeLoveTheEarth</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row110" class="row_heading level2 row110" >1842</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row110_col0" class="data row110 col0" >14</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row110_col1" class="data row110 col1" >58</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row111" class="row_heading level0 row111" rowspan=2>MNotifiedMedia</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row111" class="row_heading level1 row111" >RT @MNotifiedMedia: Shawn Mendes the Rhino🦏 #WeLoveTheEarth https://t.co/Y2plDHsIsL</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row111" class="row_heading level2 row111" >1805</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row111_col0" class="data row111 col0" >93</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row111_col1" class="data row111 col1" >416</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row112" class="row_heading level1 row112" >RT @MNotifiedMedia: “we’re just some rhinos horny as heck” THE RHINOS ARE SO CUTE🦏  #WeLoveTheEarth https://t.co/vvkaO2U7qj</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row112" class="row_heading level2 row112" >1805</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row112_col0" class="data row112 col0" >175</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row112_col1" class="data row112 col1" >647</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row113" class="row_heading level0 row113" >DOLANVVY</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row113" class="row_heading level1 row113" >RT @DOLANVVY: Rozjebał mnie snoop dog jako marihuana, rola idealna dla niego
 #WeLoveTheEarth</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row113" class="row_heading level2 row113" >1803</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row113_col0" class="data row113 col0" >44</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row113_col1" class="data row113 col1" >140</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row114" class="row_heading level0 row114" >justinsbicep</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row114" class="row_heading level1 row114" >RT @justinsbicep: I signed the petition. And you should too. Stream, donate and spread the message. Let's save the earth. #WeLoveTheEarth h…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row114" class="row_heading level2 row114" >1772</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row114_col0" class="data row114 col0" >58</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row114_col1" class="data row114 col1" >101</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row115" class="row_heading level0 row115" >purpxs3bizzle</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row115" class="row_heading level1 row115" >RT @purpxs3bizzle: "O homem destrói a natureza com desculpa de sobreviver, a natureza luta para sobreviver para garantir a sobrevivência do…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row115" class="row_heading level2 row115" >1716</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row115_col0" class="data row115 col0" >144</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row115_col1" class="data row115 col1" >224</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row116" class="row_heading level0 row116" >kba_yankee21</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row116" class="row_heading level1 row116" >RT @kba_yankee21: Never thought I would tear up because of a Lil Dicky song. But here I am crying in a cool way. Feel free to visit https:/…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row116" class="row_heading level2 row116" >1674</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row116_col0" class="data row116 col0" >4</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row116_col1" class="data row116 col1" >6</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row117" class="row_heading level0 row117" >beth_powell1</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row117" class="row_heading level1 row117" >RT @beth_powell1: LETS SAVE THE PLANET #WeLoveTheEarth</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row117" class="row_heading level2 row117" >1629</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row117_col0" class="data row117 col0" >1</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row117_col1" class="data row117 col1" >0</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row118" class="row_heading level0 row118" >piggzyneedy</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row118" class="row_heading level1 row118" >RT @piggzyneedy: i see so many people tweeting about saving the planet but i bet my whole life the majority of those people are only tweeti…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row118" class="row_heading level2 row118" >1577</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row118_col0" class="data row118 col0" >1</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row118_col1" class="data row118 col1" >2</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row119" class="row_heading level0 row119" >particularangel</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row119" class="row_heading level1 row119" >RT @particularangel: i mean we should all listen to this part specifically #WeLoveTheEarth https://t.co/tgGZKRgXV2</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row119" class="row_heading level2 row119" >1511</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row119_col0" class="data row119 col0" >1147</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row119_col1" class="data row119 col1" >2259</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row120" class="row_heading level0 row120" >avisbelieberboy</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row120" class="row_heading level1 row120" >RT @avisbelieberboy: De lo mejor que va del año  #WeLoveTheEarth https://t.co/vN7wPOGork</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row120" class="row_heading level2 row120" >1367</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row120_col0" class="data row120 col0" >3</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row120_col1" class="data row120 col1" >4</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row121" class="row_heading level0 row121" >biebrforro</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row121" class="row_heading level1 row121" >RT @biebrforro: justin y ariana cantando juntos es lo mejor que puede existir  #WeLoveTheEarth https://t.co/Dilro999ds</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row121" class="row_heading level2 row121" >1352</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row121_col0" class="data row121 col0" >292</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row121_col1" class="data row121 col1" >626</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row122" class="row_heading level0 row122" >shawnnsbabyy</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row122" class="row_heading level1 row122" >RT @shawnnsbabyy: Il rinoceronte con la voce più bella del mondo🥰

#WeLoveTheEarth https://t.co/aSQgvumgKl</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row122" class="row_heading level2 row122" >1314</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row122_col0" class="data row122 col0" >2</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row122_col1" class="data row122 col1" >6</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row123" class="row_heading level0 row123" >br_atd</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row123" class="row_heading level1 row123" >RT @br_atd: O melhor porco gordo do universo !!!

 #WeLoveTheEarth https://t.co/lJfZkgnkWI</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row123" class="row_heading level2 row123" >1249</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row123_col0" class="data row123 col0" >1</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row123_col1" class="data row123 col1" >1</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row124" class="row_heading level0 row124" >moarjustln</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row124" class="row_heading level1 row124" >RT @moarjustln: can't believe we listened to justin's voice after months and I still in love 
#WeLoveTheEarth https://t.co/JhFRENhOH3</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row124" class="row_heading level2 row124" >1216</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row124_col0" class="data row124 col0" >58</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row124_col1" class="data row124 col1" >158</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row125" class="row_heading level0 row125" >AGPLOfficial</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row125" class="row_heading level1 row125" >RT @AGPLOfficial: Nawet gdy jest zebrą, Ariana zawsze serwuje wokal! 🎶 #WeLoveTheEarth 

Posłuchaj utworu “Earth” z Arianą Grande i plejadą…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row125" class="row_heading level2 row125" >1152</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row125_col0" class="data row125 col0" >52</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row125_col1" class="data row125 col1" >166</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row126" class="row_heading level0 row126" >xmendesly</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row126" class="row_heading level1 row126" >RT @xmendesly: stream earth by lil dicky
#WeLoveTheEarth</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row126" class="row_heading level2 row126" >1129</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row126_col0" class="data row126 col0" >1</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row126_col1" class="data row126 col1" >0</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row127" class="row_heading level0 row127" >sourdieselzain</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row127" class="row_heading level1 row127" >RT @sourdieselzain: “I’m a fat fucking pig” no ja się tu poplacze zaraz  
#WeLoveTheEarth #EarthMusicVideo https://t.co/tpBQZeRQEh</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row127" class="row_heading level2 row127" >1048</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row127_col0" class="data row127 col0" >34</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row127_col1" class="data row127 col1" >99</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row128" class="row_heading level0 row128" >MrtummyV</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row128" class="row_heading level1 row128" >RT @MrtummyV: This is so cute!! aslskfjdkskskkskak
#WeLoveTheEarth

 https://t.co/ZKKKNFQ1Wf</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row128" class="row_heading level2 row128" >1005</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row128_col0" class="data row128 col0" >10</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row128_col1" class="data row128 col1" >45</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row129" class="row_heading level0 row129" >ringsofmendes</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row129" class="row_heading level1 row129" >RT @ringsofmendes: głos ari tutaj &gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;&gt;
#WeLoveTheEarth https://t.co/QDBVWZ0aom</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row129" class="row_heading level2 row129" >1005</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row129_col0" class="data row129 col0" >22</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row129_col1" class="data row129 col1" >119</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row130" class="row_heading level0 row130" >chimmybby</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row130" class="row_heading level1 row130" >RT @chimmybby: We Also Love The Sun 🌞
#WeLoveTheEarth https://t.co/mh9gfO6Z30</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row130" class="row_heading level2 row130" >1004</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row130_col0" class="data row130 col0" >34</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row130_col1" class="data row130 col1" >129</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row131" class="row_heading level0 row131" >PManice</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row131" class="row_heading level1 row131" >RT @PManice: “I’m a koala and I sleep all the time. So what? It’s cute.” 😍 #WeLoveTheEarth https://t.co/vyOvLY4bSE</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row131" class="row_heading level2 row131" >979</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row131_col0" class="data row131 col0" >33</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row131_col1" class="data row131 col1" >107</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row132" class="row_heading level0 row132" >awanasghena</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row132" class="row_heading level1 row132" >RT @awanasghena: KATY IN TENDENZA IN ITALIA. PIANGO😭🔥❤️
#ConCalmaRemix #WeLoveTheEarth https://t.co/CGnsmSd1gM</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row132" class="row_heading level2 row132" >895</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row132_col0" class="data row132 col0" >7</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row132_col1" class="data row132 col1" >14</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row133" class="row_heading level0 row133" rowspan=2>opssmymendes</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row133" class="row_heading level1 row133" >RT @opssmymendes: bardzo mi się podoba to, że można kupić merch i pieniądze z tego idą na ratowanie naszej planety #WeLoveTheEarth</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row133" class="row_heading level2 row133" >849</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row133_col0" class="data row133 col0" >1</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row133_col1" class="data row133 col1" >2</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row134" class="row_heading level1 row134" >RT @opssmymendes: warto sobie wejść na stronę https://t.co/JyL7e4jrbv i poczytać o co chodzi z każdym zwierzęciem #WeLoveTheEarth</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row134" class="row_heading level2 row134" >849</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row134_col0" class="data row134 col0" >84</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row134_col1" class="data row134 col1" >112</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row135" class="row_heading level0 row135" >_Sunfflower_</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row135" class="row_heading level1 row135" >RT @_Sunfflower_: TA PIOSENKA TO ZŁOTO I DIAMENTY
Oby jej przekaz nie skończył się tylko tylko na tym, że ludzie będą ją kojarzyli, bo spok…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row135" class="row_heading level2 row135" >784</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row135_col0" class="data row135 col0" >164</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row135_col1" class="data row135 col1" >480</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row136" class="row_heading level0 row136" >jaguareffects</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row136" class="row_heading level1 row136" >RT @jaguareffects: eu prestando atenção no áudio pra identificar cada artista

#WeLoveTheEarth https://t.co/0cDtiV2t1E</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row136" class="row_heading level2 row136" >771</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row136_col0" class="data row136 col0" >253</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row136_col1" class="data row136 col1" >380</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row137" class="row_heading level0 row137" >cerberusgrande</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row137" class="row_heading level1 row137" >RT @cerberusgrande: wszyscy mówią jak bardzo chcieliby usłyszeć biebera i ariane (same), ale ja bym chciała usłyszeć ariana x halsey, którą…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row137" class="row_heading level2 row137" >759</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row137_col0" class="data row137 col0" >3</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row137_col1" class="data row137 col1" >7</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row138" class="row_heading level0 row138" >suplisamanobal</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row138" class="row_heading level1 row138" >RT @suplisamanobal: Not blackpink related but Lil dicky x justin bieber, ft ariana grande, and shawn mendes, also charlie puth, and sia and…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row138" class="row_heading level2 row138" >740</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row138_col0" class="data row138 col0" >2</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row138_col1" class="data row138 col1" >6</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row139" class="row_heading level0 row139" rowspan=3>Yuuupthatsme</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row139" class="row_heading level1 row139" >RT @Yuuupthatsme: Jestem pod wrażeniem, że lil dicky był w stanie pokazać tak poważny problem w taki przyjemny sposób
#WeLoveTheEarth</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row139" class="row_heading level2 row139" >669</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row139_col0" class="data row139 col0" >464</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row139_col1" class="data row139 col1" >1256</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row140" class="row_heading level1 row140" >RT @Yuuupthatsme: TEGO SIĘ NIE SPODZIEWAŁAM XD
#WeLoveTheEarth https://t.co/VuhRNdhcV7</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row140" class="row_heading level2 row140" >669</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row140_col0" class="data row140 col0" >70</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row140_col1" class="data row140 col1" >316</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row141" class="row_heading level1 row141" >RT @Yuuupthatsme: Miałeś być żyrafą #WeLoveTheEarth https://t.co/0kNCpU8o6q</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row141" class="row_heading level2 row141" >669</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row141_col0" class="data row141 col0" >8</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row141_col1" class="data row141 col1" >56</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row142" class="row_heading level0 row142" >thremptyworlds</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row142" class="row_heading level1 row142" >RT @thremptyworlds: piosenka jest cudowna, tylko brakuje mi takiego momentu w ktorym wszyscy razem zaspiewaliby refren... #WeLoveTheEarth</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row142" class="row_heading level2 row142" >629</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row142_col0" class="data row142 col0" >55</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row142_col1" class="data row142 col1" >215</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row143" class="row_heading level0 row143" >typicalbizzzle</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row143" class="row_heading level1 row143" >RT @typicalbizzzle: This song and music video had such a powerful and great message it. It’s a wake up call for the whole woltd to get the…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row143" class="row_heading level2 row143" >601</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row143_col0" class="data row143 col0" >694</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row143_col1" class="data row143 col1" >1116</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row144" class="row_heading level0 row144" >liveforshawn02</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row144" class="row_heading level1 row144" >RT @liveforshawn02: This is one of the most beautiful concepts for a video ever! I’m so proud and thankful of @shawnmendes❤️ @lildickytweet…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row144" class="row_heading level2 row144" >578</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row144_col0" class="data row144 col0" >1</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row144_col1" class="data row144 col1" >3</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row145" class="row_heading level0 row145" >aurelialmao</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row145" class="row_heading level1 row145" >RT @aurelialmao: Uważam, że tą piosenkę powinni znać wszyscy, bo może wtedy kogoś ruszy stan naszej planety #EarthMusicVideo #WeLoveTheEarth</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row145" class="row_heading level2 row145" >572</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row145_col0" class="data row145 col0" >1</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row145_col1" class="data row145 col1" >2</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row146" class="row_heading level0 row146" >jaileystation</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row146" class="row_heading level1 row146" >RT @jaileystation: PERO USTEDES ESCUCHARON A ED SHEERAN SIENDO EL KOALA MAS TIERNO QUE EXISTE #WeLoveTheEarth https://t.co/UvmXmy5E1C</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row146" class="row_heading level2 row146" >546</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row146_col0" class="data row146 col0" >332</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row146_col1" class="data row146 col1" >852</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row147" class="row_heading level0 row147" >athenajasvier</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row147" class="row_heading level1 row147" >RT @athenajasvier: lil dicky is amazing #WeLoveTheEarth</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row147" class="row_heading level2 row147" >546</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row147_col0" class="data row147 col0" >2</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row147_col1" class="data row147 col1" >0</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row148" class="row_heading level0 row148" >llostinmyouth</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row148" class="row_heading level1 row148" >RT @llostinmyouth: Ed versione koala è il mio stile di vita dal lunedì alla domenica #WeLoveTheEarth https://t.co/MbhaMaVaRD</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row148" class="row_heading level2 row148" >520</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row148_col0" class="data row148 col0" >11</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row148_col1" class="data row148 col1" >34</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row149" class="row_heading level0 row149" >MercurySimons</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row149" class="row_heading level1 row149" >RT @MercurySimons: everybody should be listening to this masterpiece, the planet really needs our help and it shouldn't take a song with a…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row149" class="row_heading level2 row149" >516</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row149_col0" class="data row149 col0" >275</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row149_col1" class="data row149 col1" >489</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row150" class="row_heading level0 row150" >realestbeach</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row150" class="row_heading level1 row150" >RT @realestbeach: if y’all love the earth, start acting like it. pollution is real and y’all need to start taking it seriously!!  #WeLoveTh…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row150" class="row_heading level2 row150" >504</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row150_col0" class="data row150 col0" >354</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row150_col1" class="data row150 col1" >681</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row151" class="row_heading level0 row151" rowspan=9>Piatt_Futuro</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row151" class="row_heading level1 row151" >RT @Piatt_Futuro: Edilizia motore di rigenerazione urbana. #4future #19aprile #fridaysforfuture ☀️ #WeLoveTheEarth
https://t.co/TdVCrj86L8</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row151" class="row_heading level2 row151" >427</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row151_col0" class="data row151 col0" >4</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row151_col1" class="data row151 col1" >5</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row152" class="row_heading level1 row152" >RT @Piatt_Futuro: Il nostro "ius soli":  terra, suolo e prodotti agricoli 🇮🇹 nel mondo. #4future #19aprile #fridaysforfuture ☀️ #WeLoveTheE…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row152" class="row_heading level2 row152" >427</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row152_col0" class="data row152 col0" >2</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row152_col1" class="data row152 col1" >4</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row153" class="row_heading level1 row153" >RT @Piatt_Futuro: L'ambiente è la leva dello sviluppo economico. #4future #19aprile #fridaysforfuture
#WeLoveTheEarth
https://t.co/TdVCrj86…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row153" class="row_heading level2 row153" >427</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row153_col0" class="data row153 col0" >5</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row153_col1" class="data row153 col1" >5</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row154" class="row_heading level1 row154" >RT @Piatt_Futuro: La persona al centro, l'ambiente intorno. #4future #19aprile ☀️ #fridaysforfuture #WeLoveTheEarth
https://t.co/TdVCrj86L8</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row154" class="row_heading level2 row154" >427</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row154_col0" class="data row154 col0" >4</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row154_col1" class="data row154 col1" >6</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row155" class="row_heading level1 row155" >RT @Piatt_Futuro: Meno gomma, più ferro: trasporti veloci sicuri e non inquinanti. #4future #19aprile #fridaysforfuture ☀️ #WeLoveTheEarth…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row155" class="row_heading level2 row155" >427</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row155_col0" class="data row155 col0" >5</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row155_col1" class="data row155 col1" >6</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row156" class="row_heading level1 row156" >RT @Piatt_Futuro: Riscaldamento globale: soluzioni, non fake news! #4future #19aprile #fridaysforfuture ☀️ #WeLoveTheEarth
https://t.co/TdV…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row156" class="row_heading level2 row156" >427</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row156_col0" class="data row156 col0" >12</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row156_col1" class="data row156 col1" >12</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row157" class="row_heading level1 row157" >RT @Piatt_Futuro: Più mercato e concorrenza per l'accesso di tutti  all'acqua, meno sprechi e inefficienze. #4future #19aprile #fridaysforf…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row157" class="row_heading level2 row157" >427</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row157_col0" class="data row157 col0" >1</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row157_col1" class="data row157 col1" >1</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row158" class="row_heading level1 row158" >RT @Piatt_Futuro: Un grande sconto fiscale per l'ambiente! #4future #19aprile #fridaysforfuture ☀️ #WeLoveTheEarth
https://t.co/TdVCrj86L8</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row158" class="row_heading level2 row158" >427</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row158_col0" class="data row158 col0" >4</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row158_col1" class="data row158 col1" >6</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row159" class="row_heading level1 row159" >RT @Piatt_Futuro: Riciclo riuso e termocombustori: Italia pulita, meno malavita. #4future #19aprile #fridaysforfuture ☀️
#WeLoveTheEarth
ht…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row159" class="row_heading level2 row159" >427</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row159_col0" class="data row159 col0" >4</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row159_col1" class="data row159 col1" >8</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row160" class="row_heading level0 row160" >vanessaibanez_</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row160" class="row_heading level1 row160" >RT @vanessaibanez_: artists using their platform to spread awareness, kudos!! stream the song #WeLoveTheEarth https://t.co/nID21ksupN</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row160" class="row_heading level2 row160" >416</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row160_col0" class="data row160 col0" >1</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row160_col1" class="data row160 col1" >1</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row161" class="row_heading level0 row161" >Adryelli_Godoi</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row161" class="row_heading level1 row161" >RT @Adryelli_Godoi: Justin/ Babuíno: Nós vamos morrer?!
Lil Dicky: Sabe, Bieber nós podemos morrer. Eu não vou mentir pra você. Há muitas p…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row161" class="row_heading level2 row161" >387</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row161_col0" class="data row161 col0" >629</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row161_col1" class="data row161 col1" >1095</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row162" class="row_heading level0 row162" >fayessflatline</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row162" class="row_heading level1 row162" >RT @fayessflatline: This is some true shit #WeLoveTheEarth https://t.co/MLYY7v3JAL</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row162" class="row_heading level2 row162" >381</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row162_col0" class="data row162 col0" >8220</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row162_col1" class="data row162 col1" >16514</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row163" class="row_heading level0 row163" >brian_prism</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row163" class="row_heading level1 row163" >RT @brian_prism: #ConCalmaRemix &amp; #WeLoveTheEarth  Tonight🔥 https://t.co/EjQbaYZYmu</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row163" class="row_heading level2 row163" >348</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row163_col0" class="data row163 col0" >16</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row163_col1" class="data row163 col1" >22</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row164" class="row_heading level0 row164" >Clausbelieberj</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row164" class="row_heading level1 row164" >RT @Clausbelieberj: progetto meraviglioso, vederlo mi ha emozionata.
coinvolgere così tanti artisti per un buona causa è una cosa bellissim…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row164" class="row_heading level2 row164" >337</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row164_col0" class="data row164 col0" >14</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row164_col1" class="data row164 col1" >68</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row165" class="row_heading level0 row165" rowspan=2>ver0017</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row165" class="row_heading level1 row165" >RT @ver0017: oni doskonale wiedzą, że handwritten przyczyniło się do uratowania naszej planety #WeLoveTheEarth https://t.co/XF0JvxrerA</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row165" class="row_heading level2 row165" >288</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row165_col0" class="data row165 col0" >9</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row165_col1" class="data row165 col1" >42</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row166" class="row_heading level1 row166" >RT @ver0017: ciekawi mnie na jakiej podstawie dobierali zwierzęta do artystów 😂🤔 #WeLoveTheEarth</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row166" class="row_heading level2 row166" >288</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row166_col0" class="data row166 col0" >1</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row166_col1" class="data row166 col1" >2</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row167" class="row_heading level0 row167" >dsakrv</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row167" class="row_heading level1 row167" >RT @dsakrv: Lil Dicky - Earth (Official Music Video) https://t.co/M5TtrumWYQ via @YouTube #WeLoveTheEarth</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row167" class="row_heading level2 row167" >239</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row167_col0" class="data row167 col0" >1</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row167_col1" class="data row167 col1" >1</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row168" class="row_heading level0 row168" >CelebrityArticl</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row168" class="row_heading level1 row168" >RT @CelebrityArticl: #ArianaGrande Opens Up About the Darkish Facet of Performing Her New #music “It Is Hell” #Singer #CelebrityNews #WeLov…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row168" class="row_heading level2 row168" >217</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row168_col0" class="data row168 col0" >3</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row168_col1" class="data row168 col1" >2</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row169" class="row_heading level0 row169" >Ws_Vinicius</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row169" class="row_heading level1 row169" >RT @Ws_Vinicius: O Brasil não foi esquecido no churrasco, Rio representando manooo, com os vocais da Ariana de fundo. QUE HINOOOOOOOO EU AM…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row169" class="row_heading level2 row169" >215</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row169_col0" class="data row169 col0" >224</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row169_col1" class="data row169 col1" >516</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row170" class="row_heading level0 row170" >sevenyearsbiebs</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row170" class="row_heading level1 row170" >RT @sevenyearsbiebs: oby wiadomość zawarta w piosence trafiła do dużej ilości osób, może dzięki temu świat zacznie się zmieniać na lepsze…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row170" class="row_heading level2 row170" >214</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row170_col0" class="data row170 col0" >546</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row170_col1" class="data row170 col1" >1083</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row171" class="row_heading level0 row171" >ShawnDailyJP</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row171" class="row_heading level1 row171" >RT @ShawnDailyJP: Lil Dickyの新曲・チャリティーソング「Earth」
アリアナ・ジャスティン・カニエら名だたるアーティスト達と共にショーンも参加してます。
是非聴いてみて下さい。
#WeLoveTheEarth 
https://t.co/CKUtq0…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row171" class="row_heading level2 row171" >199</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row171_col0" class="data row171 col0" >1</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row171_col1" class="data row171 col1" >6</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row172" class="row_heading level0 row172" >Joprojekt</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row172" class="row_heading level1 row172" >RT @Joprojekt: #WeLoveTheEarth Dire que #LinkinPark a écrit deux albums magnifiques sur l'environnement (pas seulement) #AThousandSuns et #…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row172" class="row_heading level2 row172" >182</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row172_col0" class="data row172 col0" >1</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row172_col1" class="data row172 col1" >3</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row173" class="row_heading level0 row173" >justinsmiIxs</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row173" class="row_heading level1 row173" >RT @justinsmiIxs: "Ya sé que no todos somos iguales, pero vivimos en la misma tierra"  
"¿Vamos a morir?" 
"Sabes Bieber? No te voy a menti…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row173" class="row_heading level2 row173" >171</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row173_col0" class="data row173 col0" >530</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row173_col1" class="data row173 col1" >762</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row174" class="row_heading level0 row174" >jujustinbieeber</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row174" class="row_heading level1 row174" >RT @jujustinbieeber: Que la canción y el video no se quede solo un momento y creamos conciencia del daño que hacemos a los animales, al pla…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row174" class="row_heading level2 row174" >167</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row174_col0" class="data row174 col0" >115</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row174_col1" class="data row174 col1" >178</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row175" class="row_heading level0 row175" >sgxjbstylesoft</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row175" class="row_heading level1 row175" >RT @sgxjbstylesoft: la voz de todos esos artistas tan talentosos ha quedado perfecta, amo que se hayan juntando para dejar ese mensaje tan…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row175" class="row_heading level2 row175" >156</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row175_col0" class="data row175 col0" >2</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row175_col1" class="data row175 col1" >1</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row176" class="row_heading level0 row176" rowspan=2>jestemzajeta</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row176" class="row_heading level1 row176" >RT @jestemzajeta: uzbierane pieniądze trafiają na fundację Leonardo DiCaprio, która ratuje naszą planetę 
#WeLoveTheEarth</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row176" class="row_heading level2 row176" >143</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row176_col0" class="data row176 col0" >504</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row176_col1" class="data row176 col1" >1218</td>
            </tr>
            <tr>
                                <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row177" class="row_heading level1 row177" >RT @jestemzajeta: Shawn jest nosorożecem
Halsey lwiątkiem
Charlie Puth żyrafą
Ed Sheeran koalą
Sia kangurem 
miley słoniem 
Ariana zebrą 
K…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row177" class="row_heading level2 row177" >143</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row177_col0" class="data row177 col0" >126</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row177_col1" class="data row177 col1" >408</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row178" class="row_heading level0 row178" >itszGabbs</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row178" class="row_heading level1 row178" >RT @itszGabbs: OUÇAM EARTH! O clipe é o incrível, a música mais ainda e todo dinheiro arrecadado com a música será doado para a instituição…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row178" class="row_heading level2 row178" >105</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row178_col0" class="data row178 col0" >72</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row178_col1" class="data row178 col1" >105</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row179" class="row_heading level0 row179" >Clem_rocher</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row179" class="row_heading level1 row179" >RT @Clem_rocher: Justin Bieber, Ariana Grande, Wiz Khalifa, Shawn Mendes, Ed Sheeran, Rita Ora... de nombreux artistes se mobilisent autour…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row179" class="row_heading level2 row179" >99</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row179_col0" class="data row179 col0" >30</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row179_col1" class="data row179 col1" >54</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row180" class="row_heading level0 row180" >OursPooh</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row180" class="row_heading level1 row180" >RT @OursPooh: we love the earth, it is our planet
we love the earth, it is our home #WeLoveTheEarth https://t.co/NXUhU758uQ</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row180" class="row_heading level2 row180" >98</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row180_col0" class="data row180 col0" >85</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row180_col1" class="data row180 col1" >263</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row181" class="row_heading level0 row181" >soulsfiend</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row181" class="row_heading level1 row181" >RT @soulsfiend: mais les gens qui sont là à se plaindre « gngngn j’aime pas la chanson » vous pouvez connecter vos 2 neurones 2 secondes ?…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row181" class="row_heading level2 row181" >82</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row181_col0" class="data row181 col0" >24</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row181_col1" class="data row181 col1" >35</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row182" class="row_heading level0 row182" >introandrew</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row182" class="row_heading level1 row182" >RT @introandrew: #WeLoveTheEarth é un progetto molto importante,oltre ad essere una canzone,vuole lanciare un messaggio,il SALVARE LA TERRA…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row182" class="row_heading level2 row182" >81</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row182_col0" class="data row182 col0" >53</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row182_col1" class="data row182 col1" >119</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row183" class="row_heading level0 row183" >twentysevenmoon</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row183" class="row_heading level1 row183" >RT @twentysevenmoon: i recommend you guys go watch Our Planet on netflix. it made me cry and i hope it motivates you to take action on savi…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row183" class="row_heading level2 row183" >75</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row183_col0" class="data row183 col0" >106</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row183_col1" class="data row183 col1" >274</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row184" class="row_heading level0 row184" >camillazambett2</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row184" class="row_heading level1 row184" >RT @camillazambett2: Spero che con questa collaborazione arrivi a tutti il messaggio di salvare la Terra!🌍❤️
 #WeLoveTheEarth https://t.co/…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row184" class="row_heading level2 row184" >74</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row184_col0" class="data row184 col0" >3</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row184_col1" class="data row184 col1" >0</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row185" class="row_heading level0 row185" >bemybizzle_</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row185" class="row_heading level1 row185" >RT @bemybizzle_: Justin and Ariana they both sounds so damn good it's just wow !!!!
#WeLoveTheEarth https://t.co/9aRwrD6i9J</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row185" class="row_heading level2 row185" >67</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row185_col0" class="data row185 col0" >762</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row185_col1" class="data row185 col1" >1911</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row186" class="row_heading level0 row186" >xjbtalentedx</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row186" class="row_heading level1 row186" >RT @xjbtalentedx: Yo podría ver y ver éste vídeo nunca me aburriría... ♡

#WeLoveTheEarth https://t.co/HdLb55CNcA</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row186" class="row_heading level2 row186" >58</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row186_col0" class="data row186 col0" >63</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row186_col1" class="data row186 col1" >171</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row187" class="row_heading level0 row187" >__alfredo30__</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row187" class="row_heading level1 row187" >RT @__alfredo30__: Questa canzone è bellissima.
Speriamo riesca a sensibilizzare più persone possibili su questo enorme pericolo 🌍
#WeLoveT…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row187" class="row_heading level2 row187" >55</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row187_col0" class="data row187 col0" >4</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row187_col1" class="data row187 col1" >7</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row188" class="row_heading level0 row188" >protectmayne</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row188" class="row_heading level1 row188" >RT @protectmayne: Earth se tornou minha música preferida junto com o clipe 
isso simplesmente por passar uma mensagem forte pra gente, eu e…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row188" class="row_heading level2 row188" >40</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row188_col0" class="data row188 col0" >294</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row188_col1" class="data row188 col1" >562</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row189" class="row_heading level0 row189" >fabiannn___29</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row189" class="row_heading level1 row189" >RT @fabiannn___29: Me encantaría tanto que esta canción fuera #1 no solo en USA o UK si no en muchos países del mundo. Ojalá que le den el…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row189" class="row_heading level2 row189" >37</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row189_col0" class="data row189 col0" >5</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row189_col1" class="data row189 col1" >8</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row190" class="row_heading level0 row190" >4miraizat</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row190" class="row_heading level1 row190" >RT @4miraizat: ed sheeran(and koala) is my spirit animal #WeLoveTheEarth https://t.co/VYTPuAajM6</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row190" class="row_heading level2 row190" >28</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row190_col0" class="data row190 col0" >68</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row190_col1" class="data row190 col1" >181</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row191" class="row_heading level0 row191" >LILG96958980</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row191" class="row_heading level1 row191" >RT @LILG96958980: Me on my way to save the earth after bobbing to the song  for a straight hour 
#WeLoveTheEarth https://t.co/94g0N2SLBM</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row191" class="row_heading level2 row191" >24</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row191_col0" class="data row191 col0" >1224</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row191_col1" class="data row191 col1" >3448</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row192" class="row_heading level0 row192" >KeyNarumi</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row192" class="row_heading level1 row192" >RT @KeyNarumi: Ini keren sumpah. Pesannya tuh bener bikin lo mikir apa aja yg udah lo lakuin untuk Bumi Kita dengan visual yang menyenangka…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row192" class="row_heading level2 row192" >18</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row192_col0" class="data row192 col0" >1</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row192_col1" class="data row192 col1" >0</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row193" class="row_heading level0 row193" >Yuberli_Rosario</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row193" class="row_heading level1 row193" >RT @Yuberli_Rosario: “Todos estos tiroteos, la contaminación. Nos atacamos a nosotros mismos. Vamos a relajarnos, respeta lo que construimo…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row193" class="row_heading level2 row193" >17</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row193_col0" class="data row193 col0" >24</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row193_col1" class="data row193 col1" >54</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row194" class="row_heading level0 row194" >bieber_najaFC</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row194" class="row_heading level1 row194" >RT @bieber_najaFC: Com o coração quentinho depois de ver que realmente tem pessoas que se importam com o nosso planeta. #WeLoveTheEarth htt…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row194" class="row_heading level2 row194" >14</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row194_col0" class="data row194 col0" >26</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row194_col1" class="data row194 col1" >69</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row195" class="row_heading level0 row195" >khaliqhaiqal_</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row195" class="row_heading level1 row195" >RT @khaliqhaiqal_: Marvel : "Avengers Endgame is gonna be the most ambitious crossover event in history"

Lil Dicky : *hold my beer*
@lildi…</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row195" class="row_heading level2 row195" >13</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row195_col0" class="data row195 col0" >2151</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row195_col1" class="data row195 col1" >5487</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row196" class="row_heading level0 row196" >martyydg</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row196" class="row_heading level1 row196" >RT @martyydg: Semplicemente MERAVIGLIOSO #WeLoveTheEarth https://t.co/m6I2J2soEe</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row196" class="row_heading level2 row196" >12</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row196_col0" class="data row196 col0" >2</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row196_col1" class="data row196 col1" >2</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row197" class="row_heading level0 row197" >biebermylife946</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row197" class="row_heading level1 row197" >RT @biebermylife946: Salviamo il posto in cui viviamo,acquistando la canzone daremo una speranza. #WeLoveTheEarth https://t.co/6OmM8oAvhG</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row197" class="row_heading level2 row197" >12</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row197_col0" class="data row197 col0" >2</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row197_col1" class="data row197 col1" >2</td>
            </tr>
            <tr>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level0_row198" class="row_heading level0 row198" >Malikjvvd</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level1_row198" class="row_heading level1 row198" >RT @Malikjvvd: Uwuuu uwuuuuuu
#WeLoveTheEarth https://t.co/WtVzUrVJW1</th>
                        <th id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66level2_row198" class="row_heading level2 row198" >1</th>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row198_col0" class="data row198 col0" >64</td>
                        <td id="T_567d0e8a_e102_11ea_a0a7_e66463a2af66row198_col1" class="data row198 col1" >208</td>
            </tr>
    </tbody></table>


