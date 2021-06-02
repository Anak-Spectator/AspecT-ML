# Bad Word Detection

This repository is served as documentation for B21-CAP0479's Capstone Project. 

## Table of Contents
- [Dataset Sources](#dataset-sources)
- [Jupyter Notebook Content Explanation](#jupyter-notebook-content-explanation)
- [How to Replicate](#how-to-replicate)

## Dataset Sources
[Indonesian Abusive and Hate Speech Twitter Text](https://www.kaggle.com/ilhamfp31/indonesian-abusive-and-hate-speech-twitter-text?select=data.csv)

[Instagram Cyberbullying Comment](https://github.com/rizalespe/Dataset-Sentimen-Analisis-Bahasa-Indonesia/blob/master/dataset_komentar_instagram_cyberbullying.csv)


## Jupyter Notebook Content Explanation

### Dataset Preparation
Indonesian Abusive and Hate Speech Twitter Text originally is a dataset that classifies tweets in Indonesian whether they contain abusive language and/or hate speech. Furthermore, it also classifies what kind of hate speech a tweet is (towards an individual, towards group, religion-related, race-related, physical-related, gender-related, or another kind of hate speech). For our project purpose, we simply take `Abusive` column as a label.

On the other hand, the Instagram Cyberbullying Comment dataset classifies whether an Instagram comment has a positive or negative sentiment. For our project purpose, we labeled a comment with positive sentiment as 0 and a comment with negative sentiment as 1.

We then merged both datasets to a new dataset that has two columns, which are:
- Column `text`, contains tweets and Instagram comment
- Column `bad_word`, indicates whether the sentence contains a bad word or not. Value 0 means no bad word found in the sentence and value 1 otherwise.

### Data Preprocessing
The preprocessing steps that we applied to the dataset are:
- Convert text to lowercase
- Remove non-alphanumeric characters
- Remove new line, retweet symbol, username, mention, URL, and extra spaces
- Drop duplicate data

In the end, we get 13327 data with 8179 of them labeled as 0 and the remaining 5148 labeled as 1.

### Building Model
We split the dataset into 3 parts, which are training, validation, and testing with the proportions of 80%, 10%, and 10% respectively.

The model consists of a trainable 128-dimensional pre-trained embedding (taken from https://tfhub.dev/google/tf2-preview/nnlm-id-dim128-with-normalization/1) and the other six hidden layers, with five of them are Bidirectional LSTM layer. The output layer is a single neuron Dense layer with sigmoid as its activation function. 

We compiled the model with Adam optimizer and binary cross-entropy as the loss function. We trained it for 20 epochs and consistently got accuracy above 85%.

### Model Deployment
We exported the trained model to Google Cloud Storage Bucket and use the AI Platform to serve predictions. 

## How to Replicate
If you want to build a similar model and deploy it to your own Google Cloud Platform (GCP), you can run the [bad_word_detection](https://github.com/Anak-Spectator/AspecT-ML/blob/master/bad_word_detection.ipynb) notebook. Please make sure that you change the value of some variables (currently is an empty string) in the Deployment section to match your GCP information.