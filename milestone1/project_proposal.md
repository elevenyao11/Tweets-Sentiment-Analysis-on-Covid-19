Project proposal
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

- about terms, i.e. containing a number (TBD) of the following keywords:

   - Quarantine
   - Travel
   - Vaccine
   - Bleach
   - Gloves
   - Gowns
   - Face shields
   - Disinfectant cleaner
   - Industrial cleaner
   - Hand sanitizer
   - Masks
   - Wipes, 



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


## *Contribution:*

Our contribution would be an implementation of a system that delivers insights on a new topic (i.e., sentiment in tweets in different location as to COVID-19).

## *Originality:*

We applied similar techniques to state-by-state data previously and discussed the possibility of analysing city-by-city data. But this is our first attempt to scale it up to the country-by-country level.

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
### computing infrastructure:

We will first try with Personal computers only. We will consider using Google Colab and/or Google Cloud TPU if it is needed.

### deep learning of NLP method:

We would chose one of the models below with best f1 score in the traing prhase to do the classification on Covid-19 relevant tweets.
- CNN: We have done some pilot research on CNN, based on the training data, we got 0.37 f1 score by now.
- BERT-Classfication
- RoBERTa with Adapter Module
- DANN (Domain Adversarial Neural Network)
- linear regression (baseline)


###  Framework:
PyTorch (required). 

### existing codebase:
- [COLX585 Models tutorial](https://github.ubc.ca/MDS-CL-2020-21/COLX_585_trends_students/tree/master/tutorials)
- [COLX523 tweepy tutorial](https://github.ubc.ca/MDS-CL-2020-21/COLX_523_adv-corp-ling_students/blob/master/blank_lectures/Twitter_tutorial.ipynb)


[add more here, with links]

## *Previous Works (minimal):*
  -  H. Manguri, K., N. Ramadhan, R. and R. Mohammed Amin, P. (2020) “Twitter Sentiment Analysis on Worldwide COVID-19 Outbreaks”, Kurdistan Journal of Applied Research, 5(3), pp. 54-65.

  - Seabold,Skipper & Rutherford,Alex & De Backer,Olivia & Coppola,Andrea, 2015. "The pulse of public opinion : using Twitter data to analyze public perception of reform in El Salvador," Policy Research Working Paper Series 7399, The World Bank.
  - [Impact of COVID-19 pandemic on grocery shopping behaviours](https://assets.publishing.service.gov.uk/government/uploads/system/uploads/attachment_data/file/932350/Grocery_Purchasing_Report.pdf)
## *Evaluation:*
We will use `f1` score to evaluate our model.

## *Conclusion (optional):*
> You can have a very brief conclusion just summarizing the goal of the proposal. (2-3 sentences max).

The goal of our proposal is to generate a tweet sentiment clssifer traind by ML neural networks. And we could use our model to analyze the different attitude on COVID19 relevant term (such as "mask", "vaccine", "Travel") across differnet countries and different period. We would present our analysis both on location and date dimension at the end of this project.
