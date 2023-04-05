# Predicting market rent in the Bay Area using Craigslist rental listings
---
It’s easy to know when something is on sale if you know it is 30% off the original price. But for rental listings on craigslist, what is the original “market price” for a given apartment or house in that particular neighborhood? How do you know if you’re getting a good deal on rent or if you’re paying more than the market rate for your place?


This project aims to answer these questions with data from Craigslist rental ads in the Bay Area. Data was scraped every 3 days from active listings between Aug to Oct of 2022. The metros of interest are San Francisco, Peninsula, East Bay in the Bay Area.

<img src="https://github.com/melissavhan/CapstoneTwoProject/blob/96c0464b62a9355c4c37f61c78c83c1337888718/references/Simple_bay_area_metros.JPG?raw=true" alt="Simple_bay_area_metros.JPG" width="500"/>
<p style="text-align: center;">Map of Bay Area metros within the scope of this project</p>


## 1. Web scraping using BeautifulSoup and Playwright
---
[HTML parsing Notebook](https://github.com/melissavhan/CapstoneTwoProject/blob/e38988da9187f9000b49eb6948fd67ea25cd89af/notebooks/Craigslist_Parsing_HTML.ipynb)

**Use BeautifulSoup and Playwright to scrape individual listing's HTML from the index pages of Craigslist for each metro. Crawl craigslist by neighborhood in each metro, and download the listing HTML for parsing. 
Extract features from HTML using Regex.**

**The features extracted from the Craigslist listings are:**
+ Title of the craigslist listing
+ Address
+ Neighborhood and city of the listing
+ Rent price
+ Number of bedrooms
+ Number of bathrooms
+ Area measured in Square foot
+ The body of the craigslist post which contains text describing the property


## 2. Data Cleaning
---
[Data Cleaning Notebook](https://github.com/melissavhan/CapstoneTwoProject/blob/e38988da9187f9000b49eb6948fd67ea25cd89af/notebooks/Craigslist_Data_Wrangling%20.ipynb)

**Market rent price prediction follows the following assumptions about the data:**
+ The property for rent is an entire property - that means not renting by room
+ The property is located in the Bay Area metros - San Francisco, Peninsula, East Bay
+ The rental price is quoted on a monthly basis
+ To make predictions, the following features must not be missing: 
    + Square footage
    + number of bedrooms
    + number of bathrooms
    + address

**The dataset was cleaned with these assumptions in mind:**
1. Remove duplicate listings
2. Data Type Correction and removal if the above assumptions were not met
3. Add Walk Score, Transit Score, Bike Score data based on address
4. Identify and impute missing values



## 3. EDA
---
[EDA Notebook](https://github.com/melissavhan/CapstoneTwoProject/blob/e38988da9187f9000b49eb6948fd67ea25cd89af/notebooks/Craigslist_EDA.ipynb)

**Exploration of the data and each of the features and their relationships. Creation of new features such as `Average price per square foot` for each neighborhood in each metro. Calculate correlations between features to guide feature engineering in the preprocessing step.**



<img src="https://github.com/melissavhan/CapstoneTwoProject/blob/d020e4074d3842598b70287428368e57d0fa9c23/references/sf_heatmap_white_background.jpg?raw=true" alt="sf_heatmap.png" width="700"/>

<img src="https://github.com/melissavhan/CapstoneTwoProject/blob/e97e1207572f8ac2e1ded7a3b45d208476e44f5b/references/bedroom_bathroom_combos.png?raw=true" alt="bedroom_bathroom_combos.png" width="500"/>

**Look at the distribution of listings by metros, neighborhoods and by price to see if there is any skewness:**

<img src="https://github.com/melissavhan/CapstoneTwoProject/blob/e97e1207572f8ac2e1ded7a3b45d208476e44f5b/references/price%20by%20neighborhood.png?raw=true" alt="price%20by%20neighborhood" width="900"/>






## 4 & 5. Preprocessing and Machine Learning Models
---
[Proprocessing and Modeling](https://github.com/melissavhan/CapstoneTwoProject/blob/e38988da9187f9000b49eb6948fd67ea25cd89af/notebooks/Craigslist_Preprocessing_Modeling_combined-nh_models.ipynb)

**Preprocessing steps**
+ Splitting the data into testing and training datasets
+ Handle missing values
+ Removing outliers
+ Feature engineering for categorical variables
+ Scaling numeric data

**Machine learning models were trained to predict the market rent price of a given property, based on the property’s craigslist listing.**
+ A baseline model using only neighborhood and square footage features - this is a back of the envelope type of estimate
+ Stochastic Gradient Descent Regressor
+ Random Forest Model Regressor
+ XGBoost
+ LGBM Regressor
+ Piecewise model (SGDR) by number of bedrooms
+ Piecewise model (SGDR) by neighborhood

**The best performing model was XGBoost with a Mean Absolute Percentage Error (MAPE) of 12%. The piecewise linear models also showed some interesting improvements to the SGDR model.**

**Feature exploration and interpretability of the models yielded insight for renters and property owners, with answers to questions such as how much more does one pay for a property with an in-unit laundry? Is there a price premium on properties that are dog friendly?**

## 6. Web App to test the linear model
---

Built a flask web app hosted in AWS Elastic Beanstalk so users can test the performance of the linear model on a property and see the rent components that make up the total predicted rent price. 

[Web app link](http://craigslistrentprediction-env.eba-bjf33puu.us-west-1.elasticbeanstalk.com/)
