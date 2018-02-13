---
layout: post
title: "About the Author"
author: "Paul Le"
categories: journal
tags: [documentation,sample]
image: cutting.jpg
---


```python
# Checking sparkContext
sc
```




    <pyspark.context.SparkContext at 0x10997a3d0>




```python
# Importing required libraries
import twitter
import json
import time
from pyspark import SparkContext
import dateutil.parser
from pyspark.streaming import StreamingContext
```


```python
# Twitter Credentials
def connect_twitter():
    CONSUMER_KEY = "5zzIJtCwVHFiOoC6qLNRVmvM6"
    CONSUMER_SECRET = "U0Me2eN6p9QjF7lNbTv1jaLx58ZshIl8zU4Eju5fbs0I6bohr7"
    OAUTH_TOKEN = "610909012-dqAyceQeS58v22fXDRepGH0FMtSsdKLFWsNytD3U"
    OAUTH_TOKEN_SECRET = "xCMGs4G5CZ0OZS9QjHifXvpKTf42CboubdVPwP1wDHF6n" 
    twitter_stream = twitter.TwitterStream(auth=twitter.OAuth(
        token = OAUTH_TOKEN,
        token_secret = OAUTH_TOKEN_SECRET,
        consumer_key = CONSUMER_KEY,
        consumer_secret = CONSUMER_SECRET))
    return twitter_stream
```


```python
class Tweet(dict):
    """
        Classe dictionnaire pour initialiser un dictionnaire avec les informations d'un tweet.
    """
    def __init__(self, tweet_in, encoding  = 'utf-8'):
        super(Tweet, self).__init__(self)
        if tweet_in and 'delete' not in tweet_in:
            self['id']           = tweet_in['id']
            self['geo']          = tweet_in['geo']['coordinates'] if tweet_in['geo'] else None
            self['text']         = tweet_in['text'].encode(encoding)
            self['user_id']      = tweet_in['user']['id'] 
            self['hashtags']     = [x['text'].encode(encoding) for x in tweet_in['entities']['hashtags']]
            self['timestamp']    = dateutil.parser.parse(tweet_in[u'created_at']).replace(tzinfo=None).isoformat()
            self['screen_name']  = tweet_in['user']['screen_name'].encode(encoding)
```


```python
def get_next_tweet(twitter_stream, i ):
    """
    Return : JSON
    """
    block    = False # True
    stream   = twitter_stream.statuses.sample(block=False) 
    tweet_in = None        
    while not tweet_in or 'delete' in tweet_in:
        tweet_in     = stream.next()
        tweet_parsed = Tweet(tweet_in)

    return json.dumps(tweet_parsed)
```


```python
def process_rdd_queue(twitter_stream, nb_tweets = 5):
    """
     Create a queue of RDDs that will be mapped/reduced one at a time in 1 second intervals.
    """
    rddQueue = []
    for i in range(nb_tweets):
        json_twt  = get_next_tweet(twitter_stream, i ) 
        dist_twt  = ssc.sparkContext.parallelize([json_twt], 5)
        rddQueue += [dist_twt]

    lines = ssc.queueStream(rddQueue, oneAtATime=False)
    lines.pprint()
```


```python
ssc = StreamingContext(sc, 1)
```


