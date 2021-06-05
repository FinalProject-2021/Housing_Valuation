# Housing_Valuation

## Project Proposal

How much is my home worth? Is a home I'm considering fairly priced? 

In this project we propose to provide guidance to interested homebuyers and homesellers regarding home prices in the state of Florida.

We will gather a variety of indicators from diverse data sets and create a machine learning model that evaluates home prices based upon averages in particular zip codes. 

The machine learning algorithm will grade zip codes by the sales potential of homes on the market in that area.

Key indicators we will consider for incorporation into the model include: 
1. price per square foot
2. crime statistics
3. school ratings data
4. home ownership affordability
5. rental afforability
6. home prices
7. home sales 
8. housing starts
9. housing supply
10. mortgage originations
11. FHA originations
12. mortgage delinquency
13. seriously delinquent mortgages
14. change in aggregate prices
15. home ownership rate
16. underwater borrowers
17. foreclosure auctions

We planned to include 8-10 indicators after we've thoroughly investigated the availability of detailed data (see below for initial data sources).

We will create an interactive representation where users can search for a zip code and explore several visuals that give users the ability to evaluate whether their expectations are evidence-supported.

## Tools

We plan to use Python for data cleaning, JSON or a csv to store indicator data, a Flask application to generate the user's query response, and select visualization tools (perhaps Tableau, but Plotly if needed). We will deploy the project to Heroku.

*Note: After working with the project, we found that deploying only to Tableau suited the needs of the visualizations.

