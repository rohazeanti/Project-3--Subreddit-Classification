# Project 3: Subreddit Classification

#### _Rohazeanti Mohamad Jenpire_

### Problem Statement
Develop a classification model that can distinguish which of two subreddits, (r/diet or r/exercise), a particular post belongs to.

Diet and exercise are both essential for optimal health. And people often incorporate diet (for the purpose of living a healthy life) with exercise in their daily lives. We also often associate the word diet with unpleasant weight-loss regimen?  For example, consider the use of the term "diet" in marketing food products—it usually describes foods low in calories, such as diet soda.But there is another meaning of this word. Diet can also refer to the food and drink a person consumes daily and the mental and physical circumstances connected to eating. Nutrition involves more than simply eating a “good” diet—it is about nourishment on every level. 

Exercise and diet is associated with weight loss, losing weight, reduce fat, burn calorie(food or type of exercises). There are posts that are diet related are posted in r/exercise and vice versa. 

As the data scientist on the team, I have been assigned the task of building a classificiation model that can accurately classify posts from each subreddit so that r/diet and r/exercise can be free of unrelated posts so that the forum can continue to offer accurate content. 

### Executive Summary
Data was collected from r/diet and r/exercise on August 1, 2021 using the Pushift Reddit API. In total, 3000 posts from r/LifeProTips were collected, and 3000 posts from r/UnethicalLifeProTips were collected. Posts were aggreggated into a single data frame and data was cleaned according to standard protocol (handling null values, confirming data types, and searching for any abnormal values). Because text data can contain many symbols and phrases that do not add value to a sentence, any symbols or tags (e.g. '[request]') were removed to the best of my abilities. Basic features were engineered to allow for examination of full text in the post, post length in characters and post length in words. Many posts were deleted or removed by moderators but title remained. Hence, I combined both title and selftext to create a new feature called title_text. 

Once the data had been clean, text preprocessing was conducted to expand contractions in the text, and posts were lemmetized. In order to be compatible with the text vectorizers, the lemmetized words were joined as a string. At this point, the data frame was considered ready for exploratory data analysis and modeling. The data dictionary can be found below.

During EDA, distributions for all numeric features were explored, the most frequent word counts were generated per subreddit. 

Following EDA, the data was prepared for modeling. Tfidf Vectorizer and Count Vectorizer were used in all the model. A 70/30 train test split was conducted, and a column transformer with a Tfidf Vectorizer was created to vectorizer the stemmed text, while also maintaining the original form of any numeric features.

During the modeling process, a pipeline was created. Pipelines contained the column transformer (Tfidf Vectorizer/Count Vectorizer) and a classification model. GridSearchCV was used to tune hyperparameters, and model performance was evaluated using the accuracy metric. The models built were:

Logistic Regression
Multinomial Bayes
Random Forest

Summary of Classification Results

|Model|Train Accuracy|Test Accuracy|Generalisation|ROC AUC|True Positive|True Negative|False Positive|False Negative|Percision|Recall|
|---|---|---|---|---|---|---|---|---|---|---|
|TVEC/MultinomialNB|0.93|0.898|3.2%|0.97|775|793|50|128|0.939|0.858|
|CVEC/MultinomialNB|0.928|0.907|2.1%|0.97|800|783|60|103|0.93|0.886|
|TVEC/RandomForestClassifier|0.997|0.88|11.7%|0.95|776|760|83|127|0.903|0.859|
|CVEC/RandomForestClassifier|0.996|0.878|11.8%|0.95|774|759|84|129|0.902|0.857|
|TVEC/LogReg|0.95|0.913|3.1%|0.97|829|765|78|74|0.914|0.918|
|CVEC/LogReg|0.971|0.901|7%|0.96|836|738|105|67|0.888|0.926|

Based on test accuracy and AUC ROC scores, it appears that CVEC/MultinomialNB model performed the out of the six. The model is not as overfit as the rest.

In this repository my work flow will be described, and I will select and evaluate a model to classify by posts from the r/diet and r/exercise subreddits.


### Data Sources
For this project, these are the datasets used:

[diet](diet.csv)

[exercise](exercise.csv)

### Data Dictionary
The clean, preprocessed data used for EDA and modeling can be found [here](df_combined.csv).


|Feature|Data Type|Description|
|---|---|---|
|subreddit|String|Source of data|
|title_text_cleaned|String|Source of data|
|title_text_lemm|String|Source of data|
|title_text_lemm_with_stopwords|String|Source of data|
|num_char|int|Number of characters|
|num_words|int|Number of words|
|num_vocab|int|Number of vocabulary|
|lexical_div|int|Total number of words to the num of unique words|
|ave_word_length|int|average word length by diving number if characters by the number of words|


### Conclusion & Recommendations
Conclusion:

- For this problem, I was tasked with building a classification model that could accurately distinguish posts from the r/diet from the r/exercise Subreddit. The best-performing model was a CVEC Naive Bayes with an accuracy of 90.7% and a sensitivity of 88.6%. This model is good at accurately predicting posts from the r/exercise. However, we should try to improve the overall accuracy score.

Recommendation:
- To begin the next round of improvements, we will learn from our models and manually prune features from the vector of words. Once the dimensionality of the models has been reduced, we will be able to tune our models in hopes of improving the overall accuracy.

- Beyond easing the burden of moderators by giving them the ability to classify posts from two different subreddits based on their title and selftext, there are several other possible applications for this model.

- By looking at the probabilities associated with each post, moderators can also understand the overall direction of their subreddit. It's often hard to trace the evolution of subreddits over time, however, by looking at the posts that have an extremely high classification probability (>0.99), moderators can see the language and topics that have become characteristic or emblematic of their community.

- Depending on their objectives, moderators can then try to increase the diversity of topics within their subreddit, or try to attempt to refocus conversations that are straying from the vision and objectives of the subreddit.