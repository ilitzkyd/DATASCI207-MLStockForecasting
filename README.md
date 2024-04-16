# Stock Forecasting Using RNNs

Final project - DATSCI 207  
Spring 2024  

Daniel Florencio   
David Ilitzky  
Tahlee Stone  

These document is a very brief overview of the project. Please check the notebook for the full work.

## Motivation

Interest in stock market participation has surged in recent years, with both retail and institutional investors seeking higher risk-adjusted returns than the market.

In the United States, the S&P 500s has climbed to record highs on 22 separate days in 2024, even after adjusting for inflation, further adding to interest in equity markets.

Accurately predicting market behaviour could provide a competitive edge, prompting exploration of machine learning techniques.

Previous studies have explored traditional (fundamental and technical analysis) and machine-learning based approaches for stock market prediction, with mixed success. Stock market prediction is recognized as one of the most relevant, but highly challenging takss in fiancnial research.

This project aims to leverage recurrent neural networks (RNNs), specifically LSTM and GRU frameworks, to develop a model for stock price predictions. Historical stock price data will be collected, preprocessed and used to train and evaluate LSTM and RNN models.

## Data

Historical stock price data are obtained from the `Yahoo! Finance API`.  Analysis uses Microsoft Corporation `MSFT` daily closing prices using ten years’ of data `01 Mar 2014 to 31 Mar 2024`.  

Yahoo! Finance API provides historic data for stocks, EFTs and market indices globally. 

Restricted to one stock for model development, but can easily extend sample. 

With a train-dev split of 70%/30%, training set includes data to approximately the end of 2020. 
Remaining data used to create validation and test samples. 

![MSFT Stock Price](Image_1.JPG)  

## Modeling

### Base Model  

We defined very simple baseline model before employing machine learning techniques, a persistence model.  
The persistence model simply predicts the next value in the sequence to be the same as the last value observed.

![Base Model](Image_2.JPG)  

### Experiments

In the experiments conducted, various configurations of `LSTM - Long Short-Term Memory` and `GRU - Gated Recurrent Unit` models were evaluated. 

These experiments varied the `optimizer`, `batch size`, `learning rate`, and `sequence length` while measuring the model's performance based on the `training`, `validation`, and `test` root mean squared error (RSME).  

The results provide insights into which configurations perform best for predicting stock prices.  

The table below shows the `Experiment Configurations`.

<center>

| Model Type | Optimizer | Batch Size | Learning Rate | Sequence Length |
|------------|-----------|------------|---------------|-----------------|
| BASELINE   | -         | -          | -             | -               |
| LSTM       | Adam      | 32         | 0.001         | 60              |
| LSTM       | Adam      | 64         | 0.001         | 60              |
| LSTM       | Adam      | 32         | 0.01          | 60              |
| LSTM       | Adam      | 32         | 0.001         | 90              |
| LSTM       | SGD       | 32         | 0.001         | 60              |
| LSTM       | SGD       | 64         | 0.001         | 60              |
| LSTM       | SGD       | 32         | 0.01          | 60              |
| LSTM       | SGD       | 32         | 0.001         | 90              |
| GRU        | Adam      | 32         | 0.001         | 60              |
| GRU        | Adam      | 64         | 0.001         | 60              |
| GRU        | Adam      | 32         | 0.01          | 60              |
| GRU        | Adam      | 32         | 0.001         | 90              |
| GRU        | SGD       | 32         | 0.001         | 60              |
| GRU        | SGD       | 64         | 0.001         | 60              |
| GRU        | SGD       | 32         | 0.01          | 60              |
| GRU        | SGD       | 32         | 0.001         | 90              |

</center>


## Conclusions

From the final table below we can draw several conclusions:  

**Baseline Performance**: None of the models was able to beat the base model.  

**Optimizers Impact**: The choice of optimizer significantly affects model performance. Models trained with the `Adam` optimizer performed better compared to those trained with `SGD`.  

**Batch Size Influence**: Across all the experiments `small (32)`  batch sizes performd better than `large (64)` batch sizes, holding everything else constant.  

**Learning Rate Sensitivity**: The learning rate also plays a crucial role. A smaller `learning rate of 0.001` yielded better results across all the experiments.  

**Sequence Length Effect**: Models trained with a `longer sequence length (90)` had higher error rates compared to those trained with `shorter sequence lengths (60)`, suggesting that longer historical data sequences might not always improve prediction accuracy.  

**Model Comparison**: The performance between `LSTM` and `GRU` models is comparable, with slight variations depending on the optimizer, batch size, learning rate, and sequence length. The `LSTM` models on avarage performed slightly better.  

**Overfitting**: There are signs of overfitting, especially noticeable in models with significantly lower training RMSE compared to validation and test RMSE. This suggests that these models might be fitting too closely to the training data and performing poorly on unseen data.  

**Recommendations**: Based on the results, it seems that models trained with the `Adam optimizer` and a `learning rate of 0.001` generally perform better. Additionally, careful consideration should be given to batch size and sequence length.  

In summary, while LSTM and GRU models show promise for stock price prediction, further optimization and fine-tuning of hyperparameters are necessary to enhance predictive accuracy.  

Additionally, it would be beneficial to explore other features to enhance the models, such as including external indexes, company specific metrics, and macroeconomic variables.  

| Model Type | Optimizer | Batch Size | Learning Rate | Sequence Length | Train RMSE   | Validation RMSE | Test RMSE |
|------------|-----------|------------|---------------|-----------------|--------------|-----------------|-----------|
| BASELINE   | -         | -          | -             | -               | 2.25         | 5.28            | **4.79** |
| LSTM       | Adam      | 32         | 0.001         | 60              | 2.31         | 5.81            | **5.17**  |
| LSTM       | Adam      | 64         | 0.001         | 60              | 2.37         | 6.29            | 7.81      |
| LSTM       | Adam      | 32         | 0.01          | 60              | 5.15         | 5.72            | 9.65      |
| LSTM       | Adam      | 32         | 0.001         | 90              | 3.17         | 7.34            | 5.86      |
| LSTM       | SGD       | 32         | 0.001         | 60              | 4.76         | 13.93           | 23.57     |
| LSTM       | SGD       | 64         | 0.001         | 60              | 29.00        | 84.67           | 113.78    |
| LSTM       | SGD       | 32         | 0.01          | 60              | 4.19         | 11.51           | 14.62     |
| LSTM       | SGD       | 32         | 0.001         | 90              | 5.05         | 14.86           | 31.54     |
| GRU        | Adam      | 32         | 0.001         | 60              | 2.52         | 6.27            | 5.33      |
| GRU        | Adam      | 64         | 0.001         | 60              | 2.86         | 6.58            | 5.69      |
| GRU        | Adam      | 32         | 0.01          | 60              | 2.67         | 6.16            | 15.41     |
| GRU        | Adam      | 32         | 0.001         | 90              | 3.18         | 6.93            | 5.61      |
| GRU        | SGD       | 32         | 0.001         | 60              | 20.70        | 60.12           | 78.89     |
| GRU        | SGD       | 64         | 0.001         | 60              | 32.02        | 93.31           | 120.70    |
| GRU        | SGD       | 32         | 0.01          | 60              | 2.84         | 7.55            | 8.23      |
| GRU        | SGD       | 32         | 0.001         | 90              | 7.80         | 20.46           | 30.43     |

## Contributions

All the team members contributed equally to this project. Data selection, processing, model selection, algorithm implementations and slides were all produced as a group with equal participation.
