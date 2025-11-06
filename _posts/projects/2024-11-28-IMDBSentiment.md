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


## Introduction

IMDb is a popular platform where users rate and review movies. Our project explores how the textual content of reviews relates to their sentiment. The goal is to develop a model that accurately predicts whether a review is **positive or negative** while uncovering the language patterns that drive sentiment.

The dataset used consists of 50,000 film reviews that are labelled as either positive or negative. The goal is to develop an optimal model that accurately predicts sentiment and identifies patterns in review content. 


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

---

## Methodology

We evaluated multiple **text embedding methods**:
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
