# sentiment_analysis


## OVERVIEW:

Sentiment Analysis refers to the use of natural language processing,text analysis,computational linguistics, andbiometricsto systematically identify, extract, quantify, and study affective states and subjective information.

## TASK
The objective of this task is to detect hate speech in tweets. For the sake of simplicity, we say a tweet contains hate speech if it has a racist or sexist sentiment associated with it. So, the task is to classify racist or sexist tweets from other tweets.


Formally, given a training sample of tweets and labels, where label '1' denotes the tweet is racist/sexist and label '0' denotes the tweet is not racist/sexist, your objective is to predict the labels on the test dataset.


## DATASET

First take a look at the data.Dataset has 31962 entries, with no null entries.. where 29720 entries of the data with label 0(non-racist), and 2242 entries of the data with label 1 (racist).it clearly shows how our data is highly imbalanced.

Imbalanced Corpus or Dataset was always a challenge for supervised machine learning fellows. Imbalance means that not all classes in the dataset has the same size, there could be a bias to one or more classes (majority) over other less presented classes (minority).


## DATA CLEANING:
I first define data cleaning function, and then it will be applied to the whole dataset. Tokenization, stemming/lemmatization, stop words will be dealt with later stage when creating matrix with either count vectorizer or Tfidf vectorizer.

The order of the cleaning is

  -Souping

  -BOM removing

  -url address(‘http:’pattern), twitter ID removing

  -url address(‘www.'pattern) removing

  -lower-case

  -negation handling

  -removing numbers and special characters

  -tokenizing and joining


## Story Generation and Visualization from Tweets:

A) Understanding the common words used in the tweets: WordCloud

    To see how well the given sentiments are distributed across the train dataset. One way to accomplish this task is by understanding the common words by plotting wordclouds.We can see most of the words are positive or neutral. Words like love, great, friend, life are the most frequent ones. It doesn’t give us any idea about the words associated with the racist/sexist tweets. Hence, we will plot separate wordclouds for both the classes (racist/sexist or not) in our train data.


B) Words in non racist/sexist tweets

    Most of the frequent words are compatible with the sentiment, i.e, non-racist/sexists tweets. Similarly, we will plot the word cloud for the other sentiment. Expect to see negative, racist, and sexist terms.

C) Racist/Sexist Tweets

      As we can clearly see, most of the words have negative connotations. So, it seems we have a pretty good text data to work on. Next we will the hashtags/trends in our twitter data.
      

D) Understanding the impact of Hashtags on tweets sentiment

      we have prepared our lists of hashtags for both the sentiments, we can plot the top 'n' hashtags. So, first let’s check the hashtags in the non-racist/sexist tweets.
      
## Zipf’s Law
Interesting fact is that “given some corpus of natural language utterances, the frequency of any word is inversely proportional to its rank in the frequency table. Thus the most frequent word will occur approximately twice as often as the second most frequent word, three times as often as the third most frequent word, etc."


Most of the words are below 100 on X-axis and 1000 on Y-axis, and we cannot see meaningful relations between negative and positive frequency.

Explored what we  get out of frequency of each token. Intuitively, if a word appears more often in one class compared to another, this can be a good measure of how much the word is meaningful to characterise the class. In the below code I named it as ‘pos_rate'.

Words with highest pos_rate have zero frequency in the negative tweets, but overall frequency of these words are too low to consider it as a guideline for positive tweets.


## ADDING NEW METRICS:
Another metric is the frequency a word occurs in the class.But since pos_freq_pct is just the frequency scaled over the total sum of the frequency, the rank of pos_freq_pct is exactly same as just the positive frequency.

What we do now is to combine pos_rate, pos_freq_pct together to come up with a metric which reflects both pos_rate and pos_freq_pct. Even though both of these can take a value ranging from 0 to 1, pos_rate has much wider range actually spanning from 0 to 1, while all the pos_freq_pct values are squashed within the range smaller than 0.015. If we average these two numbers, pos_rate will be too dominant, and will not reflect both metrics effectively.so lets use haronic mean.

What we try next is to get the CDF (Cumulative Distribution Function) value of both pos_rate and pos_freq_pct.we calculate a harmonic mean of these two CDF values, as we did earlier. By calculating the harmonic mean, we can see that pos_normcdf_hmean metric provides a more meaningful measure of how important a word is within the class.

It seems like the harmonic mean of rate CDF and frequency CDF has created an interesting pattern on the plot. If a data point is near to the upper left corner, it is more positive, and if it is closer to the bottom right corner, it is more negative. So I took an alternative method of an interactive plot with Bokeh. Bokeh is an interactive visualisation library for Python, which creates graphics in style of D3.js.

## MODEL SELECTION:

As I have already realised, the training data is not perfectly balanced

The simple default classifier I’ll use to compare performances of different datasets will be the logistic regression. From my previous sentiment analysis project, I learned that Tf-Idf with Logistic Regression is a pretty powerful combination. Before I apply any other more complex models such as ANN, CNN, RNN etc, the performances with logistic regression will hopefully give me a good idea of which data sampling methods I should choose.


With data as it is without any resampling, we can see that the precision is higher than the recall.

This means the classifier is very picky and does not think many things are negative. All the text it classifies as negative is 61~65% of the time really negative. However, it also misses a lot of actual negative class, because it is so very picky. We have a low recall, but a very high precision.

### SMOTE (Synthetic Minority Over-Sampling Technique)
SMOTE is an over-sampling approach in which the minority class is over-sampled by creating “synthetic” examples rather than by over-sampling with replacement.

The way to look at it is to look at the f1 score, which is the harmonic average of precision and recall. The original imbalanced data had 94.53% accuracy and 67.63% F1 score. However with oversampling, we get a slightly higher accuracy of 95%, but a much higher F1 score of 82.69%

# Using Word Embeddings for Data Augmentation
I will focus on augmenting texts labelled as 1, as this class is under-represented. Oversampling can help improve perfomances.

## Step 1 : Loading Word Embeddings

## Step 2: Tokenizing
I am using Keras' Tokenizer to apply some text processing and to limit the size of the vocabulary

## Step 3 : Making a Synonym Dictionary
Word vectors are made in a way that similar words have similar representation. Therefore we can use the k -nearest neighbours to get k synonyms.
As the process takes a bit of time, I chose to compute 5 synonyms for the 20000 most frequent words.

## Step 4 - Data Augmentation / Oversampling
We work on 1 labelled texts. We apply the following algorithm to modify a sentence : For each word in the sentence :
  
  Keep it with probability p (or if we don't have synonyms for it) Randomly swap it with one of its synonyms with probability 1−p
  







