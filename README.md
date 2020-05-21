# Disaggregation-of-Yelp-Review-Ratings

## Abstract
As a crowd-sourced business review and social networking website, Yelp has hundreds of millions of reviews. However, it only provides a holistic view by giving review ratings. In this project, we separated overall ratings by different categories to gain an insight into which categories will influence the stars most. For example, a 4 star review: “Great place to hang out after work: the prices are decent, and the ambience is fun. It's a bit loud, but very lively. The staff is friendly, and the food is good. They have a good selection of drinks.” This example includes different topics such as environment, service, food and price. By generating the topics and sentiment intensity score from the user’s review, we can use linear regression to generate coefficients of each topics and understand the relationship between stars and topics.

## Proposed Approach
### 1. Topic Extraction
Topic extraction involves two steps, Term Frequency- Inverse Document Frequency (TF-IDF) and Non-Negative Matrix Factorization (NMF). We first feed in the corpus of all the reviews with each row being a tokenized sentence. TF-IDF Vectorizer generates a document feature vector for every tokenized sentence. Then using NMF, we are able to reduce the dimension of the TF-IDF and group the TF-IDF features under different topics. Using groupings of TF-IDF features, we then can come up with a general topic that encapsulates all of its TF-IDF features.
![alt text](https://github.com/Zelong-Chen/Disaggregation-of-Yelp-Review-Ratings/blob/master/images/topic_extraction.PNG)

### 2. Topic Labeling
Using the NMF vectors that are corresponding to each tokenized sentence of the reviews, we can obtain the relevance scores of that sentence to the topics extracted by the NMF. Thus, each tokenized sentenced is labeled with the topic with the highest score.
![alt text](https://github.com/Zelong-Chen/Disaggregation-of-Yelp-Review-Ratings/blob/master/images/topic_labeling.PNG)

### 3. Sentiment Analysis
After labeling every sentence, we then use VADER Sentiment Analysis to generate a sentiment intensity score of [-1,1] for that sentence. Then we aggregate the sentiment intensity scores of each user by averaging the intensity score under each topic.
![alt text](https://github.com/Zelong-Chen/Disaggregation-of-Yelp-Review-Ratings/blob/master/images/sentiment_analysis.PNG)

### 4. Linear Regression + Evaluation
With the inputs of each user and their sentiment intensity score of each topic, we then fit a linear regression model to generate the coefficients of each topic that explains the relationship between stars and that particular topic. We mainly use Mean Squared Error (MSE) and Akaike Information Criterion (AIC) to evaluate our models. Then through cross validation of several different linear regression models such as linear regression, polynomial, ridge, lasso, we can confidently provide the model that have the lowest MSE and AIC scores. Then using the best model, we apply it to our test data and evaluate our final model with MSE and AIC scores.
![alt text](https://github.com/Zelong-Chen/Disaggregation-of-Yelp-Review-Ratings/blob/master/images/linear_reg.PNG)
Note: By removing the intercept and uncleared topics we are trading off between Model Accuracy vs Model Interprebility
