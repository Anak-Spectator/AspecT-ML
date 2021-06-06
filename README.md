# Bad Word Detection

This repository is served as a documentation for B21-CAP0479's Capstone Project.

## Table of Contents
- [Versioning](#versioning)
- [Dataset Sources](#dataset-sources)
- [Jupyter Notebook Content Explanation](#jupyter-notebook-content-explanation)
	* [Dataset Preparation](#dataset-preparation)
	* [Data Preprocessing](#data-preprocessing)
	* [Building Model](#building-model)
	* [Model Deployment](#model-deployment)
- [How to Replicate](#how-to-replicate)
- [Credits](#credits)

## Versioning
We currently have two versions of the model with some differences in the data preprocessing steps. You can find more detailed information in the [Data Preprocessing](#data-preprocessing) section. Both model versions give accuracy above 85% on the validation and test set, but the second version has a much better loss on the validation set.

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
- Remove non-alphanumeric characters
- Remove new line, retweet symbol, username, mention, URL, and extra spaces
- Convert text to lowercase
- Drop duplicate data

On the second version, we add these preprocessing steps before dropping duplicate data:
- Remove carriage return, ampersand symbol, and hexadecimal characters
- Remove repeating letters at the end of the word (e.g. pagiii semuaa  -> pagi semua)
- Transform non-standard words to standard words, you can access the full dictionary [here](https://www.kaggle.com/ilhamfp31/indonesian-abusive-and-hate-speech-twitter-text?select=new_kamusalay.csv)
- Remove [stopwords](https://www.kaggle.com/oswinrh/indonesian-stoplist?select=stopwordbahasa.csv) (adalah, dari, kepada, etc.)

Reference: https://www.kaggle.com/aldonistan/preprocessing-the-indonesian-twitter-toxic-comment

### Building Model
We split the dataset into 3 parts, which are training, validation, and test setwith the proportions of 80%, 10%, and 10% respectively.

The model consists of a trainable 128-dimensional pre-trained embedding (taken from https://tfhub.dev/google/tf2-preview/nnlm-id-dim128-with-normalization/1) and the other six hidden layers, with five of them are Bidirectional LSTM layer. The output layer is a single neuron Dense layer with sigmoid as its activation function. 

We compiled the model with Adam optimizer and binary cross-entropy as the loss function. We trained it for 20 epochs and consistently got accuracy above 85% on the validation and test set.

### Model Deployment
We exported the trained model to Google Cloud Storage Bucket and use the AI Platform to serve predictions. 

## How to Replicate
If you want to build a similar model and deploy it to your own Google Cloud Platform (GCP), you can run the [bad_word_detection](https://github.com/Anak-Spectator/AspecT-ML/blob/master/version-1/bad_word_detection.ipynb) or [bad_word_detection_v2](https://github.com/Anak-Spectator/AspecT-ML/blob/master/version-2/bad_word_detection_v2.ipynb) notebook. Please make sure that you change the value of some variables (currently is an empty string) in the `Deployment` section to match your GCP information.

## Credits
- Muhammad Okky Ibrohim and Indra Budi. 2019. Multi-label Hate Speech and Abusive Language Detection in Indonesian Twitter. In ALW3: 3rd Workshop on Abusive Language Online, 46-57.
- Luqyana, W., Cholissodin, I., & Perdana, R.S. (2018). Analisis Sentimen Cyberbullying pada Komentar Instagram dengan Metode Klasifikasi Support Vector Machine. Jurnal Pengembangan Teknologi Informasi Dan Ilmu Komputer, 2(11), 4704-4713.