# Project Summary
We aimed to construct a long/short portfolio by leveraging sentiment analysis from various models, including GPT, RNN, BERT, and FinBERT. Given the suboptimal accuracy of most models, we opted to use the FinBERT model. While training these models, we integrated the Yahoo Finance API to gather stock information for our specified tickers. Once sentiment analysis was obtained for the articles, we devised a strategy to buy or sell stocks based on sentiment and its corresponding score. Initially planning to utilize the AlphaLens package for backtesting and portfolio construction, persistent errors led us to develop our own tests instead.

# Our Approach
## Data collection / PreProcessing
Due to time constraints and the original dataset's focus on ESG, we opted to reevaluate sentiments using ChatGPT. We supplied ChatGPT with the article summary, description, ticker, and adjusted date, and it classified each article as positive, negative, or neutral. This relabeling process was applied to the entire dataset. Additionally, we integrated this newly generated sentiment data with stock information obtained from the Yahoo Finance API.

Before engaging ChatGPT for sentiment relabeling, we introduced a new column named "adjusted date." This column accommodates articles published after 4 pm EST, considering that the stock market closes then. If an article was published after 4 pm EST, the adjusted date reflects the following day; otherwise, it remains unchanged for articles published before 4 pm. To be consistent when running our models we allocated all 2022 data for training and all 2023 data for testing.

# Modeling Results
## GPT Model
* **Purpose:** We used GPT-3.5 to analyze the sentiment of text data and classified the data as positive, negative, or neutral.
* **Data Handling:** Split the data into two sets: 80% for training and 20% for testing.
* **Sentiment Analysis Function:** The text was sent to GPT-3.5 to perform sentiment analysis (positive, negative, or neutral).
* **Evaluating Accuracy:** We compared GPT-3.5’s sentiment predictions with the actual labels from the dataset. It calculated the accuracy of the model, which reflects how often GPT-3.5’s predictions were correct.
* **Results:** When compared with the sentiment provided in the data set, the model provided an accuracy of 37%.

## RNN Model
* **Preprocessing:** We converted categorical sentiment labels to numerical values using LabelEncoder, tokenized the summary, limiting it to a maximum of 5000 words, and converted the tokenized texts into sequences and padded them to a uniform length.
* **Model:** A Sequential RNN model was built using Keras which includes an Embedding layer, an LSTM layer with dropout and recurrent dropout for regularization, and a Dense output layer with softmax activation. L2 regularization was used in the LSTM layer to prevent overfitting.
* **K-Fold Cross-Validation:** Employed the use of 5-fold cross-validation for training to improve the robustness of the model. In each fold, the model was compiled, trained, and evaluated.
* **Training:** The model was trained with a batch size of 32 and for 10 epochs and the training loss and accuracy were tracked.
* **Evaluation:** After training, the model was evaluated on the separate testing dataset.
* **Results:** The final accuracy of the model was 68.32%

## BERT
* **Sentiment Distribution**: The dataset comprises 1801 negative, 948 neutral, and 836 positive articles, which is not balanced.
* **Dataset Split**: Allocated 80% for training and 20% for testing.
* **Model Choice**: Utilized BERT for sequence classification, targeting 3 sentiment classes.
* **Full Model Fine-Tuning**: Engaged all 109,484,547 parameters over 3 epochs, achieving a 39% accuracy. It took about an hour on an M1 Pro laptop.
* **Selective Fine-Tuning**: Froze all the layers before the head, only fine-tuned the last layer, involving 2,307 parameters across 100 epochs, resulting in 49% accuracy. which took 3 hours. Accuracy remained at 49% after the first 50 epochs of training.

The potential reasons for low accuracy:
* **Token Length Limitation**: BERT-Base, uncased supports up to 512 tokens, whereas the average article length in the dataset is 3,379 words, which means most articles were truncated before passing to the model.
* **Data Quality Concerns**: The training dataset may lack high quality, impacting model performance.


## FinBERT
* **Sentiment Ground Truth**: Employed ChatGPT-generated sentiment as the ground truth for fine-tuning.
* **Model Selection**: Choose FinBERT (ProsusAI/finbert) for sentiment analysis, focusing on three classes.
* **Initial Fine-Tuning**: Targeted the classifier layer with 2,307 parameters over 10 epochs, achieving 72% accuracy.
* **Advanced Fine-Tuning**: Expanded to fine-tune both pooler and classifier layers, totaling 592,899 parameters over 10 epochs, maintaining 72% accuracy.

The potential reasons for a much higher accuracy:
* **Choice of Target**: We chose summaries instead of articles as text in finetuning. Since most of the summaries fell within FinBERT's maximum token length.
* **Pre-trained Advantage**: The FinBERT model is already finetuned on an extensive financial corpus.

# Portfolio Constructed

## AlphaLens Package
The AlphaLens package is designed to analyze sentiment scores alongside stock prices, presenting the optimal portfolio mix using its inherent long/short strategy. Despite our thorough validation confirming the alignment of 'factor' and 'price' data frames indices, we encountered a "TypeError: incompatible index of inserted column with frame index," indicating a misalignment. This led us to conclude that the discrepancy stemmed from an issue within the AlphaLens package itself. Consequently, we decided to bypass this problem by manually coding our strategy.

## Strategy 
We designed a simple long/short strategy using our sentiment analysis. Proper risk management rules have also been added to our portfolio to ensure capital preservation. 
Rules we designed for our portfolio strategy:
* A stop loss of 2.5% on each trade
* A take profit of 10% on each trade
* Long the stock when sentiment is positive and sentiment score is higher than 0.77
* Short the stock when sentiment is negative and higher than 0.28.
* A maximum holding period for each trade of 5 days

We did try and play around with different combinations during backtesting, specifically for the stop loss and holding period, and these were the best metrics that yielded the best return. Our mean return on a trade was 1.3% which significantly underperforms the SP500 and the risk-free rate. During backtesting, we also observed some data errors in the stock data from the Yahoo Finance API, which yielded extreme profits and losses. These wiped out our trading account even with the risk management rules put in place. 

# Limitations & Future improvements
The biggest limitation for our group was the AlphaLens package and the integrity of the original dataset. We struggled with the package and it made backtesting more complicated than it should have been. 

For future improvements, we would use other factors including the sentiment to decide on when to long/short a stock. This would allow for a longer holding period which could lead to higher returns. We would also use news that does not come out during the trading day. We could also implement options trading as a way to manage our downside risk a bit more and amplify our returns. 
