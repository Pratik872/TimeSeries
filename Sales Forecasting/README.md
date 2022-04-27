# Rossmann Drug Sales Forecasting

## Overview
- Rossmann is a European drug distributor which operates over 3,000 drug stores across seven European countries. Since a lot of drugs come with a short shelf life, that is, they do not have a long expiry date, it becomes imperative for Rossmann to accurately forecast sales at their individual stores. 

- As expected, store sales are influenced by many factors, including promotional campaigns, competition, state holidays, seasonality, and locality. 

- The data is present for 1115 drug stores. I have built a model for one of the drug stores whose id is '1'(Please check the dataset)

Please check [here](https://www.kaggle.com/c/rossmann-store-sales) for more details on dataset.

## Motivation
- I was always interested in how production companies manage their inventories and how do they forecast the future sales.

- I chose this dataset because I found it very interesting as there are some features included in the dataset which might affect the time series data. So I would get to know how external factors affect our target sales.

- This model would give you approximate fare value and not the exact!

## Project Structure

- [readme_resources](https://github.com/Pratik872/TimeSeries/tree/main/Sales%20Forecasting/readme%20resources) : This folder has all the images used to create readme file.

- [Sales Forecasting.ipynb](https://github.com/Pratik872/TimeSeries/blob/main/Sales%20Forecasting/Sales_forecasting_Pratik_Waghmare.ipynb) : This jupyter notebook has the code for making models.

## Problem Objective
- To build a time series model which forecasts the sale of Rossmann drug seller store number 1

## Methodology

### Outliers treatment
- As we have to build time series models, we know that outliers need to treated as otherwise it would be difficult for the models to converge.

- 'Sales' and 'Customers' were having outliers. You can see the plots below :

![SalesBeforeOutliers](https://github.com/Pratik872/TimeSeries/blob/main/Sales%20Forecasting/readme%20resources/sales_box_before_outliers.png)

![CustomersBeforeOutliers](https://github.com/Pratik872/TimeSeries/blob/main/Sales%20Forecasting/readme%20resources/customers_box_before_outliers.png)

![RegBeforeOutliers](https://github.com/Pratik872/TimeSeries/blob/main/Sales%20Forecasting/readme%20resources/before%20outliers.png)

- As we can see the linearity is also affected by the outliers. I have capped the outliers below 5th percentile and above 95th percentile. Please look at the plots below :

![SalesAfterOutliers](https://github.com/Pratik872/TimeSeries/blob/main/Sales%20Forecasting/readme%20resources/sales_box_after_outliers.png)

![CustomersAfterOutliers](https://github.com/Pratik872/TimeSeries/blob/main/Sales%20Forecasting/readme%20resources/customers_box_after_outliers.png)

![RegAfterOutliers](https://github.com/Pratik872/TimeSeries/blob/main/Sales%20Forecasting/readme%20resources/after%20outliers.png)

- As we can see the outliers treatment is done

### Feature Engineering
- I have handled the missing values by imputing/dropping wherever necessary. I have dropped the features which were not necessary for forecasting.

- Conversion of few features from object into required types was necessary

### Data decomposition(Seasonality,trend check):
- Stationary data i.e data whose 'mean' is constant for given period of time is easier to model and easier to converge. Time series models like AR,MA,ARMA,ARIMA,ARIMAX,SARIMA,SARIMAX,VAR,VARMA,VARMAX requires data to be stationary. So I used decomposition method to find the trend and seasonality in the data.

- The plots for 'Sales' are : 
![SalesDecomp](https://github.com/Pratik872/TimeSeries/blob/main/Sales%20Forecasting/readme%20resources/sales%20decompose.png)

- The plots for 'Customers' are :
![CustomersDecomp](https://github.com/Pratik872/TimeSeries/blob/main/Sales%20Forecasting/readme%20resources/customers%20decompose.png)

- As we can see for both the plots, we don't have an increading/decreasing trend as such i.e the series seems to be stationary, but still I have checked for stationarity using Adfuller test afterwards. 

### Data Stationarity Check:
- As weknow that the series needs to be stationary to be fed into the models, I have done stationarity test i.e AdFuller which is based on hypothesis testing.

- The null hypothesis for this test is 'The series is not stationary' and if our p-value comes out to be greater that 5%, then we accept the null hypothesis and vice-versa.

- For both 'Sales' and 'Customers' the p-value was less than 5% and hence the series is stationary.

### ACF and PACF plots:
- PACF plot is used to find the 'p' value which is the parameter for AR models. Basically PACF tells us the DIRECT relation between the original time series and the lagged time series. Check the plot below:

![PACFSales](https://github.com/Pratik872/TimeSeries/blob/main/Sales%20Forecasting/readme%20resources/sales%20PACF.png)

- Here as we can see I have plotted for 30 lagged values as per my system's computational capacity. And hence I chose p-value as 29 for most of my models.

- Similarly ACF plot is used to find 'q' value which is also know as 'window size' for MA models. ACF also includes the indirect relations between the lagged time series. Check the plot below:

![ACFSales](https://github.com/Pratik872/TimeSeries/blob/main/Sales%20Forecasting/readme%20resources/sales%20ACF.png)

- As we see in the plot I chose my q-value to be 30 for most of my models as per my system's computational capacity.

### Causality test for VAR/VARMA/VARMAX models:
- If some other time series(here,'Customers') influences our target time series(here,'Sales'), then we can use these models.

- We can find the relation between these series using a 'GrangerCausalityTest' which also works on hypothesis testing. If we get p-value below 5%, we can conclude that one time series causes the other i.e influences the other time series.

- Here I got the p-values below 5% and hence I concluded that 'Customers' time series influences 'Sales' time series and hence I have also build VAR, VARMA, VARMAX models and yu can check the performance of these models below


### Model Making

- I have built models from basic i.e AR(Auto Regression) till advanced i.e VARMAX(Vector AutoRegression with MovingAverage and Exogenous variable).

- At last I chose the model which gave less RMSE(Root mean squared error).

- I have plotted the predictions against the test and train data for each model just to check the performance of the models. Please find it below:

- AutoRegression (AR) model :

![AR](https://github.com/Pratik872/TimeSeries/blob/main/Sales%20Forecasting/readme%20resources/AR.png)

- MovingAverage (MA) model :

![MA](https://github.com/Pratik872/TimeSeries/blob/main/Sales%20Forecasting/readme%20resources/MA.png)

- AutoRegression with MovingAverage model (ARMA): 

![ARMA](https://github.com/Pratik872/TimeSeries/blob/main/Sales%20Forecasting/readme%20resources/ARMA.png)

- AutoRegression with MovingAverage with 'Promo' as exogenous variable (ARMAX): 

![ARMAX](https://github.com/Pratik872/TimeSeries/blob/main/Sales%20Forecasting/readme%20resources/ARMAX.png)

- Vector AutoRegression (VAR) : 

![VAR](https://github.com/Pratik872/TimeSeries/blob/main/Sales%20Forecasting/readme%20resources/VAR.png)

- Vector AutoRegression with MovingAverage (VARMA):

![VARMA](https://github.com/Pratik872/TimeSeries/blob/main/Sales%20Forecasting/readme%20resources/VARMA.png)

- Vector AutoRegression with MovingAverage with 'Promo' as exogenous variable (VARMAX) :

![VARMAX](https://github.com/Pratik872/TimeSeries/blob/main/Sales%20Forecasting/readme%20resources/VARMAX.png)


### Metrics

- I have used 'RMSE' i.e Root Mean Squared Error as my metric for these models. We can use other metrics also

### Results

![Results](https://github.com/Pratik872/TimeSeries/blob/main/Sales%20Forecasting/readme%20resources/results.JPG)

- So here you can see that the performance of the models are different accordingly. I have used the p,q values as per my system's computational capacity. You can also increase the values and get better results than this. I have mentioned the results in below section.


### DATA SOURCE
- [Rossmann Drugs Dataset](https://www.kaggle.com/c/rossmann-store-sales)

### Notebook
- [Sales Forecasting](https://github.com/Pratik872/TimeSeries/blob/main/Sales%20Forecasting/Sales_forecasting_Pratik_Waghmare.ipynb)

### Built with 🛠️
- Packages/Repo : Pandas,Numpy,Seaborn,Matplotlib,Sklearn,Git,Statsmodels

- Dataset : Kaggle

- Coded on : Jupter Notebook (modelling), VSCode