# Value-at-Risk-prediction-using-Random-Forest-and-LightGBM

**Value at Risk (VaR)** is an estimate of how much an investment portfolio might lose with a given confidence interval on a specific period of time. Here, we will focus on estimating the one day 95% VaR on a portfolio composed only by **S&P500 shares**. 
<br>(Let's assume that the one day 95% VaR is X%; that means that over a trading year of 252 days  I expect to incur in losses equal or greater than X% of my investment value in 5% of the cases, namely 12,6 trading days)

In this project we will estimate the VaR using **Machine Learning based approaches** (**Random Forest** and **LightGBM**). We will then compare the performance of these models with **Historical Simulation** which relies on a more traditional approach to VaR computation.

The project is divided in three sections:
<br> **1. Data preparation**
<br> **2. Model training & Validation**
<br> **3. Backtest**

## 1. Data preparation
In this section we will download the daily closing prices of the S&P500 index from Yahoo Finance from 2018-01-01 to 2019-12-31. We will then compute the **daily log return** and the **95% actual VaR** at time *t* as the fifth quantile in the daily log return distribution from day *t-30-1* to day *t* (included). 
<br> Then we will transform the time series of VaR values in **tabular form**: the actual VaR at day *t* will be our response variable and the VaR at day *t-1* the regressor.
