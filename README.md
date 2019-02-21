# Rossmann_Store_Sales
Forecast sales using store, promotion, and competitor data

This project is based on a Kaggle Prediction competition: https://www.kaggle.com/c/rossmann-store-sales

## Overview
Rossmann operates over 3,000 drug stores in 7 European countries. Currently, Rossmann store managers are tasked with predicting their daily sales for up to six weeks in advance. Store sales are influenced by many factors, including promotions, competition, school and state holidays, seasonality, and locality. With thousands of individual managers predicting sales based on their unique circumstances, the accuracy of results can be quite varied.

In their first Kaggle competition, Rossmann is challenging competition participants to predict 6 weeks of daily sales for 1,115 stores located across Germany. Reliable sales forecasts enable store managers to create effective staff schedules that increase productivity and motivation. By helping Rossmann create a robust prediction model, we will help store managers stay focused on what’s most important to them: their customers and their teams! 

##
Before building a model I started with data cleaning and performed following steps to setup the model:
#### 1. Dropped duplicates from Train, Test, and Store tables
#### 2. Performed outlier detection on sales in train data set and removed the 0.4% extreme outliers
#### 3. Dealt with missing values in tables:
##### o Test table – If the values are missing in “open” column I am assuming that the store was open, so replacing the null values in “open” column with 1.
##### o Store table:
###### ▪ Replaced NAs in the 'CompetitionOpenSinceYear' with Median (2010)
###### ▪ Replaced NAs in the 'CompetitionOpenSinceMonth' with Median (8-Aug)
###### ▪ Replaced NAs in the 'CompetitionDistance' with Median (2325.0)
(The rationale behind replacing null values with median values - Given those fields associated with actual values of competitive distance indicating there is at least one competitor nearby)
###### ▪ I filled the 544 NAs in the 'Promo2SinceWeek'/'Promo2SinceYear'/'PromoInterval' columns with 0s, which indicates 'no promotion going on'
#### 4. I only use the data of Sales > 0 and Open is 1. I am using when there are sales and the store is open to train our model
#### 5. Converted categorical features into dummy variables
