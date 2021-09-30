Project Progress Report
---
link to project repo:
[https://github.ubc.ca/lisa67/CL585-COVID19-Sentiment-Analysis-Team](https://github.ubc.ca/lisa67/CL585-COVID19-Sentiment-Analysis-Team)

Team Members:

Yundong Yao,
Quang Vu,
Lisa Liu,
Alex Chen.

## *Introduction:* 
In this project, we intend to answer the following question:
> Did English-speaking people from different countries show different sentiments regarding term at a particular stage of the pandemic?

To do this, we use a pre-trained model to apply Sentiment Analysis to tweets that are:

- about terms, i.e. containing a number of keywords (TBD).

- from 3 or 4 English-speaking countries in different regions of the world:
   - Canada (Americas)
   - Australia (Asia-Pacific)
   - United Kingdom (Europe)
   - (Optional) United States (also Americas),  
- within 1 week in the year 2020. (subject to change if dataset size is not sufficiently large) 

## *Motivation:*
We as practitioners think this question is important due to its relevance in: 

- commerce:  
	The pandemic reshapes both demand and supply. Trans-national companies would be interested to learn how the public in different continents are reacting. This insight would help them to better adapt to the disruption to their global supply chain.
	
- policy:  
  Policy-makers, pollsters, and the press may be interested to feel the "pulse" of public opinion, domestic and foreign, towards supplies. This knowledge may make more informed policy-making possible.
  
- society:  
  Online echo chambers may make people feel either more anxious or more unconcerned than they should be. It might help to put into a broader context their attitudes towards supplies.


## *Data:*
### Training Dataset:

- source: [SemEval-2017 Task A](https://alt.qcri.org/semeval2017/task4/index.php?id=data-and-tools)


- size: train:43860, dev:1999, test:2390 tweets labeled by polarity(positive, neutral, negative).

### Inference Dataset:

Our plan to get inference dataset is as follows:

0. Pick a dehydrated dataset 
1. Get tweetIDs by country, e.g. Canada
2. Use `twarc` and Twitter API to crawl tweets (Proof-of-concept crawler [here](https://github.ubc.ca/lisa67/CL585-COVID19-Sentiment-Analysis-Team/blob/master/milestone2/data-collection/data-collection-proof-of-concept.ipynb))
3. Anonymize tweets by replacing urls with <url> token
4. Further pre-process data by:
    - replacing emoticons by text such as <smile>;
    - truncating any letter repeating more than 2 times in a row (e.g.  sooooo -> soo).

Our preliminary research on twitter's developer API policy suggests that using a single token may get us 500k requests per month. This means, if the four of us in the team each get a token in time, we may be able to scrape 2 million tweets per month. Also we are aware one person could be able to even get multiple tokens so we might be able to "ramp up" our data-gathering operation if necessary.
 
In terms of data storage, we intend to keep 5 fields of metadata, if possible, in csv format:  
1. id: a unique identifier inherited from Mega-COV  
2. txt: text of a tweet  
3. loc: country code (CA, AU, UK, optionally US) of the tweet.  
4. date: date of the tweet    
5. hashtag: keywords about the product.  
6. language: English, Chinese(optional, if we later decide to further develop our model into a multi-lingual one)


## *Engineering:*
### computing infrastructure:

We will first try with Personal computers only. We will consider using Google Colab and/or Google Cloud TPU if it is needed.

### deep learning of NLP method:

We would choose one of the models below with best f1 score in the training phrase to do the classification on Covid-19 relevant tweets.
- CNN: We have done some pilot research on CNN, based on the training data, we got 0.37 f1 score by now.
- BERT-Classification
- RoBERTa with Adapter Module
- DANN (Domain Adversarial Neural Network)
- LinearSVC (baseline), F1 score:0.47

- After the pilot models experiment we decided to choose pre-trained language model BERTweet as our final model to do the inference on tweet data.
- From the SemEval-2017 training dataset, we now get 0.618 F1 score and 0.628 recall score.


###  Framework:
PyTorch (required). 

### existing codebase:
- [Sentiment_Analysis_on_Bertweet.ipynb](https://github.ubc.ca/lisa67/CL585-COVID19-Sentiment-Analysis-Team/blob/master/src/Sentiment_Analysis_on_Bertweet.ipynb)

- [COLX585 Models 
tutorial](https://github.ubc.ca/MDS-CL-2020-21/COLX_585_trends_students/tree/master/tutorials)

- [COLX523 tweepy tutorial](https://github.ubc.ca/MDS-CL-2020-21/COLX_523_adv-corp-ling_students/blob/master/blank_lectures/Twitter_tutorial.ipynb)


## *Previous Works (minimal):*
-  [H. Manguri, K., N. Ramadhan, R. and R. Mohammed Amin, P. (2020) “Twitter Sentiment Analysis on Worldwide COVID-19 Outbreaks”, Kurdistan Journal of Applied Research, 5(3), pp. 54-65.](https://www.researchgate.net/publication/341500307_Twitter_Sentiment_Analysis_on_Worldwide_COVID-19_Outbreaks)
     
    - This sentiment analysis is based on the analysis of tweets data scraping from Twitter social media on two specified hashtag keywords, (“COVID-19, coronavirus”). The date of searching data is seven days from 09-04-2020 to 15-04-2020, 530232 tweets were collected.
    - In this paper, "Emotional Guidance Scale” for polarity evaluation is setup for the analysis.
    - The study shows that people's reactions vary day to day from posting their feelings on social media specifically Twitter.

- [BB twtr at SemEval-2017 Task 4: Twitter Sentiment Analysis with CNNs and LSTMs](https://www.aclweb.org/anthology/S17-2094.pdf)
    - As the 1st rank team on all of the five English subtasks, this paper described the Twitter sentiment classifier using Convolutional Neural Networks (CNNs) and Long Short Term Memory (LSTMs) networks.
    - They leveraged a large amount of unlabeled data to pre-train word embeddings. They then use a subset of the unlabeled data to fine tune the embeddings using distant supervision.
    - To boost performances they ensembled several CNNs and LSTMs together.
    - They got 0.685 average F1 score.
  
- [Impact of COVID-19 pandemic on grocery shopping behaviours](https://assets.publishing.service.gov.uk/government/uploads/system/uploads/attachment_data/file/932350/Grocery_Purchasing_Report.pdf)
    
## *Evaluation:*
We will use `f1` and `recall` score to evaluate our model and compare other similar peer works.

## *Conclusion (optional):*

The goal of our project is to generate a tweet sentiment classifer trained by ML neural networks. And we could use our model to analyze the different attitude on COVID19 relevant term (such as "mask", "vaccine", "travel") across different countries and different period. We would present our analysis both on location and date dimension at the end of this project.

## Challenges

### Data

#### Inference Data

As discussed in the data section above, for the inreference data, we need to start with a dehydrated dataset that enables us to filter by country and by language, while still yielding enough tweet_ids (i.e. data) for us to later feed into the model.

We first tried UBC-NLP's [Megacov](https://github.com/UBC-NLP/megacov) but it was unlikely to get us enough data after filtering by country and by language.

In the case of May 2020:

| Filter                                  | # of tweets |   |
|-----------------------------------------|------------------|---|
| None                                    | 10,137,401       |   |
| Country == Canada                       | 7,660            |   |
| Country == Canada & Language == English | 292              |   |

Given the Megacov May 2020 data covers May 1 to May 15 (half a month), if we scale up to Feb 1 to May 15 (3.5 months), we probably can get:

| Filter                                  | # of tweets |   |
|-----------------------------------------|------------------|---|
| Country == Canada                       | 53k            |   |
| Country == Canada & Language == English | 1k              |   | `` tweets (filter: Country == Canada.

We doubt this will be sufficient as our target data size was 100k per country per keyword, which is double the size we can get now. Plus we have yet to remove retweets and other noisy data, let alone filtering by keyword/hashtag, which was our original plan but will reduce the size of the inference data. 

To overcome this challenge, we can think of 2 approaches going forward:

1. Try other datasets to see if we can get more tweet_ids that meet our standards;

2. Loosen up the restrictions. For example, instead of studying "mask/vaccine/travel", we look into the general sentiment of all tweets in the dataset.

