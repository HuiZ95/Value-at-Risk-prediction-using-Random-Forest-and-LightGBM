# Value-at-Risk-prediction-using-Random-Forest-and-LightGBM

**Value at Risk (VaR)** is an estimate of how much an investment portfolio might lose with a given confidence interval on a specific period of time. Here, we will focus on estimating the one day 95% VaR on a portfolio composed only by **S&P500 shares**. 
<br>(Let's assume that the one day 95% VaR is X%; that means that over a trading year of 252 days  I expect to incur in losses equal or greater than X% of my investment value in 5% of the cases, namely 12,6 trading days)

In this project we will estimate the VaR using **Machine Learning based approaches** (**Random Forest** and **LightGBM**). We will then compare the performance of these models with **Historical Simulation** which relies on a more traditional approach to VaR computation.

The project is divided in three sections:
<br> **1. Data preparation**
<br> **2. Model training & Validation**
<br> **3. Backtest**

## 1. Data preparation
In this section we will download the daily closing prices of the S&P500 index from Yahoo Finance from 2018-01-01 to 2019-12-31. We will then compute the **daily log return** and the daily **95% actual VaR** at time *t* as the fifth quantile in the daily log return distribution from day *t-30-1* to day *t* (included). 
<br> Then we will transform the time series of VaR values in **tabular form**: the actual VaR at day *t* will be our response variable and the VaR at day *t-1* the regressor.
<br>![image](https://user-images.githubusercontent.com/117392795/204327474-91288fe6-29d0-4e27-b460-706887220c27.png)

## 2. Model training & Validation
After creating the dataset, we are ready to train our models.
<br> We will use three models to make our prediction: **Random Forest**, **LightGBM** and **Historical Simulation**.
<br>**Random Forest** and **LightGBM** are two supervised Machine Learning algorithms based on decision trees. In order to find the best combinations of **hyperparameters** we will use the optimizations algorithm in the **Optuna** package instead of the traditional Grid Search. Finally, in order to make the most accurate predictions we will bw using the one step **Walk Forward Validation** method by including in the training set the previous day's observed VaR (at day *t+2* we will retrain the model on the training set and the observed value of the response variable at time *t* and *t+1*). 

<br>Finally with the **Historical Simulation** method we assume that the distribution of returns computed from historical data is a good approximation of the distribution of future returns. In this case we essentially take the historical *n* returns of the latest *n* periods and sort them from smallest to largest. The 95% VaR at time *n+1* is the quantile that corresponds to
the desired level of confidence, 95%. if *n* = 100, the 95% VaR at day 101 is the fifth smallest observation out of 100.

## 3. Backtest
Finally we will evaluate the performances of the three selected models by conducting **Kupiec's Proportion of Failure (POF) Test**.
<br> With the POF test we can test if the frequency of the actual VaR violations (number of days where losses exceed predicted VaR) is statistically different from the theoretical one which we can compute as <br> *validation set size * 5%* = 71 days * 5% = 4 days. (In other words we expect 4 days in which the estimated VaR will be violated).

Under the null hypothesis, the test statistic is distributed as a chi-squared distribution with one degree of freedom


As we will see from the results, the machine learning approaches outperformed Historical Simulation:
<br>![image](https://user-images.githubusercontent.com/117392795/204328690-59c918e1-5db9-4eb7-80e7-2b020db2c4a9.png)

<br> - for both **Random Forest** and **LightGBM** losses were greater than the predicted VaR values in 5 instances (5 VaR violations)
<br> - for **Historical Simulation** losses were greater than the predicted VaR values in 7 instances (7 VaR violations).
