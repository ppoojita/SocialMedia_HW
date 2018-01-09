

```python
#Dependencies
import tweepy
import json
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import datetime as dt


# Import and Initialize Sentiment Analyzer
from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer
analyzer = SentimentIntensityAnalyzer()

# Twitter API Keys
consumer_key = 'Kosj5SkJQIwVMDCZsQn6uVSJ3'
consumer_secret = '7WshkaiZoBeXgsDwIb68ZsjJ8E5KflcXPHP5tokRC9gRl5X1Xi'
auth=tweepy.OAuthHandler(consumer_key,consumer_secret)


access_token = '942946531027460096-arfBVpeTExGYB05xEEerckybcUdKdxO'
access_token_secret = 'UPYHQYgiPSeowNoIeq6nLj2E4jLXdQzzTSRfvaeDHiPAH'
auth.set_access_token(access_token,access_token_secret)

api=tweepy.API(auth, parser=tweepy.parsers.JSONParser())


# Target Search Term
target_terms = ("@BBC", "@CNN", "@CBS",
                "@Fox", "@NewYorkTimes")


# Array to hold sentiment
sentiment_array = []
d=[]
tweets=[]
tweet_time=[]




# Variable for holding the oldest tweet
oldest_tweet = ""

# Loop through all target users
for target in target_terms:

    # Variables for holding sentiments
    compound_list = []
    positive_list = []
    negative_list = []
    neutral_list = []
    tweetsago=[]
    tweet_time=0
    

    # Run search around each tweet
    public_tweets = api.search(target, count=100, result_type="recent", max_id=oldest_tweet)

        # Loop through all tweets
    for tweet in public_tweets["statuses"]:
        
       
        compound = analyzer.polarity_scores(tweet["text"])["compound"]
        pos = analyzer.polarity_scores(tweet["text"])["pos"]
        neu = analyzer.polarity_scores(tweet["text"])["neu"]
        neg = analyzer.polarity_scores(tweet["text"])["neg"]
        tweet_text=tweet["text"]
        date_tweet=tweet['created_at']
        tweet_time=tweet_time+1
       
        
    
                # Add each value to the appropriate array
        compound_list.append(compound)
        #print(compound_list)
        positive_list.append(pos)
        negative_list.append(neg)
        neutral_list.append(neu)
        tweetsago.append(tweet_time)
        
        d.append({'Source_Account': target, 'Compound': compound, 'Positive':pos,'Negative':neg, 'Neutral':neu, 'Tweets':tweet_text, 'Timestamp':date_tweet,'Tweets_Ago':tweet_time})

    # Store the Average Sentiments
    sentiment = {"User": target,
                 "Compound": np.mean(compound_list),
                 "Positive": np.mean(positive_list),
                 "Neutral": np.mean(negative_list),
                 "Negative": np.mean(neutral_list),
                 "Tweet_Count":len(tweetsago)}

    # Print the Sentiments
    print(sentiment)
    print("")
    
    
    
df=pd.DataFrame(d)

print(df)



    
    
```

    {'User': '@BBC', 'Compound': 0.17999400000000002, 'Positive': 0.1321, 'Neutral': 0.043589999999999997, 'Negative': 0.8243100000000001, 'Tweet_Count': 100}
    
    {'User': '@CNN', 'Compound': 0.0021050000000000031, 'Positive': 0.094899999999999998, 'Neutral': 0.080159999999999995, 'Negative': 0.82491000000000014, 'Tweet_Count': 100}
    
    {'User': '@CBS', 'Compound': -0.011852999999999997, 'Positive': 0.062100000000000002, 'Neutral': 0.059149999999999994, 'Negative': 0.87878000000000001, 'Tweet_Count': 100}
    
    {'User': '@Fox', 'Compound': -0.047030999999999989, 'Positive': 0.048380000000000006, 'Neutral': 0.055320000000000008, 'Negative': 0.89629999999999999, 'Tweet_Count': 100}
    
    {'User': '@NewYorkTimes', 'Compound': 0.08656999999999998, 'Positive': 0.065879999999999994, 'Neutral': 0.032570000000000009, 'Negative': 0.90154000000000001, 'Tweet_Count': 100}
    
         Compound  Negative  Neutral  Positive Source_Account  \
    0      0.4404     0.000    0.828     0.172           @BBC   
    1      0.2263     0.177    0.651     0.172           @BBC   
    2      0.0000     0.000    1.000     0.000           @BBC   
    3      0.4215     0.000    0.823     0.177           @BBC   
    4      0.4215     0.000    0.517     0.483           @BBC   
    5      0.0000     0.000    1.000     0.000           @BBC   
    6      0.0000     0.000    1.000     0.000           @BBC   
    7      0.5562     0.298    0.088     0.614           @BBC   
    8      0.0000     0.000    1.000     0.000           @BBC   
    9      0.6249     0.000    0.494     0.506           @BBC   
    10     0.0000     0.000    1.000     0.000           @BBC   
    11    -0.6901     0.289    0.711     0.000           @BBC   
    12     0.8225     0.137    0.430     0.434           @BBC   
    13    -0.5960     0.244    0.756     0.000           @BBC   
    14     0.0000     0.000    1.000     0.000           @BBC   
    15     0.6369     0.000    0.724     0.276           @BBC   
    16     0.0000     0.000    1.000     0.000           @BBC   
    17    -0.5994     0.253    0.679     0.068           @BBC   
    18     0.3975     0.051    0.829     0.120           @BBC   
    19     0.4215     0.000    0.823     0.177           @BBC   
    20     0.0000     0.000    1.000     0.000           @BBC   
    21     0.3802     0.000    0.776     0.224           @BBC   
    22     0.0000     0.000    1.000     0.000           @BBC   
    23     0.4215     0.000    0.823     0.177           @BBC   
    24     0.0000     0.000    1.000     0.000           @BBC   
    25     0.0000     0.000    1.000     0.000           @BBC   
    26     0.0000     0.000    1.000     0.000           @BBC   
    27    -0.5994     0.253    0.679     0.068           @BBC   
    28     0.6900     0.000    0.513     0.487           @BBC   
    29     0.3182     0.000    0.827     0.173           @BBC   
    ..        ...       ...      ...       ...            ...   
    470    0.0000     0.000    1.000     0.000  @NewYorkTimes   
    471    0.0000     0.000    1.000     0.000  @NewYorkTimes   
    472    0.0000     0.000    1.000     0.000  @NewYorkTimes   
    473    0.0000     0.000    1.000     0.000  @NewYorkTimes   
    474    0.0000     0.000    1.000     0.000  @NewYorkTimes   
    475    0.0000     0.000    1.000     0.000  @NewYorkTimes   
    476   -0.7096     0.258    0.742     0.000  @NewYorkTimes   
    477    0.0000     0.000    1.000     0.000  @NewYorkTimes   
    478    0.0000     0.000    1.000     0.000  @NewYorkTimes   
    479    0.4404     0.000    0.838     0.162  @NewYorkTimes   
    480    0.4574     0.000    0.800     0.200  @NewYorkTimes   
    481    0.0000     0.000    1.000     0.000  @NewYorkTimes   
    482    0.0000     0.000    1.000     0.000  @NewYorkTimes   
    483   -0.5106     0.231    0.769     0.000  @NewYorkTimes   
    484    0.3612     0.000    0.872     0.128  @NewYorkTimes   
    485   -0.2960     0.145    0.855     0.000  @NewYorkTimes   
    486    0.4019     0.000    0.838     0.162  @NewYorkTimes   
    487   -0.8686     0.423    0.577     0.000  @NewYorkTimes   
    488    0.0000     0.000    1.000     0.000  @NewYorkTimes   
    489    0.6633     0.172    0.408     0.419  @NewYorkTimes   
    490   -0.0772     0.119    0.776     0.105  @NewYorkTimes   
    491   -0.7184     0.333    0.667     0.000  @NewYorkTimes   
    492    0.6124     0.104    0.625     0.271  @NewYorkTimes   
    493    0.0000     0.000    1.000     0.000  @NewYorkTimes   
    494    0.0000     0.000    1.000     0.000  @NewYorkTimes   
    495    0.0000     0.000    1.000     0.000  @NewYorkTimes   
    496    0.0000     0.000    1.000     0.000  @NewYorkTimes   
    497    0.0000     0.000    1.000     0.000  @NewYorkTimes   
    498    0.2937     0.099    0.747     0.154  @NewYorkTimes   
    499    0.2937     0.099    0.747     0.154  @NewYorkTimes   
    
                              Timestamp  \
    0    Sun Jan 07 18:40:59 +0000 2018   
    1    Sun Jan 07 18:40:47 +0000 2018   
    2    Sun Jan 07 18:40:44 +0000 2018   
    3    Sun Jan 07 18:40:32 +0000 2018   
    4    Sun Jan 07 18:40:14 +0000 2018   
    5    Sun Jan 07 18:40:04 +0000 2018   
    6    Sun Jan 07 18:39:49 +0000 2018   
    7    Sun Jan 07 18:39:41 +0000 2018   
    8    Sun Jan 07 18:39:06 +0000 2018   
    9    Sun Jan 07 18:39:03 +0000 2018   
    10   Sun Jan 07 18:38:51 +0000 2018   
    11   Sun Jan 07 18:38:44 +0000 2018   
    12   Sun Jan 07 18:38:43 +0000 2018   
    13   Sun Jan 07 18:38:35 +0000 2018   
    14   Sun Jan 07 18:38:34 +0000 2018   
    15   Sun Jan 07 18:38:33 +0000 2018   
    16   Sun Jan 07 18:38:24 +0000 2018   
    17   Sun Jan 07 18:38:13 +0000 2018   
    18   Sun Jan 07 18:38:04 +0000 2018   
    19   Sun Jan 07 18:38:01 +0000 2018   
    20   Sun Jan 07 18:38:01 +0000 2018   
    21   Sun Jan 07 18:37:56 +0000 2018   
    22   Sun Jan 07 18:37:51 +0000 2018   
    23   Sun Jan 07 18:37:49 +0000 2018   
    24   Sun Jan 07 18:37:38 +0000 2018   
    25   Sun Jan 07 18:37:27 +0000 2018   
    26   Sun Jan 07 18:37:18 +0000 2018   
    27   Sun Jan 07 18:37:13 +0000 2018   
    28   Sun Jan 07 18:37:04 +0000 2018   
    29   Sun Jan 07 18:37:01 +0000 2018   
    ..                              ...   
    470  Thu Jan 04 20:58:49 +0000 2018   
    471  Thu Jan 04 20:57:07 +0000 2018   
    472  Thu Jan 04 20:19:24 +0000 2018   
    473  Thu Jan 04 18:44:17 +0000 2018   
    474  Thu Jan 04 16:42:37 +0000 2018   
    475  Thu Jan 04 16:41:20 +0000 2018   
    476  Thu Jan 04 16:21:02 +0000 2018   
    477  Thu Jan 04 15:40:23 +0000 2018   
    478  Thu Jan 04 15:08:17 +0000 2018   
    479  Thu Jan 04 15:02:04 +0000 2018   
    480  Thu Jan 04 14:07:14 +0000 2018   
    481  Thu Jan 04 12:05:15 +0000 2018   
    482  Thu Jan 04 08:50:18 +0000 2018   
    483  Thu Jan 04 08:49:34 +0000 2018   
    484  Thu Jan 04 08:48:30 +0000 2018   
    485  Thu Jan 04 08:46:43 +0000 2018   
    486  Thu Jan 04 08:42:37 +0000 2018   
    487  Thu Jan 04 08:41:32 +0000 2018   
    488  Thu Jan 04 06:53:01 +0000 2018   
    489  Thu Jan 04 03:22:52 +0000 2018   
    490  Thu Jan 04 02:36:15 +0000 2018   
    491  Thu Jan 04 02:18:47 +0000 2018   
    492  Thu Jan 04 01:51:28 +0000 2018   
    493  Wed Jan 03 18:49:39 +0000 2018   
    494  Wed Jan 03 18:40:40 +0000 2018   
    495  Wed Jan 03 18:18:58 +0000 2018   
    496  Wed Jan 03 17:20:22 +0000 2018   
    497  Wed Jan 03 17:04:36 +0000 2018   
    498  Wed Jan 03 14:29:43 +0000 2018   
    499  Wed Jan 03 13:28:12 +0000 2018   
    
                                                    Tweets  Tweets_Ago  
    0    RT @DavidGarval: More than 230000 signaturas s...           1  
    1    @qikipedia Not in the Netherlands, unfortunate...           2  
    2    政治家とは？個人の顔なし@NOSUKE0607: 「与党からの参加者はゼロ」\nこれが何を意...           3  
    3    RT @BBC: What's the true scale of the global t...           4  
    4                                @BBC Donald Trump lol           5  
    5    @BBC @guardian @NPR @PBS @maddow @funder @USAT...           6  
    6    @BBC @BBCCarrie has quit as China Editor becau...           7  
    7                         @BBC Wow. Truly disgusting !           8  
    8    @BlaineCoughlan @BBC @bbcdoctorwho @DWTheFanSh...           9  
    9                  @MockTheWeek @BBC That's great news          10  
    10   RT @BBC: Tonight, David Attenborough attempts ...          11  
    11   RT @miss_l_locket: @aev1609 @Teel18 @campbellc...          12  
    12   @BBC @BBCiPlayer Congrats @mrneilforsyth on a ...          13  
    13   @GaryLineker If the @BBC is so pc why can’t th...          14  
    14   RT @CyberHibby: @Leahcimnosined @ariyana69 @BB...          15  
    15   RT @BBC: Budget Italian feasts the whole famil...          16  
    16   RT @BBC: Tonight, David Attenborough attempts ...          17  
    17   RT @raju: One Of The @BBC's Top Female Journal...          18  
    18   RT @campbellclaret: Surely she will also be al...          19  
    19   RT @BBC: What's the true scale of the global t...          20  
    20   Threaders at the endless stream of singing sho...          21  
    21   @BBC please make this happen! #DavidAttenborou...          22  
    22   RT @BBC: Tonight, David Attenborough attempts ...          23  
    23   RT @BBC: What's the true scale of the global t...          24  
    24   @BBC When the Doctor regenerates, what happens...          25  
    25   RT @BBC: Tonight, David Attenborough attempts ...          26  
    26   RT @BBC: Tonight, David Attenborough attempts ...          27  
    27   RT @raju: One Of The @BBC's Top Female Journal...          28  
    28         @tom_bondy @BBC Def good and you will like!          29  
    29   When is @TheFourOnFOX airing in the UK please ...          30  
    ..                                                 ...         ...  
    470  @elkloden @washingtonpost @newyorktimes Ahhh.....          71  
    471  @elkloden Debemos DENUNCIAR LA  PRENSA en #Esp...          72  
    472  @NewYorkTimes_es @NewYorkTimes \n\n¡100 𝖉𝖎𝖆𝖘! ...          73  
    473  First #threewords of this @newyorktimes #headl...          74  
    474  Wondering what new books to dive into? @pbsnew...          75  
    475  Wondering what new books to dive into? @pbsnew...          76  
    476  https://t.co/eXklbx72lI\n\nWith the recent fat...          77  
    477  @LaOtraCronica Que temas tan trascendentales y...          78  
    478  @LaureeTilton @SarahLerner @washingtonpost @ne...          79  
    479  @Issybelle99 @SarahLerner @washingtonpost @new...          80  
    480  In Finland, a Postal Worker Will Mow Your Lawn...          81  
    481  Sasakawa USA 2017 Journalism Fellow Seth Berkm...          82  
    482  @LaureeTilton @SarahLerner @washingtonpost @ne...          83  
    483  @LaureeTilton @SarahLerner @washingtonpost @ne...          84  
    484  @LaureeTilton @SarahLerner @washingtonpost @ne...          85  
    485  @Issybelle99 @SarahLerner @washingtonpost @new...          86  
    486  @LaureeTilton @SarahLerner @washingtonpost @ne...          87  
    487  @SarahLerner @Issybelle99 REALLY! I'm so tired...          88  
    488  The majority of American's did not vote for Tr...          89  
    489  @realDonaldTrump @CNN\n@FOXANDFRIENDS @NEWYORK...          90  
    490  A fresh #perspective on the potential of #A.I....          91  
    491  @TuckerCarlson @NewYorkTimes @CBSNews  #Irania...          92  
    492  If you enjoyed seeing "Isaac Oliver's Lonely C...          93  
    493  RT @Dansereau31: @realDonaldTrump\n@StephenBan...          94  
    494  @realDonaldTrump\n@StephenBannon @IvankaTrump ...          95  
    495  Sasakawa USA 2017 Journalism Fellow Seth Berkm...          96  
    496  @NewYorkTimes what a difference in constructio...          97  
    497  A Note From Our New Publisher https://t.co/mxU...          98  
    498  RT @OlivierGuitta: #IranProtests: Kudos to @ne...          99  
    499  RT @OlivierGuitta: #IranProtests: Kudos to @ne...         100  
    
    [500 rows x 8 columns]



