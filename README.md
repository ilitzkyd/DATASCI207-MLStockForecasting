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

From the final table bewlow we can draw several conclusions:  

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

| Model Type | Optimizer | Batch Size | Learning Rate | Sequence Length | # Parameters | Train RMSE | Validation RMSE | Test RMSE |
|------------|-----------|------------|---------------|-----------------|--------------|------------|-----------------|-----------|
| BASELINE   | -         | -          | -             | -               | -            | 2.25       | 5.28            | **4.79**  |
| LSTM       | Adam      | 32         | 0.001         | 60              | 31901        | 2.31       | 5.71            | 5.90      |
| LSTM       | Adam      | 64         | 0.001         | 60              | 31901        | 2.49       | 6.48            | 6.93      |
| LSTM       | Adam      | 32         | 0.01          | 60              | 31901        | 4.43       | 5.76            | 5.38      |
| LSTM       | Adam      | 32         | 0.001         | 90              | 31901        | 3.15       | 7.34            | 5.84      |
| LSTM       | SGD       | 32         | 0.001         | 60              | 31901        | 5.05       | 14.58           | 24.93     |
| LSTM       | SGD       | 64         | 0.001         | 60              | 31901        | 20.83      | 65.50           | 92.94     |
| LSTM       | SGD       | 32         | 0.01          | 60              | 31901        | 4.44       | 15.44           | 32.35     |
| LSTM       | SGD       | 32         | 0.001         | 90              | 31901        | 5.17       | 16.15           | 35.06     |
| GRU        | Adam      | 32         | 0.001         | 60              | 24551        | 2.54       | 6.37            | 5.46      |
| GRU        | Adam      | 64         | 0.001         | 60              | 24551        | 3.68       | 7.68            | 6.68      |
| GRU        | Adam      | 32         | 0.01          | 60              | 24551        | 2.38       | 6.41            | 15.97     |
| GRU        | Adam      | 32         | 0.001         | 90              | 24551        | 2.77       | 6.35            | 5.18      |
| GRU        | SGD       | 32         | 0.001         | 60              | 24551        | 13.05      | 37.69           | 50.47     |
| GRU        | SGD       | 64         | 0.001         | 60              | 24551        | 33.68      | 98.16           | 127.05    |
| GRU        | SGD       | 32         | 0.01          | 60              | 24551        | 2.83       | 7.54            | 8.19      |
| GRU        | SGD       | 32         | 0.001         | 90              | 24551        | 4.82       | 11.82           | 17.50     |

## Contributions

All the team members contributed equally to this project. Data selection, processing, model selection, algorithm implementations and slides were all produced as a group with equal participation.