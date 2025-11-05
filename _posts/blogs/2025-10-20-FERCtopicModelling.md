---
title: "Energy Discourse"
date: 2024-05-26
categories:
  - blog
tags:
  - topic modelling
  - data science
  - analysis
header:
  teaser: /assets/images/article_covers/grid.png
excerpt: "How does the media shape the conversation around energy?"
---

<style>
    .container {
        display: flex;
        justify-content: center;
        align-items: center;
    }
    img {
        margin: 10px;
        max-width: 100%;
        height: auto;
    }
</style>

### Context

Today marks the longest government lockdown in government history. 
This shutdown has hit energy hard, with soaring prices and delays on energy production projects. At the same time, there is a huge conversation around meeting electricity demands of power hungry AI by building more data centers, and the impact that has on local communities. There are dozens of other conversations happening in the energy space and this is what my group was interested in looking at and how the recent change in political power has shifted these conversations. 


The data for this project is almost 5000 articles from the same time windows in 2024 and 2025: spanning from February to July. Did some standard preprocessing for text, removed stopwords, punctuation and tokenized the data. We ended up with 



### Extract articles from txt files. 

Each raw news text file was compliled by taking multiple energy articles from the same day scaped from the web. As a result, a lot of these files end up hacing inconsistent spacing and formatting, as well as relics from the web interface. Luckily I had a list of all article titles at hand that i could utilize, but they were not always an exact match to the text. In order to segment the articles accurately, I tried implementing fuzzy string matching (Levenshtein distance) to align detected titles with known article headers. I managed to successfully link about 2,859 of 4,751 titles. After segmentation, articles underwent standard NLP preprocessing: tokenization, stopword and punctuation removal, and exclusion of filler journalistic terms (e.g., “said,” “reported”), resulting in a clean, comparable corpus suitable for topic modeling.


<div class="container">
  <img src="https://github.com/anna-christina-mikr/anna-christina-mikr.github.io/blob/main/assets/images/trigram.png" alt="Trigram Frequency in news data" width="600" height="400">
</div>

Using the keyboard shortcut (Option + Space) to open the GPT app and clicking ‘Take Screenshot’ of the browser was simple. GPT immediately provided a very accurate and detailed explanation of the chart, even highlighting key insights. I particularly liked how it called out ‘the significant role of AWS in Amazon’s financial structure.’

<div class="container">
  <img src="https://yudong-94.github.io/personal-website/assets/images/gpt-screenshots/gptapp_chart_interpratation.png" alt="GPT Chart Interpretation 1" width="300" height="600">
  <img src="https://yudong-94.github.io/personal-website/assets/images/gpt-screenshots/gptapp_chart_interpratation2.png" alt="GPT Chart Interpretation 2" width="300" height="600">
</div>

Taking it one step further, I challenged it to extract the data points as a downloadable dataset. It successfully saved the dataset as a CSV file with a download link! Notably, in the chart, there are two ‘Other’ categories, one under revenue and another under expenses. GPT automatically renamed one of them to ‘Other Expenses’ to avoid confusion – a detail I appreciated.  

<div class="container">
  <img src="https://yudong-94.github.io/personal-website/assets/images/gpt-screenshots/gptapp_csv.png" alt="GPT Generated CSV" width="600" height="400">
</div>

### Analyze and Visualize Data

GPT has the amazing capability of data analysis as I have evaluated in [this blog](https://yudong-94.github.io/personal-website/blog/EvaluatingChatGPTinDataScience/). It is even becoming stronger with [recent updates](https://openai.com/index/improvements-to-data-analysis-in-chatgpt/), though I haven’t got access to features like real-time table and visualization edits yet.

To continue testing the Desktop App, I asked it to make a visualization using the same dataset it had just prepared for me. It created a simple visualization with two bar charts and a text label of the operating profit at the bottom. This visualization is simple but effective in conveying the story.

<div class="container">
  <img src="https://yudong-94.github.io/personal-website/assets/images/gpt-screenshots/gptapp_gpt_visualization.png" alt="GPT Visualization 1" width="300" height="600">
  <img src="https://yudong-94.github.io/personal-website/assets/images/gpt-screenshots/gptapp_gpt_visualization2.png" alt="GPT Visualization 2" width="300" height="600">
</div>

I then asked it to calculate the profit margin for me. It provided the correct answer with well-formatted formulas, explanations, and the underlying Python code.

<div class="container">
  <img src="https://yudong-94.github.io/personal-website/assets/images/gpt-screenshots/gptapp_profit_margin.png" alt="GPT Profit Margin Calculation" width="600" height="400">
</div>

### Real-time Data Tool Assistance

The last thing I tested was how it could assist me using various data tools. I used Tableau to make a rough visualization using the same dataset, pretending I didn’t know much about Tableau. I then took a screenshot and asked GPT how to improve it.

It provided high-level recommendations as well as step-by-step guidance. These were accurate and easy to follow. The adjusted visualization turned out to be quite similar to the one GPT made earlier, not very advanced but straightforward with this simple dataset. I am excited to utilize this capability further for my [weekly visualization project](https://yudong-94.github.io/personal-website/tags/#data-visualization).

<div class="container">
  <img src="https://yudong-94.github.io/personal-website/assets/images/gpt-screenshots/gptapp_tableau_draft.png" alt="GPT Tableau Guidance 1" width="300" height="600">
  <img src="https://yudong-94.github.io/personal-website/assets/images/gpt-screenshots/gptapp_tableau_guidance.png" alt="GPT Tableau Guidance 2" width="300" height="600">
</div>

<div class="container">
  <img src="https://yudong-94.github.io/personal-website/assets/images/gpt-screenshots/gptapp_tableau_final.png" alt="GPT Guided Tableau Visualization" width="600" height="400">
</div>

### Final Thoughts

In the sections above, I demonstrated how the GPT Desktop App can be seamlessly integrated into day-to-day analytics workflows. The example is simple but highlights the potential for various use cases. Here are some ideas that came to mind:
* Summarize the articles you are reading and answer questions on the topic;
* Ask coding questions and get real-time debugging help;
* Reply to incoming emails;
* Voice chat with GPT to brainstorm the analysis you are working on, etc.

There are existing plugins for these use cases, but the GPT Desktop App simplifies them and allows you to interact with everything in one place. I plan to explore more, and I’d love to hear any ideas you have!
