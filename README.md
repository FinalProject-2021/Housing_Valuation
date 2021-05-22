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
* Sales Count/Zip Code ATTOM 
* Median Sales Price/Zip Code ATTOM 
* School Grades/Zip Code Fl Dept of Ed 
* Crime / Zip Code ATTOM 
* FHA Loan Originations / Zip Code FHA/HUD.gov (# of mortgages in each zip in FL over 2-year period through 2019) 

## Looking for
* Historical Avg. Mortgage Rate (monthly avg?)/FL? 
* Home ownership rate? 
* Census -- Median Household Income, Pop Growth Data? 
* Rental Data - [RapidAPI.com](https://rapidapi.com/moneals/api/rent-estimate)

##
* Home Affordability Calculation: what would be average cost/mo // what's median income?

## Plan
* Separate Python notebooks for each data
* SQLite DB
* Zip Code (5 digit) as index (INT), with year (INT) and month (INT) as available
* Tuesday: Try to get data into DB