## Data Sources & Helpful Links
* [UCI ML Repo - Real estate valuation](https://archive.ics.uci.edu/ml/datasets/Real+estate+valuation+data+set)
* [Federal Housing Finance Agency - House Price Index](https://www.fhfa.gov/DataTools/Downloads/Pages/House-Price-Index-Datasets.aspx)
* [Rocket Mortgage - A Guide To Housing Market Indicators](https://www.rocketmortgage.com/learn/guide-to-housing-market-indicators)
* [Kaggle - Zillow Prize](https://www.kaggle.com/c/zillow-prize-1/data)
* [40 Real Estate APIs: Zillow, Trulia, Walk Score](https://www.programmableweb.com/news/40-real-estate-apis-zillow-trulia-walk-score/2012/02/15)
* [Trading Economics Housing Indexes](https://tradingeconomics.com/united-states/building-permits)
* [Housing Patterns - US Census](https://www.census.gov/data/tables/time-series/demo/housing-patterns/housing-patterns-tables.html)
* [Interest Rates by Year](https://data.worldbank.org/indicator/FR.INR.RINR?locations=US)
* [Property Data](https://api.developer.attomdata.com/home)
* [School Data](http://www.fldoe.org/accountability/accountability-reporting/school-grades/)
* [Census Data](https://censusreporter.org)

## Current Data Sets in Hand
* Sales Count/Zip Code - ATTOM
* Median Sales Price/Zip Code - ATTOM
* Property Tax/Zip Code - ATTOM
* Owner Occupied/Zip Code - ATTOM
* Renter Occupied/Zip Code - ATTOM
* Total Vacant/Zip Code - ATTOM
* Total Dwellings/Zip Code - ATTOM
* Rent Prices for studio, 1bed, 2bed, 3bed, 4bed/Zip Code - ATTOM
* Expense Index/Zip Code - Index 100 is nation average - ATTOM
* Average Commute/Zip Code - ATTOM
* Crime Index/Zip Code - Index 100 is nation average - ATTOM
* School Grades/Zip Code - Fl Dept of Ed
* Median Household Income/Zip Code - Census
* Average Mortgage Rate/National - ATTOM
* FHA Loan Originations / Zip Code - HUD.gov (# of mortgages in each zip in FL over 2-year period through 2019)

## Calculation Sources
* Home Affordability Calculation - [National Association of Realtors](https://www.nar.realtor/research-and-statistics/housing-statistics/housing-affordability-index/methodology)
* Mortgage Payment Calculation - [Medium.com](https://medium.com/personal-finance-analytics/mortgage-calculator-python-code-94d976d25a27) 

---

## Project Report

### Process
We began by extracting data through the sites listed under Data Sources. This required a variety of approaches, including repeated calls from ATTOM Data's API, from which we harvested a great deal of data.

Most of the data was available by Florida zip code for 2019 and 2020. Some of the data was available for 2021. Some data was annual, such as mobility rate and median household income, while other data was available for each month.

The data was cleaned in jupyter notebook and compliled into a SQLite database. This resulted in a total of 8 tables that can be joined on zip code, year, & month. We then used a SQL pull to join and filter this data into a single csv file for consumption by machine learning and Tableau. This final csv is organized by zip code (Florida only), month, and year with over 24,000 records, and 29 columns.

The SQL work involved cross joins and unions of these various files:
```
                     FROM zipcode AS zip
                        CROSS JOIN (SELECT 2019 AS year UNION SELECT 2020 AS year UNION SELECT 2021 AS year) AS y
                        CROSS JOIN (SELECT 1 AS month UNION SELECT 2 AS month UNION SELECT 3 AS month
                                UNION SELECT 4 AS month UNION SELECT 5 AS month UNION SELECT 6 AS month
                                UNION SELECT 7 AS month UNION SELECT 8 AS month UNION SELECT 9 AS month
                                UNION SELECT 10 AS month UNION SELECT 11 AS month UNION SELECT 12 AS month) AS m
                        LEFT JOIN fha_loans AS fha ON zip.zipcode = fha.zipcode AND y.year = fha.year AND m.month = fha.month
```

We began experimenting with different machine learning algorithms. We used a Lazy Predictor algorithm to identify the strongest approaches, which included approaches not covered in the class. In addition, we used KNN and SVM approaches. Our initial conception was to offer a classification algorithm that would indicate with relatively high certainty whether median house prices in individual zip codes would rise, fall, or stay the same in upcoming months. However, as the results section indicates, developing a successful prediction algorithm was challenging, at least with the data we were able to find.

We also employed a number of approaches to time series modelling, including a random forest regressor combined with a Pandas melt function, ARIMA, and VAR (vector autoregressive modelling). VAR has the advantage of allowing the use of multiple features.

We also experimented with narrower and wider feature sets and with different labels. We consistently employed the following features: Zip Code, Year, Month, Median Sale Price, Total # of Sales, count of FHA loans, a calculated home affordability score (based on median mortgage payments' relation to 30% of median household income), and rent affordability (similar but with rent). [NAME OF FEATURES IN ALLMODELS], and we experimented with including other features, such as median household income, rent price, property taxes, # of people employed full time, # of unemployed, median loan amount, term, and interest rate.

In addition, we experimented with different label values, including predicting median home price and categorical predictions of the change in median home price over a 1 and 3 month period were positive or negative. We also experimented with including calculated 1, 2, and 3 month changes in median home price in the features.

---
### Results of Machine Learning

#### Classification vs. Regression
Initially we ran LazyPredict to test a multitude of regression and classification algorithms to see which would potentially provide the best results.  Random Forest Classification received the best results from classifiers but even after adjusting parameters the best results we could get is 65% accuracy. After experimenting with different features, SVM and KNN maxed out between 63% and 64%.  Therefore we decided regression models would work best, although since many of the predictions would be based on a percentage of home price, a high accuracy rate likely reflected the relative stability of house prices month over month.  We compared three different models: 2 autoregressive models ARIMA & VAR and 1 standard regression model Random Forest Regressor.  We trained each model on all of 2019-2020 data and tested on Jan-Mar 2021.  We ran the models to forecast Total Sales, FHA Loans, Median Sale Price.  We forcasted April 2021 with both ARIMA & VAR, but random forest regressor is unable to forecast past the testing data.

#### Time Series Analysis
Because our data set only went back two years, we needed to find a way to do a time series prediction with a small data set, so we decided to calculate the sale price changes from month to month per ZIP code so we could train on many month-to-month deltas. A Google search for a solution led to a blog post by data scientist Mario Filho (https://www.mariofilho.com/how-to-predict-multiple-time-series-with-scikit-learn-with-sales-forecasting-example/), who suggested using the Pandas melt function to shape the dataset from a wide to a long form with many rows.  Our data set was already in long form, so the melt function wasn’t necessary, but his use of sliding window validation did prove useful.

First, we recreated his methods precisely with our own data to get a feel for the sort of results we could expect: we ran a Random Forest Regressor model using only the sale price as our value and the ZIP codes and months as identifiers, the root mean squared logarithmic error (RMSLE) seemed to bottom out at .21 after six window shifts. This gave us an R2 score of 87% in the test phase. However, we didn’t want to use only sales prices to predict sales prices.
We re-ran the RFR model with 16 features, including Mobility_Rate,       Expense_Index, Crime_Index, Total_Vacant, Total_Dwellings, Total_Sales, FHA_Count, Home_Affordability, Rent_Affordability, Sale_Price,Last_Month_Price, Last_Month_Diff, Last_2Month_Price, Last_2Month_Diff, Last_3Month_Price, and Last_3Month_Diff.

The training score from this model was 99% and testing score was 97%.

We then tried different combinations of X values by removing X values one at a time or starting from scratch and adding one at a time. None of the values had as much impact on the final training and testing R2 scores as the calculated differences from the last months (Last_Month_Diff, etc.) I checked the math against the original data to make sure it was calculating correctly. Training and testing scores without the calculated differences as X values hovered around 97% for the training and 80% for the test. This made me concerned that I had overfitted the model, but because the scores from the second RF model were good, I wanted to use it to try to predict.

We ran ARIMA tests to predict house prices, total house sales, and # of FGA loans in the state and by zip code. We ran autocorrelation tests to gauge the statistical significance of different time lags. For house prices, we ended up settling on 5 months of lag, which had significance above a reasonable threshold.  The results are detailed in Tableau, but they include predictions like the following: For April 2021, our ARIMA model predicts the mean home sales price across Florida to be $287,972.77.
