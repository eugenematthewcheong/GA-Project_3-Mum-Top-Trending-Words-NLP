<img src="http://imgur.com/1ZcRyrc.png" style="float: left; margin: 20px; height: 55px">

# DSI-SG-42
## Project 3: Web APIs & NLP
> Authors: Pius Yee, Conrad Aw, Eugene Matthew Cheong
---

## Contents
---
- [Executive Summary](#executive-summary)
- [Problem Statement](#problem-statement)
- [Data Collection](#data-collection)
- [Data Cleaning, Preprocessing and EDA](#Data Cleaning,-Preprocessing-and-EDA)
- [Modeling](#Preprocessing-and-Modeling)
- [Conclusion and Recommendations](#Conclusion-and-Recommendations)

## Executive Summary
---
In today's fast-paced world, the role of a mother is more challenging and multifaceted than ever before. Mothers are not only caregivers but also significant contributors to the workforce. This is especially true in Singapore, where the workforce is the main driver of the economy. As the nation progresses, the competing demands of work and family life have placed immense pressure on working mothers, making it a topic of great importance and ongoing debate.

Recent discussions have highlighted the need for more support for working mothers, despite the government's efforts to improve support systems. The challenges faced by working mothers in Singapore are unique due to the high expectations of productivity and efficiency in both professional and personal spheres. These challenges often lead to a struggle to maintain a healthy work-life balance, which can impact their well-being and that of their families.

News Articles:
- [Channel News Asia - Working Mum challenges](https://www.channelnewsasia.com/commentary/working-mums-challenges-best-tips-juggle-home-work-family-1373851)

- [Channel News Asia - More support for middle class working mothers](https://www.channelnewsasia.com/singapore/budget-2023-debate-working-mothers-middle-class-cpf-ceiling-jobs-skills-integrators-3295851)

- [What can government and employers do to support mothers](https://www.nytimes.com/2021/02/04/parenting/government-employer-support-moms.html)

## Problem Statement
---
In light of the current generation's high engagement on social media and forums, particularly in discussions related to work-life balance and childcare, our Campaign Strategists face challenges in consistently identifying and categorizing relevant topics for our campaigns. This reliance on personal assumptions rather than empirical evidence hampers the effectiveness of our campaigns or programs, hindering our ability to optimize online presence and effectively reach our target audience.

To address this challenge, we propose developing a classification model to accurately classify posts and forums relevant to mothers, thereby enhancing our campaign targeting and outreach strategies.

## Data Dictionary
---
For raw data file 'daddit_mommit_df.csv':

|Feature|Type|Dataset|Description|
|---|---|---|---|
|subreddit|object|daddit_mommit_df.csv|Subreddit Source Heading|
|title|object|daddit_mommit_df.csv|Title of post|
|selftext|object|daddit_mommit_df.csv|Post Body text|
|score|int64|daddit_mommit_df.csv|Number of votes|
|url|object|daddit_mommit_df.csv|URL to the subreddit post|
|comments|object|daddit_mommit_df.csv|List of comments and replies in the post|

For cleaned and preprocessed data file 'final_df.csv':

|Feature|Type|Dataset|Description|
|---|---|---|---|
|category|object|final_df.csv|Type of text structure|
|text|object|final_df.csv|List of words after data cleaning and tokenizing|
|mum|int64|final_df.csv|Binary value of whether the text list is classifed as written by mothers.|

## Data Collection
---
### Criterias for selection of subreddit

1) **Relevance**: The discussions and subjects in the subreddit must be relevant to Mothers and Fathers.

2) **Activity & Engagement Level**: The subreddit needs to be active and engaging based on the number of posts and comments as higher activity levels may provide more interesting data for analysis.

3) **Size**: The member size of the subreddit is important as larger subreddits may have more diverse content and discussions.

4) **Moderation**: Strong moderation preferred to ensure fewer spam or irrelevant posts.

5) **Quality of Content**: Quality of content based on post and comments level of detail and informativeness.

6) **Variety**: Consider selecting a mix of subreddits to get a diverse range of content and perspectives.


Based on the above, we have selected subreddits 1) r/Mummit and 2) r/daddit for data collection.

Both r/daddit and r/Mommit are relevant as they are communities for fathers and mothers with a membership size of 1.5 million and 1.1 million respectively. Both fulfill the content quantity and quality criterias with high number of posts & comments, and detailed descriptions. In addition, moderation is deemed as sufficient with the rules in place for each subreddit.

### Collection Method

A brief description of the data collection method:
- Register to use Reddit's official API 
- Identified the following features for text scraping: subreddit name,title of post, post body text, number of votes, URL to subreddit post and comments.
- Extraction of identified features from all posts and comments in the subreddits.
- Export the dataframe with consolidated data from both subreddits.

## Data Cleaning, Preprocessing and EDA
---
For Data Cleaning, the following tasks were performed:
- Identified and removed punctuations, special characters, system auto-replies, blank rows, missing values and other irrelevant texts.

For Preprocessing, the following tasks were performed:
- Tokenization: Breaking sentences into individual words.
- Stop Word Removal: Removed high frequency words with low semantic value.
- Lemmatization: Convert inflected forms of a word to its base form

For EDA, the following metrics were evaluated:
- Length of comments: No distict difference between the two classes
- Sentiment Analysis: Both classes showed a common trend with majority Positive sentiments
- N-gram Analysis: 1-gram, 2-gram, 3-gram and 4-gram was conducted with more differences observed for 3-gram and 4-gram.

## Modeling
---
Models used for tuning and selection
- Multinomial Naive Bayes
- Logistics Regression

Text vectorizers used:
- CountVectorizer
- TF-IDF vectorizer
    
## Model scores
---
|   |                     Model |      Vectorizer |                                                                                      Best parameter | Average F1 score with 5 folds | F1 score on train | F1 score on test | Sensitivity | Precision | Specificity | Accuracy |
|--:|--------------------------:|----------------:|----------------------------------------------------------------------------------------------------:|------------------------------:|------------------:|-----------------:|------------:|----------:|------------:|---------:|
| 1 | Naive Bayes - Multinomial | CountVectorizer |   {'cvec__max_df': 0.3, 'cvec__max_features': 9000, 'cvec__min_df': 2, 'cvec__ngram_range': (1, 1)} |                      0.720852 |          0.764590 |         0.731145 |    0.708710 |  0.755047 |    0.854013 | 0.797584 |
| 2 | Naive Bayes - Multinomial | TfidfVectorizer |   {'tvec__max_df': 0.2, 'tvec__max_features': 9000, 'tvec__min_df': 2, 'tvec__ngram_range': (1, 2)} |                      0.629599 |          0.700490 |         0.640493 |    0.514423 |  0.848414 |    0.941641 | 0.775728 |
| 3 |      Logistics Regression | CountVectorizer | {'cvec__max_df': 0.3, 'cvec__max_features': 11000, 'cvec__min_df': 2, 'cvec__ngram_range': (1, 1... |                      0.710769 |          0.844581 |         0.710260 |    0.655826 |  0.774549 |    0.878793 | 0.792202 |
| 4 |      Logistics Regression | TfidfVectorizer | {'lr__C': 1, 'lr__penalty': 'l2', 'lr__solver': 'liblinear', 'tvec__max_df': 0.3, 'tvec__max_fea... |                      0.710110 |          0.780063 |         0.715776 |    0.649887 |  0.796534 |    0.894595 | 0.799561 |

**Model Selection: Multinomial Naïve Bayes with CountVectorizer**

Multinomial Naïve Bayes with CountVectorizer have the highest score hence we have selected it as our model.

---
|   |                     Model |      Vectorizer |                                                                                      Best parameter | Average F1 score with 5 folds | F1 score on train | F1 score on test | Sensitivity | Precision | Specificity | Accuracy |
|--:|--------------------------:|----------------:|----------------------------------------------------------------------------------------------------:|------------------------------:|------------------:|-----------------:|------------:|----------:|------------:|---------:|
|  | Naive Bayes - Multinomial | CountVectorizer |   {'cvec__max_df': 0.3, 'cvec__max_features': 9000, 'cvec__min_df': 2, 'cvec__ngram_range': (1, 1)} |                      0.720852 |          0.764590 |         0.731145 |    0.708710 |  0.755047 |    0.854013 | 0.797584 |


---
## Limitations

- Unable to identify localized words
The model was trained on data from the global subreddits r/daddit and r/Mummit.

 - Performance of the model is constrained by limitations in the Naive Bayes and Logistics Regression approach.
Exhaustive tuning did not yield substantial improvements.


---
## Recommendations

- **With more resources:**
We can acquire more localized data from either from other social media platforms or forums.
For example: Picking up Singlish.

- **Use more complex models such as Random Forest or XGBoost.**
To have more accurate classification

---
## Conclusions
##### Our model (Multinomial Naive Bayes with CountVectorizer) demonstrates strong performance in classifying posts from subreddits to identify whether they were written by a mum or dad. However, there's room for further improvement using the following ideas: 

- We can acquire more localized data from either from other social media platforms or forums.

- Use more complex models such as Random Forest or XGBoost to have more accurate classification.

### Files

**Code**
- 1.0 Web Scraping.ipynb   
- 2.0 Data Cleaning & Preprocessing.ipynb   
- 3.0 Modelling.ipynb
- 4.0 Model Test.ipynb  

**Datasets**
- daddit_mommit_df.csv
- final_df.csv
- final_model.pkl
- final_tokenized_df.pkl
- parenting_df.pkl


Group 1 - Project 3.pdf

README.md

---