```python
result_BBC=df.loc[df['Source_Account'] == '@BBC']
#print(result_BBC.head())

BBC_Compound=np.mean(result_BBC['Compound'])

print(BBC_Compound)



result_CNN=df.loc[df['Source_Account'] == '@CNN']

CNN_Compound=np.mean(result_CNN['Compound'])

print(CNN_Compound)


result_CBS=df.loc[df['Source_Account'] == '@CBS']

CBS_Compound=np.mean(result_CBS['Compound'])

print(CBS_Compound)


result_Fox=df.loc[df['Source_Account'] == '@Fox']

Fox_Compound=np.mean(result_Fox['Compound'])

print(Fox_Compound)

result_NYT=df.loc[df['Source_Account'] == '@NewYorkTimes']

NYT_Compound=np.mean(result_NYT['Compound'])

print(NYT_Compound)


```

    0.17999400000000002
    0.002105000000000003
    -0.011853000000000008
    -0.04703099999999998
    0.08657



```python
ax=result_BBC.plot.scatter(x='Tweets_Ago', y='Compound',color='skyblue',s=200,label='BBC',alpha=0.7, edgecolor='black',linewidth=2.0)

result_CNN.plot.scatter(x='Tweets_Ago', y='Compound', color='red',s=200,ax=ax,label='CNN',alpha=0.7, edgecolor='black',linewidth=2.0)

result_CBS.plot.scatter(x='Tweets_Ago', y='Compound', color='green',s=200,ax=ax,label='CBS',alpha=0.7, edgecolor='black',linewidth=2.0)

result_Fox.plot.scatter(x='Tweets_Ago', y='Compound', color='blue',s=200,ax=ax,label='Fox',alpha=0.7, edgecolor='black',linewidth=2.0)

result_NYT.plot.scatter(x='Tweets_Ago', y='Compound', color='yellow',s=200,ax=ax,label='NYT',alpha=0.7, edgecolor='black',linewidth=2.0)

fig_size = plt.rcParams["figure.figsize"]

fig_size[0] = 15
fig_size[1] = 9
plt.rcParams["figure.figsize"] = fig_size

plt.xlabel("Tweets Ago")
plt.ylabel("Tweet Polarity")
plt.title("Sentiment Analysis of Media Tweets" + "("+dt.datetime.today().strftime("%m/%d/%Y")+")")
ax.legend(loc='best', borderpad=2,labelspacing=2, title='Media Sources', bbox_to_anchor=(1, 1))
plt.grid(True,color='black')
ax.set_facecolor("lightgrey")

plt.savefig('Sentiment_MediaTweets.png', dpi=100)

plt.show()





```


![png](SocialMedia_HW_files/SocialMedia_HW_2_0.png)



```python
target_terms = ("@BBC", "@CNN", "@CBS",
                "@Fox", "@NewYorkTimes")


x_values = np.arange(len(target_terms))
y_values=[BBC_Compound, CNN_Compound, CBS_Compound, Fox_Compound, NYT_Compound]
colors=['lightblue','red','green','blue','yellow']


plt.title("Overall Media Sentiment based on Twitter" + "("+dt.datetime.today().strftime("%m/%d/%Y")+")")
plt.xlabel("")
plt.ylabel("Tweet Polarity")

fig_size = plt.rcParams["figure.figsize"]

fig_size[0] = 8
fig_size[1] = 8
plt.rcParams["figure.figsize"] = fig_size

plt.bar(x_values, y_values,color=colors, tick_label=target_terms,width=1)

plt.savefig('OverallMedia_Sentiment.png', dpi=100)

  
plt.show()
```


![png](SocialMedia_HW_files/SocialMedia_HW_3_0.png)

