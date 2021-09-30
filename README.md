
## Sentiment classification models on Covid-19 tweets by BERT and CNN across time and space.

## *Introduction*

Since the end of 2019, COVID-19 broke out gradually country by country throughout the world, almost no place on earth could stay out of this epidemic. Human got different feelings: Some feel frightened, angry, and desperate, while the others feel that it is just a bad cold, and sooner or later, human beings will overcome the virus. Neural network model such as BERT and natural language processing method to the Covid- 19 related social media data to do sentiment analysis, so that to find out the best model for this kind of application. At the same time, it is interested to find in the same world, do people in different country got the same feeling towards the pandemic, and the role of time and space play.


#### Tweets Sentiment-Daily cases distribution in US
<img width="822" alt="image" src="https://user-images.githubusercontent.com/49421464/135540107-5dcc9d91-a461-4b30-8ef6-5887ada38d6b.png">


#### Tweets Sentiment-Daily cases distribution in UK
<img width="827" alt="image" src="https://user-images.githubusercontent.com/49421464/135540242-adec2f5a-37b5-4974-8992-82af32faf888.png">


#### Tweets daily sentiment to keyword “mask”
<img width="796" alt="image" src="https://user-images.githubusercontent.com/49421464/135540318-f483cfb9-9993-4edc-a641-3b85bda61aaf.png">



## *Data:*
### Training Dataset:
- source: [SemEval-2017 Task A](https://alt.qcri.org/semeval2017/task4/index.php?id=data-and-tools)  
- size: train:43860, dev:1999, test:2390 tweets labeled by polarity(positive, neutral, negative).
### Implementation Dataset:
Thanks to Chiyu, we are able to build our work upon UBC-NLP's Mega-COV V0.2. According to its data organisation  [readme](https://github.com/UBC-NLP/megacov/tree/master/tweet_ids), we can get the tweet\_ids by country. 

We then plan to pass the tweet_id as an argument to `tweepy`, a python module to retrieve data from twitter as taught in COLX\_523 [here](https://github.ubc.ca/MDS-CL-2020-21/COLX_523_adv-corp-ling_students/blob/master/blank_lectures/Twitter_tutorial.ipynb) in order to get the text of a tweet.

Our preliminary research on twitter's developer API policy suggests that using a single token may get us 500k requests per month. This means, if the four of us in the team each get a token in time, we may be able to scrape 2 million tweets per month. Also we are aware one person could be able to even get multiple tokens so we might be able to "ramp up" our data-gathering operation if necessary.
 
In terms of data storage, we intend to keep 5 fields of metadata, if possible, in Json format:  
1. id: a unique identifier inherited from Mega-COV  
2. txt: text of a tweet  
3. loc: country code (CA, AU, UK, US) of the tweet.  
4. date: date of the tweet    
5. hashtag: keywords about the product.  
6. language: English, Chinese(optinal, if we later decide to further develop our model into a multi-lingual one)


## *Engineering:*
### Models
- CNN
- BERT-Classfication
- RoBERTa with Adapter Module
- linear regression (baseline)


###  Framework:
PyTorch (required). 
