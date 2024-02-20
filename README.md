# Project Summary
We wanted to build a long/short portfolio based on sentiment from the articles. For that, we used multiple models that included GPT, RNN, BERT, and FinBERT models. Since most of the models produced low-accuracy results we focused on using the FinBERT model. While training the models we also used the Yahoo Finance API to get stock information on our desired tickers. Once we got the sentiment based on those articles, we designed a strategy to buy or sell a stock based on the sentiment and its score. Our goal was to use the AlphaLens package to perform backtesting and aid in portfolio construction but after errors persisted we decided to create our own tests.

# Our Approach
## Data collection / PreProcessing
Since the original dataset sentiment analysis was based on ESG and due to time constraints we decided to relabel the sentiments using ChatGPT. We gave ChatGPT the summary, description, ticker, and adjusted date. It then categorized each article as either positive, negative, or neutral. We did this for all the data. In addition, we merged this newly generated data with stock information pulled from Yahoo Finance API. Before giving ChatGPT our data to relabel we created a new column called adjusted date. This accounts for articles published after 4 pm EST since the stock market closes at the time. If an article was published after 4 pm EST then the adjusted date was the next day, if it was published before 4 pm then the date stayed the same. 

# Modeling Results / Replication
## GPT Model

## RNN Model

## BERT / FinBERT Model

# Portfolio Constructed
## Strategy 
Buy when sentiment is positive and higher than 0.77 and sell when sentiment is negative and higher than 0.28. 
Have a take profit at 10%, and stop loss at 2.5%
