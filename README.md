# Project Summary
We aimed to construct a long/short portfolio by leveraging sentiment analysis from various models, including GPT, RNN, BERT, and FinBERT. Given the suboptimal accuracy of most models, we opted to emphasize the use of the FinBERT model. During the training of these models, we integrated the Yahoo Finance API to gather stock information for our specified tickers. Once sentiment analysis was obtained from the articles, we devised a strategy to buy or sell stocks based on sentiment and its corresponding score. Initially planning to utilize the AlphaLens package for backtesting and portfolio construction, persistent errors led us to develop our own tests instead.

# Our Approach
## Data collection / PreProcessing
Due to time constraints and the original dataset's focus on ESG, we opted to reevaluate sentiments using ChatGPT. We supplied ChatGPT with the article summary, description, ticker, and adjusted date, and it classified each article as positive, negative, or neutral. This relabeling process was applied to the entire dataset. Additionally, we integrated this newly generated sentiment data with stock information obtained from the Yahoo Finance API.

Before engaging ChatGPT for sentiment relabeling, we introduced a new column named "adjusted date." This column accommodates articles published after 4 pm EST, considering that the stock market closes at that time. If an article was published after 4 pm EST, the adjusted date reflects the following day; otherwise, it remains unchanged for articles published before 4 pm.

# Modeling Results / Replication
## GPT Model

## RNN Model

## BERT / FinBERT Model

### BERT
**BEFORE** We realized human labeled sentiment is not for this project, I used this as the target for finetuning.

* **Sentiment Distribution**: The dataset comprises 1801 negative, 948 neutral, and 836 positive articles, which is not balanced

* **Dataset Split**: Allocated 80% for training and 20% for testing.

* **Model Choice**: Utilized BERT for sequence classification, targeting 3 sentiment classes.

* **Full Model Fine-Tuning**: Engaged all 109,484,547 parameters over 3 epochs, achieving a 39% accuracy. It took about an hour on my laptop (M1 Pro).

* **Selective Fine-Tuning**: I froze all the layers before the head, only fine tuned the last layer, involving 2,307 parameters across 100 epochs, resulting in a 49% accuracy. which took 3 hours


But, the accuracy remained stuck at 49% after the first 50 epochs of training.


The potential reasons for low accuracy:
* **Token Length Limitation**: BERT-Base, Uncased supports up to 512 tokens, whereas the average article length in the dataset is 3,379 words, which means most articles were truncated before pass to the model.
* **Data Quality Concerns**: The training dataset may lack high quality, impacting model performance.


### FinBERT
* **Data Preparation**: Utilized 4 months of data from 2022 for fine-tuning and all 2023 data for predictions.
* **Sentiment Ground Truth**: Employed ChatGPT-generated sentiment as the ground truth for fine-tuning.
* **Model Selection**: Chose FinBERT (ProsusAI/finbert) for sentiment analysis, focusing on three classes.
* **Initial Fine-Tuning**: Targeted the classifier layer with 2,307 parameters over 10 epochs, achieving 72% accuracy.
* **Advanced Fine-Tuning**: Expanded to fine-tune both pooler and classifier layers, totaling 592,899 parameters over 10 epochs, maintaining 72% accuracy.

The potential reasons for a much higher accuracy:

* **Choice of Target**: I chose summaries instead of articles as text in finetuning. Since most of the summaries fell within FinBERT's maximum token length.
* **Pre-trained Advantage**: FinBERT model has already finetuned on an extensive financial corpus.

# Portfolio Constructed

## AlphaLens Package
The AlphaLens package is designed to analyze sentiment scores alongside stock prices, presenting the optimal portfolio mix using its inherent long/short strategy. Despite our thorough validation confirming the alignment of 'factor' and 'price' dataframes' indices, we encountered a "TypeError: incompatible index of inserted column with frame index," indicating a misalignment. This led us to conclude that the discrepancy stemmed from an issue within the AlphaLens package itself. Consequently, we decided to bypass this problem by manually coding our strategy.

## Strategy 
We desinged a simple long/short strategy using our sentiment analysis. Proper risk management rules have also been added to our portfolio to ensure capital preservation. 
Here are the rules we designed for our portfolio strategy:
* A stop loss of 2.5% on each trade
* A take profit of 10% on each trade
* Long the stock when sentiment is positive and sentiment score is higher than 0.77
* Short the stock when sentiment is negative and higher than 0.28.
* A maximum holding period for each trade of 5 days

We did try and play around with different combinations during backtesting, specifically for the stop loss and holding period, and these were the best metrics that yielded the best return. 
Our mean return on a trade was 1.3% which significantly underperforms the SP500 and the risk free rate. 
During backtesting we also observed some data errors in the stock data, which yielded extreme profit and losses. These wiped out our trading account even with the risk management rules put in place. 

## Limitations & Future improvements
The biggest limitation for our group was the alphalens package, and the integrity of the original dataset. 
We struggled a lot with the package and it made backtesting impossible for us. 

For future improvements, we would use other factors inlcuding the sentiment to make a decision of when to long/short a stock. This would allow for a longer holding period which could lead to higher returns. 
We would also use news that does not come out during the trading day. 
We might also implement options trading as a way to manage our downside risk a bit more, and amplify our returns. 
