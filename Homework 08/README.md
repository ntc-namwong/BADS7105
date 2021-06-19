 # Campaign Response Model

## Dataset

A retail dataset consists of 2 files.
- [Retail_Data_Transactions.csv](https://github.com/ntc-namwong/BADS7105/blob/main/Homework%2008/Retail_Data_Response.csv) contains 3 columns customer_id, trans_date, tran_amount which are on customer's basket level.
- [Retail_Data_Response.csv](https://github.com/ntc-namwong/BADS7105/blob/main/Homework%2008/Retail_Data_Transactions.csv) contains 2 columns customer_id, response which 0 is not response and 1 is response.

## Feature Engineering

Originally, there has three features, including, Recency, Frequency and Monetary. The additional features are shown below.
 - aou	is age of usage
 - ticket_size	is average ticket size of each user
 - frequency	is frequency
 - monetary_value	is monetary
 - ticket_size is average ticket size of each user

## Train models

The features from previous step are used to train XGBoost model and resampling by undersampling and oversampling. 

## Model Performance

The best model is XGBoost + oversampled with the result of test-auc score 0.715.

![Picture 8-1](https://github.com/ntc-namwong/BADS7105/blob/main/Homework%2008/Picture%208-1%20ROC%20Oversampling.jpg)
