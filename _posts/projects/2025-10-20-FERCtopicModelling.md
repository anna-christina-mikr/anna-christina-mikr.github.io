---
title: "What Drives the Conversation? Exploring Energy News Through Topic Modeling"
date: 2024-05-26
categories:
  - project
tags:
  - topic modelling
  - data science
  - analysis
header:
  teaser: /images/article_covers/grid.png
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
  <img src="/images/article_covers/trigram.png" alt="Trigram Frequency in news data" width="600" height="400">
</div>

Here we see some of the top word trigrams visualized. This help us contextualize what we 3 words phrases we typically see in these articles, which can help us later with our topic analysis. 


### Using LDA as a baseline
Latend Dirichlet Allocation (not to be confused with Linear Discriminant analysis) is a topic modelling technique that uses Bayesian modelling to cluster words into topics. In other words, it assumes that given a corpus of words, each word can be allocated to a discrete set of topics. As you can imagine, the number of topics chosen is important, it affects how cohesive the topics are. 

The best method for topic number selection is using a key metric, plotting its value for a chosen range of possible topic numbers, and see where this value plateaus, where adding new topics offers no better value. I chose **Topic Coherece**, a measure of how similar words within a topic are.  

<div class="container">
  <img width="576" height="325" alt="Screenshot 2025-11-05 at 3 26 56 PM" src="https://github.com/user-attachments/assets/3c37cc77-97f5-4fde-a6f0-6626def89925" width="600" height="400" />
</div>



Interestingly, the coherence score reaches a peak at 12 topics and then steadily goes down. These values are not dramatically different, as a C_V score in the range of 0.5-0.59 is considered decent, but it is not the behavior we expect. This sharp decrease in performance could be due to the fact that our corpus is exclusively concerning energy news, so there is a lot of overlap in the topics discussed. In a different topic modelling scenario, e.g. modelling research papers: we would end up getting very distinct topics such as History, Physics, Anthropology, Political Science etc. Here, we reach a limit where no more specificity can be achieved, as each word has been allocated to a redundant subtopic that does not offer any more nuance. In our case, the energy news corpus is lexically and conceptually dense, with few truly distinct themes, after 12 topics the model keeps slicing coherent topics into smaller, overlapping pieces, which coherence penalizes it harshly.

Another way however to evaluate our choice for number of topics is to check the topic:



<iframe src="/images/ferc/lda-viz.html"
        width="200%"
        height="700"
        style="border:none; border-radius:10px;">
</iframe>


This approach uncovers another aspect of LDA, topics may have similar meaning between each other, yielding a high coherence score, but they may also have a lot of overlap. This graph is from gensim's package  **pyLDAvis**, its a great tool to visualize topic similarity!
It utilies the relevance of a word *w* to a topic *t*, calculated as:


$$
r(w, t \mid \lambda) = \lambda \, p(w \mid t) + (1 - \lambda) \, \frac{p(w \mid t)}{p(w)}
$$


Here, λ (*lambda*) controls the balance between how **frequent** and how **distinctive** a term is:

- λ = 1 → ranks words by how common they are within the topic (general terms).  
- λ = 0 → ranks words by how unique they are to that topic (specific terms).  
- λ ≈ 0.6 (default) → balances both views for interpretability.



We see here that a 12 topic LDA is insufficient, we have a lot of topic overlap; topic 4 and topic 5 are virtually the same, covering renewable energy projects and climate change related words. So in order to balance high coherence with topic uniqueness I chose 10 topics, and plotted this instead;

<iframe src="/images/ferc/ldaviz9.html"
        width="200%"
        height="750"
        style="border:none; border-radius:10px;">
</iframe>

Here we see that with nine topics a much better separated topics, so we are sticking with 9. Here are some words from the topics:

| **Topic ID** | **Top Terms** | **Likely Theme / Interpretation** |
|---------------|---------------|-----------------------------------|
| **0** | bill, house, republican, senate, tax, trump, committee, credit, democrat, budget | Federal policy and legislation |
| **1** | data, plant, demand, coal, nuclear, center, price, july, generation, reactor | Data centers, power generation, and nuclear demand |
| **2** | agency, rule, court, epa, environmental, trump, ferc, climate, order, law | Regulation, courts, and environmental policy |
| **3** | ferc, transmission, order, utility, rate, miso, market, proposal, pjm, process | Transmission and FERC regulatory orders |
| **4** | dam, water, river, pipeline, county, safety, area, lake, land, million | Infrastructure, water resources, and safety |
| **5** | capacity, market, gw, mw, pjm, demand, load, price, generation, solar | Grid capacity, markets, and generation |
| **6** | utility, transmission, electricity, solar, demand, line, system, wind, data, center | Utilities, electrification, and renewables |
| **7** | lng, export, pipeline, wind, facility, global, offshore, million, terminal, permit | LNG exports and offshore energy infrastructure |
| **8** | tariff, trump, lng, market, trade, oil, april, global, supply, import | Trade, tariffs, and global energy markets |


What stands out in these topics is how the vocabulary of energy policy reflects both technical and political worlds.
The model doesn’t just surface buzzwords, it sketches the infrastructure of how the U.S. talks about power itself.

On one side, we see the hard engineering lexicon: words like transmission, capacity, load, generation, pipeline, and LNG anchor discussions in the physical systems that move electrons and fuel.
These are the operational conversations, the ones about keeping the grid stable, expanding transmission lines, or meeting demand from new data centers.

On the other side sits the language of governance and law: bill, house, committee, court, rule, EPA, order.
This vocabulary reveals that energy is never just a technical challenge; it’s also a legislative and judicial process.
Policy and legal terms intermingle with industry keywords, reminding us that decisions about pipelines or nuclear reactors are made as much in congressional committees and courtrooms as in control rooms.

That overlap — between ferc, trump, epa, rule, and market — captures a particularly modern dynamic:
the way energy debates intersect with climate regulation, trade policy, and even global supply chains.
In other words, the “energy transition” isn’t just happening in the grid — it’s happening in the rhetoric that shapes how we understand and argue about it.

Beyond static interpretation, these topics also open the door to temporal comparison.

<iframe
  src="https://public.tableau.com/views/newsarticletopicdistribution&#47;Dashboard1?:showVizHome=no&:embed=true"
  width="100%"
  height="800"
  style="border:none; border-radius:10px;">
</iframe>






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