```python
twitter_stream = connect_twitter()
process_rdd_queue(twitter_stream)

ssc.start()
time.sleep(2)
```

    -------------------------------------------
    Time: 2017-01-26 01:57:43
    -------------------------------------------
    {"user_id": 2831727884, "screen_name": "norang214782", "text": "RT @FemiQuotes_kr: \ub0a8\uc131\uc740 \uc5ec\uc131\uc774 \ubc18\ub780\uc744 \uc77c\uc73c\ud0a4\uba74, \ub0a8\uc131\uc774 \uc5ec\uc131\uc744 \uc9c0\ubc30\ud574\uc654\ub4ef\uc774 \uc5ec\uc131\uc774 \ub0a8\uc131\uc744 \uc9c0\ubc30\ud558\ub294 \uc138\uc0c1\uc774 \uc624\ub9ac\ub77c\uace0 \uc0c1\uc0c1\ud55c\ub2e4. \u2015\uc0d0\ub9ac \ucf10\ud134 https://t.co/WE7MVtMrQF", "hashtags": [], "timestamp": "2017-01-26T07:57:40", "geo": null, "id": 824526707373662209}
    {"user_id": 744591545618268161, "screen_name": "LKralovna", "text": "RT @tzzfnibby: jsem projela n\u011bjak\u00fd ty msngr stories a co je to to \u201emiluju adamka\", \u201emiluju denisku, lucinku, janicku\" etc\n???", "hashtags": [], "timestamp": "2017-01-26T07:57:41", "geo": null, "id": 824526711563943938}
    {"user_id": 4592980152, "screen_name": "Truth_of_HELL", "text": "NO ONE LOVES https://t.co/4kQII2xNBD #Aljazeera #Iraq #Iran #Sirya #Egypt #Arab #UAE #Islam #Muslim #Jihad #Quran #Saudi #Yemen #Afganistan", "hashtags": ["Aljazeera", "Iraq", "Iran", "Sirya", "Egypt", "Arab", "UAE", "Islam", "Muslim", "Jihad", "Quran", "Saudi", "Yemen", "Afganistan"], "timestamp": "2017-01-26T07:57:41", "geo": null, "id": 824526711559643136}
    {"user_id": 123748187, "screen_name": "Jibboung", "text": "#\ub124\uc774\ubc84\uc0dd\uc131\uc544\uc774\ub514\ud310\ub9e4\n#\uc804\ud654:070-7896-9490\n#\ud398\uc774\uc2a4\ubd81\uc544\uc774\ub514\ud310\ub9e4\n#\uad6c\uae00\uc544\uc774\ub514\ud310\ub9e4\n#\ud2b8\uc704\ud130\uc544\uc774\ub514\ud310\ub9e4\n#\ub124\uc774\ubc84\uc544\uc774\ub514\ud310\ub9e4\n#\uc778\uc2a4\ud0c0\uadf8\ub7a8\uc544\uc774\ub514\ud310\ub9e4\n#\ub2e4\uc74c\uc544\uc774\ub514\ud310\ub9e4\nskype: naver007kim\n$!~&amp;&amp;&amp;", "hashtags": ["\ub124\uc774\ubc84\uc0dd\uc131\uc544\uc774\ub514\ud310\ub9e4", "\uc804\ud654", "\ud398\uc774\uc2a4\ubd81\uc544\uc774\ub514\ud310\ub9e4", "\uad6c\uae00\uc544\uc774\ub514\ud310\ub9e4", "\ud2b8\uc704\ud130\uc544\uc774\ub514\ud310\ub9e4", "\ub124\uc774\ubc84\uc544\uc774\ub514\ud310\ub9e4", "\uc778\uc2a4\ud0c0\uadf8\ub7a8\uc544\uc774\ub514\ud310\ub9e4", "\ub2e4\uc74c\uc544\uc774\ub514\ud310\ub9e4"], "timestamp": "2017-01-26T07:57:41", "geo": null, "id": 824526711567978498}
    {"user_id": 3159745538, "screen_name": "HotironMeuMeu", "text": "\u3042\u3065\u3081\u3081\u3046\u3065\u3046\u3042\u3046\u3046\u3046\u3046\u3044\u3046\u3046\u3046\u3042\u3065\u3046\u3081\u3046\u3065\u3081\u3081\u3065\u3081\u3044\u3065\u3065\u3065\u3081\u3046\u3046\u3042\u3065\u3081\u3065\u3042\u3042\u3081\u3044\u3046\u3044\u3044\u3046\u3042\u3081\u3081\u3081\u3065\u3042\u3081\u3046\u3042\u3044\u3081\u3042\u3081\u3044\u3042\u3044\u3044\u3046\u3046\u3046\u3065\u3044\u3065\u3044\u3046\u3042\u3046\u3044\u3042\u3044\u3044\u3044\u3044\u3044\u3046\u3042\u3042\u3044\u3065\u3065\u3081\u3065\u3081\u3044\u3046\u3081\u3081\u3042\u3044\u3065\u3044\u3042\u3065\u3046\u3042\u3065\u3042\u3046\u3065\u3081\u3081\u3044\u3081\u3042\u3065\u3046\u3065\u3042\u3046\u3081\u3044\u3065\u3046\u3042\u3044\u3046\u3044\u3044\u3081\u3046\u3081\u3065\u3081\u3065\u3046\u3046\u3044\u3046\u3065\u3046\u3065\u3046\u3081\u3042\u3044", "hashtags": [], "timestamp": "2017-01-26T07:57:42", "geo": null, "id": 824526715770830849}
    
    -------------------------------------------
    Time: 2017-01-26 01:57:44
    -------------------------------------------
    
    -------------------------------------------
    Time: 2017-01-26 01:57:45
    -------------------------------------------
    
    -------------------------------------------
    Time: 2017-01-26 01:57:46
    -------------------------------------------
    
    -------------------------------------------
    Time: 2017-01-26 01:57:47
    -------------------------------------------
    
    -------------------------------------------
    Time: 2017-01-26 01:57:48
    -------------------------------------------
    
    -------------------------------------------
    Time: 2017-01-26 01:57:49
    -------------------------------------------
    
    -------------------------------------------
    Time: 2017-01-26 01:57:50
    -------------------------------------------
    
    -------------------------------------------
    Time: 2017-01-26 01:57:51
    -------------------------------------------
    
    -------------------------------------------
    Time: 2017-01-26 01:57:52
    -------------------------------------------
    



```python
ssc.start()
```


    ---------------------------------------------------------------------------

    IllegalArgumentException                  Traceback (most recent call last)

    <ipython-input-17-fadb2db3cba3> in <module>()
    ----> 1 ssc.start()
    

    /Users/kbsriharsha/spark-1.6.1-bin-hadoop2.6/python/pyspark/streaming/context.pyc in start(self)
        277         Start the execution of the streams.
        278         """
    --> 279         self._jssc.start()
        280         StreamingContext._activeContext = self
        281 


    /Users/kbsriharsha/spark-1.6.1-bin-hadoop2.6/python/lib/py4j-0.9-src.zip/py4j/java_gateway.py in __call__(self, *args)
        811         answer = self.gateway_client.send_command(command)
        812         return_value = get_return_value(
    --> 813             answer, self.gateway_client, self.target_id, self.name)
        814 
        815         for temp_arg in temp_args:


    /Users/kbsriharsha/spark-1.6.1-bin-hadoop2.6/python/pyspark/sql/utils.pyc in deco(*a, **kw)
         51                 raise AnalysisException(s.split(': ', 1)[1], stackTrace)
         52             if s.startswith('java.lang.IllegalArgumentException: '):
    ---> 53                 raise IllegalArgumentException(s.split(': ', 1)[1], stackTrace)
         54             raise
         55     return deco


    IllegalArgumentException: u'requirement failed: No output operations registered, so nothing to execute'



```python
ssc.stop(stopSparkContext=True, stopGraceFully=True)
```

    -------------------------------------------
    Time: 2017-01-26 01:57:53
    -------------------------------------------
    
    -------------------------------------------
    Time: 2017-01-26 01:57:54
    -------------------------------------------
    
    -------------------------------------------
    Time: 2017-01-26 01:57:55
    -------------------------------------------
    



```python

```
