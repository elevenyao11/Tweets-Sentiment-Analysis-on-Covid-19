# Milestone 1 feedback:

Thanks for your project proposal. 
The proposal is well-organized and readable. I would like to provide some comments:
1. Do you have a Twitter API to crawl the tweet contents by IDs? Have you got any tweets? 
2. How do you pre-process the tweets? Do you use the same pipeline to pre-process your labelled training data and unlabeled inference data?
3. Please carefully describe your data pre-processing in your final paper. Twitter data is very noisy. You should anonymize the user information and standardize tweets (e.g., removing URL). 
4. How can you retrieve the inference datasets? For example, how can you get tweets corresponding to the term "Vaccine"? Keywords searching or hashtag searching? 
5. For the temporal analysis, will you consider several important time points? (such as lock-down, the wave of the pandemic, and social event)
6. You should think about your evaluation metrics. The macro-F1 score is good. But the official metric of SemEval-2017 is average recall. For comparison, you should also report this metric. 
7. As I suggested, you may directly use BERTweet rather than the regular BERT or RoBERTa.
8. Please think about hyper-parameter tuning for your models. You may need to search for the best hyper-parameters in each model. 
9. When you build a CNN model, you should think about how to create a vocabulary and how to initialize the word embedding layer. 
