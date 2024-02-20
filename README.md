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

# Portfolio Constructed
## Strategy 
Buy when sentiment is positive and higher than 0.77 and sell when sentiment is negative and higher than 0.28. 
Have a take profit at 10%, and stop loss at 2.5%
