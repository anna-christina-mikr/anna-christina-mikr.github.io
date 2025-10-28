---
title: "IMDb Movie Review Sentiment Analysis "
excerpt: "This was a project for my applied machine learning class. I also made some edits to take it a step further. The data used is a Kaggle dataset of 50k reviews with, half positive, half negative. The goal of the project is to predict the review of a sentiment (postive or negative) using text classification as well as deep learning models.'>"
collection: portfolio
---

## Results

| Model | Embedding | Accuracy | AUC |
|:--|:--|--:|--:|
| Logistic Regression | TF-IDF | 89.83% | 0.96 |
| Random Forest | TF-IDF | 84.02% | 0.92 |
| KNN | TF-IDF | 79.92% | 0.88 |
| Deep Neural Network | TF-IDF | 87.23% | 0.95 |
| **BERT Transformer** | BERT | **91.74%** | **0.97** |

**Key Takeaways:**
- TF–IDF was the best-performing traditional embedding.  
- Fine-tuned BERT achieved the **highest overall accuracy and AUC**, confirming the advantage of contextual embeddings.  
- GloVe underperformed likely due to its static nature and limited adaptability to domain-specific sentiment.
<img src='/images/wordcloud.png'>"

---

## Model Notebooks
Implementation is available on my github!
https://github.com/anna-christina-mikr/sentiment-analysis
- [Logistic Regression Notebook](../notebooks/logistic_regression.ipynb)  
- [KNN Notebook](../notebooks/knn.ipynb)  
- [Random Forest Notebook](../notebooks/random_forest.ipynb)  
- [Deep Neural Network Notebook](../notebooks/deep_neural_network.ipynb)  
- [BERT Transformer Notebook](../notebooks/bert_transformer.ipynb)


---

