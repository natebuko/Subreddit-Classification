# The Future, The Past & Natural Language Processing


#### Nate Bukowski


## Problem Statement


Using comments and submissions from the `r/history` and `r/Futurology` subreddits, can we build a model that can tell the difference between a conversation about the future and a conversation about the past?


## Repo Contents


The [data](./data) folder contains the final data from the scrape in the form of the [reddit_data.csv](./data/reddit_data.csv) file.


The [code](./code) folder contains two python notebooks.
- [data-collection.ipynb](./code/data-collection.ipynb) details the data collection, cleaning & feature engineering involved in the project.
- [modeling.ipynb](./code/modeling.ipynb) deltails the modeling process and results of the project.


## Data


Using [Pushshift's API](https://github.com/pushshift/api), 10,000 comments and 10,000 submissions were collected from each subreddit for a total of 40,000 rows of data.


## Executive Summary


### 1. Data Collection, Cleaning & Feature Engineering
Using the Pushshift API, language data was collected from two subreddits: `r/history` and `r/Futurology`. Scraping 10,000 comments and 10,000 submissions from each subreddit totaled 40,000 rows of data when merged together into one final dataframe. In order to get the data ready for modeling I did a little bit of cleaning. Each 10,000 row dataframe only needed certain columns depending on the category of the post; the comment dataframes used the `author`, `body` and `subreddit` columns, while the submission dataframes used the `author`, `title` and `subreddit` columns. So that the individual dataframes could be effectively combined, the `body` and `title` columns were both renamed `text`. Once combined, the target `subreddit` column was converted into binary values that our models could use for prediction; 1 for Futurology and 0 for history. The last step was to clear all deleted and removed posts from the dataframe before converting it to a `.csv` file.


### 2 . Modeling
Outside of the Base Model I fit 8 total models: 2 Logistic Regression, 2 Naive Bayes, 2 k-Nearest Neighbors and 2 Support Vector Machines. All models used a pipeline and gridsearch outside of the Gaussian Naive Bayes Model and the Support Vector Machines. The answer to the problem statement only required the accuracy measure on each model, so accuracy was the only measurement taken. The Base Model was set at 50% accuracy.

#### Logistic Regression:
- **Hyperparameters: Count Vectorizer & Tfidf-Vectorizer**
    - Max Feature Limit: None, 5,000, 10,000
    - With & without stopwords
    - 1 & 2 n-gram range
- **Best Count Vectorized Model**
    - No max feature limit
    - No stopwords
    - 2 n-gram range
    - Training Accuracy:  0.8942
    - Testing Accuracy: 0.8976
- **Best Tfidf-Vectorized Model**
    - No max feature limit
    - With stopwords
    - 1 gram range
    - Training Accuracy:  0.8963
    - Testing Accuracy: 0.8975

#### Naive Bayes:
- **Multinomial Hyperparameters**
    - Max Feature Limit: None, 5,000, 10,000
    - With & without stopwords
    - 1 & 2 n-gram range
- **Best Multinomial Model**
    - No max feature limit
    - No stopwords
    - 2 n-gram range
    - Training Accuracy:  0.9081
    - Testing Accuracy: 0.9081
- **Best Gaussian Model**
    - Training Accuracy:  0.87
    - Testing Accuracy: 0.7549

#### k-Nearest Neighbors:
- **Hyperparameters: Count Vectorizer & Tfidf-Vectorizer**
    - 5, 15 & 25 nearest neighbors
- **Best Count Vectorized Model**
    - 5 nearest neighbors
    - Training Accuracy:  0.6658
    - Testing Accuracy: 0.66781
- **Best Tfidf-Vectorized Model**
    - 25 nearest neighbors
    - Training Accuracy:  0.8141
    - Testing Accuracy: 0.7569

#### Support Vector Machines:
- **Count Vectorized Model**
    - Training Accuracy:  0.9192
    - Testing Accuracy: 0.8699
- **Tfidf-Vectorized Model**
    - Training Accuracy:  0.9888
    - Testing Accuracy: 0.9023
    

### 3. Conclusions & Next Steps

Lets refer back to the **problem statement**: Using comments and submissions from the `r/history` and `r/Futurology` subreddits, can we build a model that can tell the difference between a conversation about the future and a conversation about the past? Considering that our two best models (the Multinomial Naive Bayes and the Tfidf-Vectorized Support Vector Machine) both scored above 90% accuracy on the testing data and the base model was only 50% accuracy, there is enough evidence to confidently say that we have built a model that can accurately predict whether the topic of conversation is the future or the past.

Given more time these models can be refined and more models (like a Random Forest) can be fit. Moving forward I will be running all models through a pipeline and adjusting the hyperparameters.
