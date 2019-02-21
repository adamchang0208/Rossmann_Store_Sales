# Rossmann_Store_Sales
Forecast sales using store, promotion, and competitor data (with Linear Regression, Random Forest, and XGBoost models)

This project is based on a Kaggle sales prediction competition: https://www.kaggle.com/c/rossmann-store-sales

## Overview
Rossmann operates over 3,000 drug stores in 7 European countries. Currently, Rossmann store managers are tasked with predicting their daily sales for up to six weeks in advance. Store sales are influenced by many factors, including promotions, competition, school and state holidays, seasonality, and locality. With thousands of individual managers predicting sales based on their unique circumstances, the accuracy of results can be quite varied.

In their first Kaggle competition, Rossmann is challenging competition participants to predict 6 weeks of daily sales for 1,115 stores located across Germany. Reliable sales forecasts enable store managers to create effective staff schedules that increase productivity and motivation. By helping Rossmann create a robust prediction model, we will help store managers stay focused on what’s most important to them: their customers and their teams! 

## Data cleaning process
Before building a model I started with data cleaning and performed following steps to setup the model:
#### 1. Dropped duplicates from Train, Test, and Store tables
#### 2. Performed outlier detection on sales in train data set and removed the 0.4% extreme outliers
#### 3. Dealt with missing values in tables:
##### o Test table – If the values were missing in “open” column I assumed that the store was open, so replacing the null values in “open” column with 1.
##### o Store table:
##### ▪ Replaced NAs in the 'CompetitionOpenSinceYear' with Median (2010)
##### ▪ Replaced NAs in the 'CompetitionOpenSinceMonth' with Median (8-Aug)
##### ▪ Replaced NAs in the 'CompetitionDistance' with Median (2325.0)
(The rationale behind replacing null values with median values - Given those fields associated with actual values of competitive distance indicating there is at least one competitor nearby)
##### ▪ I filled the 544 NAs in the 'Promo2SinceWeek'/'Promo2SinceYear'/'PromoInterval' columns with 0s, which indicates 'no promotion going on'
#### 4. I only used the data of Sales > 0 and Open is 1. I also used the data when there are sales and the store is open to train our model
#### 5. Converted categorical features into dummy variables

## Feature engineering process
#### 1.	Created features related to Date
##### o	Year
##### o	Month
##### o	Week Number
##### o	Quarter
##### o	Year Month
##### o	Weekday
##### o	Weekend Flag

#### 2.	Features related to Promo (Daily Promotions)
##### o	I created promotion interval using following breakdown in order to compare the months with the current month so that I could capture whether there is an interval periodic promotion going on or not on each day.

##### 	Jan, Apr, Jul, Oct = 1
##### 	Feb, May, Aug, Nov = 2
##### 	Mar, Jun, Sept, Dec = 3 

#### 3.	Features related to Promo2 (Interval Periodic Promotions)
##### o	Whether the promotion is ongoing or not

#### 4.	Features related to Competition:
##### o	Competition open - Whether the competition is open or not
To calculate whether competition is open or not, I compared competition since year with actual year/months. Actual < open. If comp is open then I also calculated the comp. open months

#### 5.	Modified the feature 'StateHoliday' to binary 

#### 6.	Features related to Sales 
##### o	Calculated Average quarterly sales
##### o	Calculated Monthly Total Sales

#### 7.	Features related to Customers
##### o	Calculated Monthly Total Number of Customers


## Summary of Analysis & Findings from All the models

#### Model 1: Linear Regression
##### 1.	Split the data into target and predictor variables
##### 2.	Fit the linear model with all features
##### 3.	Predict the Sales for the test data set								
		
#### Model 2: Linear Regression (with selected features)
##### 1.	Split the data into target and predictor variables
##### 2.	Dropped features - CompetitionDistance' and 'NumOfCompetitionOpenMonths because for stores that don’t have competition at certain time, the competition distance and number of competition open months will be NAs. 
##### 3.	Fit the linear model
##### 4.	Predict the Sales for the test data set								

Linear regression with feature selection performs slightly better than the model with all features

#### Model 3: Random Forest
##### 1.	Split the data into target and predictor variables
##### 2.	Use grid search to perform hyper-parameter tuning
##### 3.	Fit the random forest model with all features
##### 4.	Predict the Sales for the test data set									

#### Model 4: Random Forest (with Feature Selection)
##### 1.	Split the data into target and predictor variables
##### 2.	Use grid search to perform hyper-parameter tuning
##### 3.	Plot feature importance for random forest model and consider top 8 features 
##### 4.	Fit the random forest on selected features
##### 5.	Predict the Sales for the test data set	

Random Forest with all features performs better than the model with feature selection

#### Model 5: XGBoost (Without Feature Selection)	
##### 1.	Split the data into target and predictor variables
##### 2.	Use grid search to perform hyper-parameter tuning
##### 3.	Fit the XGBoost model based on hyper-parameter settings
##### 4.	Predict the Sales for the test data set	

#### Model 6: XGBoost (With Feature Selection)
##### 1.	Split the data into target and predictor variables
##### 2.	Use grid search to perform hyper-parameter tuning
##### 3.	Fit the XGBoost model based on hyper-parameter settings on selected features
##### 4.	Plot feature importance for XGB boost model and consider top 25 features 
##### 5.	Fit the XGBoost model on selected features using hyper-parameter settings
##### 6.	Predict the Sales for the test data set

#### Model 7: XGBoost (With Feature Selection)
##### 1.	Split the data into target and predictor variables
##### 2.	Use grid search to perform hyper-parameter tuning
##### 3.	Fit the XGBoost model based on hyper-parameter settings on selected features
##### 4.	Plot feature importance for XGB boost model and consider top 10 features 
##### 5.	Fit the XGBoost model on selected features using hyper-parameter settings
##### 6.	Predict the Sales for the test data set								

XGBoost with all features performs better slight better at private leaderboard however the model with feature selection better at public leaderboard

## Conclusion
As I compared hyperparameters across different models, I found that there was a point when changing our features for XGBoost was not helping to increase RMSE. It seems that it is better to have less complex models that can be communicated easily than more complex with many hyperparameters that is harder to communicate. The best model was Random Forest with all features that we engineered.

Random forest achieves a lower test error solely by variance reduction. I set the hyper-parameters using grid search cross validation. The reason Random Forest is performing well without feature selection is because it performs implicit feature selection and provide a pretty good indicator of feature importance. It handles binary features, categorical features, and numerical features without any need for scaling. 

