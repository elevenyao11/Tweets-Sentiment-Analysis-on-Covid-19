Project Progress Report
---
link to project repo:
[https://github.ubc.ca/lisa67/CL585-COVID19-Sentiment-Analysis-Team](https://github.ubc.ca/lisa67/CL585-COVID19-Sentiment-Analysis-Team)

Team Members:

Yundong Yao,
Quang Vu,
Lisa Liu,
Alex Chen.


## Progress this week:

### Data

- Updated the data-wrangling notebook [(link)](https://github.ubc.ca/lisa67/CL585-COVID19-Sentiment-Analysis-Team/blob/master/milestone3/data-wrangling/data-wrangling-proof-of-concept.ipynb) by:
	- converting csv to tsv;
	- automating processing .json files in batch; 
	- leaving room for further pre-processing;
 

- Extended the scope to include more countries and longer periods. We have yet to decide the exact window (in time and in geography). But currently we collected hydrated tweets, based on tweet_ids from MegaCov, that meeting the following criteria:
   -  Size: 675187 tweets after preprocessing, 720802 before preprocessing. 	
   -  Time: Jan, Feb, Mar, Apr, May of 2020
   -  Language: English
   -  Countries:
   
	   	-  Americas: 
		   	- Canada
			- United States
			- Mexico
		
		-  Oceania: 
			-  Australia
			-  New Zealand
		
		-  Europe: 
			-  United Kingdom
			-  Germany
			- France
 
### Data Preprocessing

- Here the data preprocessing pipeline on inference data:
	- Combined data from differnt months.
	- Filtered out the invalid tweet without datatime or country information.
	- Replaced the all user IDs in tweet text as "@user".
	- Replaced the all url in tweet text as "\<url\>".
	- Stored the information we are interested in. (tweetid, full text, datetime, country)

### Model

After fine-tuning with the TWEETBert model on the official splits of the sentiment analysis dataset from TweetEval, we now have got  0.7363 F1 score.

### Sentiment inference

We implemented our best model -- BERTweet on the processed tweets, the output includes inferred sentiment and probability after softmax. 


---
Below is an up-to-date report in its original format:

## *Introduction:* 
In this project, we intend to answer the following question:
> Did English-speaking people from different regions show different sentiments at a particular stage of the pandemic?

To do this, we use a pre-trained model to apply Sentiment Analysis to tweets that are:

- in English language; 
- from January to May 2020;
- from 8 countries in different regions of the world:
	-  Americas: 
	   	- Canada
		- United States
		- Mexico
		
	-  Oceania: 
		-  Australia
		-  New Zealand
		
	-  Europe: 
		-  United Kingdom
		-  Germany
		-  France

## *Motivation:*
We as practitioners think this question is important due to its relevance in: 

- commerce:  
	The pandemic reshapes both demand and supply. Trans-national companies would be interested to learn how the public in different regions are reacting. This insight would help them to better adapt to the disruption to their global supply chain.
	
- policy:  
  Policy-makers, pollsters, and the press may be interested to feel the "pulse" of public opinion, domestic and foreign. This knowledge may make more informed policy-making possible.
  
- society:  
  Online echo chambers may make people feel either more anxious or more unconcerned than they should be. It might help to put into a broader context their attitudes in general.


## *Data:*
### Training Dataset:

- source: [SemEval-2017 Task A](https://alt.qcri.org/semeval2017/task4/index.php?id=data-and-tools)


- size: train:43860, dev:1999, test:2390 tweets labeled by polarity(positive, neutral, negative).

### Inference Dataset:

Our plan to get inference dataset is as follows:

0. Pick a dehydrated dataset 
1. Get tweetIDs by country, e.g. Canada
2. Use `twarc` and Twitter API to hydrate tweets (Proof-of-concept [here](https://github.ubc.ca/lisa67/CL585-COVID19-Sentiment-Analysis-Team/blob/master/milestone3/data-wrangling/data-wrangling-proof-of-concept.ipynb)
3. Anonymize tweets by replacing urls with <url> token
4. Further pre-process data by:
    - replacing emoticons by text such as <smile>;
    - truncating any letter repeating more than 2 times in a row (e.g.  sooooo -> soo).

Our preliminary attempts on twitter's developer API suggests that using a single token may get us 200k tweets per 24 hours. After that we will hit a rate limit and have to wait. But we tried parallel processing by using 2 token APIs at the same time and managed to get the 3.5 months' worth of data in week 3.
 
In terms of data storage, we intend to keep all fields of metadata that we received from Twarc for now, in case our final model needs input from them.

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

### get enough inference data 

We would like to get a sufficient amount of data seems for our models. But we are inevitably limited in our resources, in terms of:
	- which datasets to retrieve more tweet ids from;
	- how many tweet_ids of interest we can get from the datasets, given limited memory and computing power;
	- the rate limit imposed by Twitter when we hydrate.

To overcome these and to get a decent amount of valid inference data after pre-processing, we might need to take one, or both, of the 2 routes below:

1. find more data;
2. adjust our research scope;  
	for example, we might not have the privilege to analyse the data in greater granularity, such as at the keyword level, like we proposed. 
