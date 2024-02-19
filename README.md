# Project Summary
We wanted to build a long/short portfolio based on sentiment from articles. For that we used the Finbert model and a gpt model. 
While training the models on the data we got the stock prices from yahoo finance to add to our data. 
Once we got the sentiment based on those articles, we designed a strategy to buy or sell a stock based on the sentiment and its score. 
We incorporated proper risk management in our strategy. 

# Our Approach
## Data collection
Data from excel, then used the yahoo finance API to get the stocks' returns data.

## Preprocessing 
We had some data cleaning to do, considering that the data given had missing values that we had to fill. Additionally, some of the stocks included went through bankruptcy, so we were not able to get the prices for our whole dataset. 
## Modeling

# Results of the Sentiment Classification Model(s)


# Portfolio Constructed

## Strategy 
Buy when sentiment is positive and higher than 0.77 and sell wehn sentiment is negative and higher than 0.28. 
Have a take profit at 10%, and stop loss at 2.5%

# Instructions on running your model(s) and replicating your results

# Running the model post-production
