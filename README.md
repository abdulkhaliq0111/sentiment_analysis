# sentiment_analysis


##OVERVIEW:

Sentiment Analysis refers to the use of natural language processing,text analysis,computational linguistics, andbiometricsto systematically identify, extract, quantify, and study affective states and subjective information.



The objective of this task is to detect hate speech in tweets. For the sake of simplicity, we say a tweet contains hate speech if it has a racist or sexist sentiment associated with it. So, the task is to classify racist or sexist tweets from other tweets.

Formally, given a training sample of tweets and labels, where label '1' denotes the tweet is racist/sexist and label '0' denotes the tweet is not racist/sexist, your objective is to predict the labels on the test dataset.




First take a look at the data.Dataset has 31962 entries, with no null entries.. where 29720 entries of the data with label 0(non-racist), and 2242 entries of the data with label 1 (racist).it clearly shows how our data is highly imbalanced.

Imbalanced Corpus or Dataset was always a challenge for supervised machine learning fellows. Imbalance means that not all classes in the dataset has the same size, there could be a bias to one or more classes (majority) over other less presented classes (minority).


##DATA CLEANING:
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


##Story Generation and Visualization from Tweets:

A) Understanding the common words used in the tweets: WordCloud

To see how well the given sentiments are distributed across the train dataset. One way to accomplish this task is by understanding the common words by plotting wordclouds.We can see most of the words are positive or neutral. Words like love, great, friend, life are the most frequent ones. It doesn’t give us any idea about the words associated with the racist/sexist tweets. Hence, we will plot separate wordclouds for both the classes (racist/sexist or not) in our train data.


B) Words in non racist/sexist tweets

Most of the frequent words are compatible with the sentiment, i.e, non-racist/sexists tweets. Similarly, we will plot the word cloud for the other sentiment. Expect to see negative, racist, and sexist terms.

C) Racist/Sexist Tweets





