# Rossmann_Store_Sales
Forecast sales using store, promotion, and competitor data

This project is based on a Kaggle sales prediction competition: https://www.kaggle.com/c/rossmann-store-sales

## Overview
Rossmann operates over 3,000 drug stores in 7 European countries. Currently, Rossmann store managers are tasked with predicting their daily sales for up to six weeks in advance. Store sales are influenced by many factors, including promotions, competition, school and state holidays, seasonality, and locality. With thousands of individual managers predicting sales based on their unique circumstances, the accuracy of results can be quite varied.

In their first Kaggle competition, Rossmann is challenging competition participants to predict 6 weeks of daily sales for 1,115 stores located across Germany. Reliable sales forecasts enable store managers to create effective staff schedules that increase productivity and motivation. By helping Rossmann create a robust prediction model, we will help store managers stay focused on what’s most important to them: their customers and their teams! 

## Data cleaning process
Before building a model I started with data cleaning and performed following steps to setup the model:
1. Dropped duplicates from Train, Test, and Store tables
2. Performed outlier detection on sales in train data set and removed the 0.4% extreme outliers
3. Dealt with missing values in tables:
o Test table – If the values were missing in “open” column I assumed that the store was open, so replacing the null values in “open” column with 1.
o Store table:
▪ Replaced NAs in the 'CompetitionOpenSinceYear' with Median (2010)
▪ Replaced NAs in the 'CompetitionOpenSinceMonth' with Median (8-Aug)
▪ Replaced NAs in the 'CompetitionDistance' with Median (2325.0)
(The rationale behind replacing null values with median values - Given those fields associated with actual values of competitive distance indicating there is at least one competitor nearby)
▪ I filled the 544 NAs in the 'Promo2SinceWeek'/'Promo2SinceYear'/'PromoInterval' columns with 0s, which indicates 'no promotion going on'
4. I only used the data of Sales > 0 and Open is 1. I also used the data when there are sales and the store is open to train our model
5. Converted categorical features into dummy variables

## Feature engineering process
1.	Created features related to Date
o	Year
o	Month
o	Week Number
o	Quarter
o	Year Month
o	Weekday
o	Weekend Flag

2.	Features related to Promo (Daily Promotions)
o	I created promotion interval using following breakdown in order to compare the months with the current month so that I could capture whether there is an interval periodic promotion going on or not on each day.

	Jan, Apr, Jul, Oct = 1
	Feb, May, Aug, Nov = 2
	Mar, Jun, Sept, Dec = 3 

3.	Features related to Promo2 (Interval Periodic Promotions)
o	Whether the promotion is ongoing or not

4.	Features related to Competition:
o	Competition open - Whether the competition is open or not
To calculate whether competition is open or not, I compared competition since year with actual year/months. Actual < open. If comp is open then I also calculated the comp. open months

5.	Modified the feature 'StateHoliday' to binary 

6.	Features related to Sales 
o	Calculated Average quarterly sales
o	Calculated Monthly Total Sales

7.	Features related to Customers
o	Calculated Monthly Total Number of Customers
