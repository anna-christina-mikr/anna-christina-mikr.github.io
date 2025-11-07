---
title: "Predicting Positive vs Negative Movie Reviews"
date: 2024-05-26
categories:
  - project
tags:
  - nlp
  - data science
  - sentiment analysis
  - text classification
header:
  teaser: /images/article_covers/imdb.png
excerpt: "Using NLP methods to predict sentiment of IMDb movie reviews"
---
This is a step-by-step walk through of sentiment analysis on IMDb movie reviews. The goal is to develop[ a model that accurately predicts whether a reciew is positive or negative by uncovering laanguage patterns that drive sentiment. This is a **Text Classification** task. 

## The Data

The dataset used consists of 50,000 film reviews scraped from IMDb that are labelled as either positive or negative. It can be found on Kaggle: The goal is to develop an optimal model that accurately predicts sentiment and identifies patterns in review content. 


---

## Data Processing

We cleaned the text by:
- Removing special characters, URLs, and HTML tags  
- Converting text to lowercase  
- Removing stopwords using NLTK  
- Tokenizing text into unigrams, bigrams, and trigrams  
- Adding `START` and `STOP` markers to preserve context  

These steps reduced noise and made the linguistic structure suitable for modeling.

---

## Exploratory Data Analysis

Before modeling, we visualized word frequency patterns.  
Positive reviews frequently used terms like *“excellent”* and *“fun,”* while negative reviews often used *“worst”* and *“poor.”*  
These clear distinctions highlighted the potential of text-based sentiment modeling.

<img width="792" height="265" alt="Screenshot 2025-11-06 at 1 28 55 PM" src="/images/imbd/wordcloud.png" />

---

## Methodology
In order to capture relationships between text, we need to transform it into embeddings. Embedding are numerical representations of words, words with similar meaning will have similar representations.

There are different types of text embedding methods, some more complex than others. This [schematic](https://www.geeksforgeeks.org/nlp/word-embeddings-in-nlp/) captures the 3 general categories. 

<img width="1220" height="601" alt="Screenshot 2025-11-07 at 10 07 44 AM" src="https://github.com/user-attachments/assets/bc3f0bc5-7c91-48ed-8cfe-30f3e2a002f5" />


We evaluated the following:
- **Bag of Words (BoW)**  
- **TF-IDF**  
- **GloVe**  
- **BERT embeddings**

Each embedding was used across several **machine learning models**: Logistic Regression, K-Nearest Neighbors (KNN), Random Forest, a Deep Neural Network (DNN), and a fine-tuned BERT Transformer model.

---

### Logistic Regression

The **TF-IDF Logistic Regression** model performed best, achieving **89.83% accuracy** and **AUC = 0.96**, followed closely by BoW (89.4% accuracy).  
Regularization experiments showed that **L2 penalties** (Ridge regression) were consistently optimal, suggesting that allowing all features to contribute led to more accurate predictions.

---

### K-Nearest Neighbors

KNN models were optimized through grid search for distance metrics and weighting schemes.  
The best-performing configuration used **TF-IDF** embeddings with distance-based weighting, achieving **79.92% accuracy** and **AUC = 0.88**.  
Performance decreased significantly for GloVe embeddings (54.66% accuracy), indicating poorer feature representation.

---

### Random Forest

Random Forest models captured complex word interactions.  
With **TF-IDF embeddings**, accuracy reached **84.02%**, while **BERT embeddings** followed closely at **83.62%**.  
BoW and GloVe lagged behind due to their limited ability to represent nuanced sentiment context.

---

### Deep Neural Network (DNN)

A 3-layer DNN trained with TF-IDF embeddings achieved **87.23% accuracy** and **AUC = 0.95**, outperforming simpler models but not BERT.  
The model used dropout regularization and ReLU activations to prevent overfitting.

---

### Fine-tuned BERT Transformer

The pre-trained **BERT** model achieved the highest overall performance, with **91.74% accuracy** and **AUC = 0.97**.  
Fine-tuning for two epochs with a learning ra
