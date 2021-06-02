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
AVAIL BY REGION? 8. housing starts
9. housing supply
10. mortgage originations
11. FHA originations
12. mortgage delinquency
13. seriously delinquent mortgages
14. change in aggregate prices
AVAIL BY REGION? 15. home ownership rate
LEAVE OUT? 16. underwater borrowers
LEAVE OUT? 17. foreclosure auctions

We hope to include 8-10 indicators after we've thoroughly investigated the availability of detailed data (see below for initial data sources).

We will create an interactive representation where users can search for a zip code and explore several visuals that give users the ability to evaluate whether their expectations are evidence-supported.

## Tools

We plan to use Python for data cleaning, JSON or a csv to store indicator data, a Flask application to generate the user's query response, and select visualization tools (perhaps Tableau, but Plotly if needed). We will deploy the project to Heroku.

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

## Current Data Sets in Hand
* Sales Count/Zip Code - [ATTOM](https://api.developer.attomdata.com/home)  
* Median Sales Price/Zip Code - [ATTOM](https://api.developer.attomdata.com/home)
* School Grades/Zip Code - [Fl Dept of Ed](http://www.fldoe.org/accountability/accountability-reporting/school-grades/)
* Crime / Zip Code - [ATTOM](https://api.developer.attomdata.com/home)
* FHA Loan Originations / Zip Code - [HUD.gov](https://www.hud.gov/program_offices/housing/rmra/oe/rpts/sfsnap/sfsnap) (# of mortgages in each zip in FL over 2-year period through 2019) 

## Looking for
* Historical Avg. Mortgage Rate (monthly avg?)/FL?   
* Home ownership rate?   
* Census -- Median Household Income, Pop Growth Data?  
* Rental Data - [RapidAPI.com](https://rapidapi.com/moneals/api/rent-estimate)  

## Calculation Sources
* Home Affordability Calculation - [National Association of Realtors](https://www.nar.realtor/research-and-statistics/housing-statistics/housing-affordability-index/methodology)
* Mortgage Paymeny Calculation - [Medium.com](https://medium.com/personal-finance-analytics/mortgage-calculator-python-code-94d976d25a27)

## Plan
* Separate Python notebooks for each data  
* SQLite DB  
* Zip Code (5 digit) as index (INT), with year (INT) and month (INT) as available  
* Tuesday: Try to get data into DB  

## Project Report
We began by extracting data through the sites listed under Data Sources. This required a variety of approaches, including repeated calls from ATTOM Data's API, from which we harvested a great deal of data.

All of the data was available by Florida zip code for 2019 and 2020. Some of the data was available for 2021. Some data was annual, such as mobility rate and median household income, while other data was available for each month.

We used extensive SQL pulls to join and filter this data into a SQLite database. This resulted in a database organized by zip code (Florida only), month, and year with over 21,000 records, and there were 29 columns.

We began experimenting with different machine learning algorithms. We used [EXPLAIN MELT APPROACH], a Lazy Predictor algorithm to identify the strongest approaches [EXPLAIN FURTHER?], which included approaches not covered in the class. In addition, we used KNN and SVM approaches.

We also experimented with narrower and wider feature sets and with different labels. We consistently employed the following features: Zip Code, Year, Month, Median Sale Price, Total # of Sales, count of FHA loans, a calculated home affordability score (based on median mortgage payments' relation to 30% of median household income), and rent affordability (similar but with rent). [NAME OF FEATURES IN ALLMODELS], and we experimented with including other features, such as median household income, rent price, property taxes, # of people employed full time, # of unemployed, median loan amount, term, and interest rate [OTHERS?].

In addition, we experimented with different label values, including predicting median home price and categorical predictions of the change in median home price over a 1 and 3 month period were positive or negative. We also experimented with including calculated 1, 2, and 3 month changes in median home price in the features.

The results ...
