# Team Zephyr Capstone Project

## Predict Changes In Air Quality Based On Industry

### Abstract

With this project we plan to look at data on air quality by county and the growth of certain industries over time to determine if there is a correlation between certain industries growth and changes in air quality by county. Air Quality can be tied to many socioeconomic factors, and we felt it was important to track the AQI to determine what industries have a sizeable impact on those factors. 

### Questions we hope to answer:

What industries, if any, have a noticeable impact on Air Quality?

What have the general trends over 10 years been within certain industries?

What have the general trends over 10 years been in Air Quality?

### Data

We have taken economic data from the Census Bureau to identify employment in certain areas by county. The air quality index we are using comes from the EPA and we isolated 3 years, 2010, 2015 and 2019 to use for training and testing. We felt that 2020 was probably going to be an outlier year for employment and air quality so we opted not to use it. After cleaning the data and joining it to the air quality data, we are left with 477 counties in the US that have complete data that we can train from.

### Database

We stored our data in an AWS S3 bucket and linked it to PGAdmin.

### Machine Learning Module: 
 
Predicting Median AQI by Year (Outcome) with Industry Employment by County (Predictors). 
 
Linear Regression Model: testing significance of Industry on Air Quality Index 

- Multiple linear regression was chosen since the initial form of the data has quantitative values for each predictor variable (employment percentage within a county) and a continuous numerical outcome variable (Median_AQI for a county)

- During the training of the model, the dataset was split into and 80% training set and a 20% testing set using Python's train_test_split function from the skearn.model_selection library.

- The initial model had very low predictive value (R^2 ~ 0.01) with 1% of the variation of median AQI being explained by variation in the employment percentage by industries.

- To improve the model we decided to reduce features by evaluating VIF (variance inflation factor) scores that measures multicollinearity. We removed many of the features that had calculated VIF scores above 10 and the predictive power of the model did not improve. 

- We recognized that we had yet to remove outliers from the initial data so the next step was to remove percentages that were more than 1.8(IQR) above the 75th percentile or were 1.8(IQR) below the 25th percentile. The R^2 value did not improve after the removal of outliers.

Binning Values:

- Instead of predicting a continous outcome variable we decided to bin the AQI values into 4 separate categories and attempt to use classification models to predict how the industries affect which category the AQI fits in for that county.

Decision Tree: Classifying Median_AQI based on industry employment percentages

- This classifier has been used as our initial attempt at determining if a classification method can be used for our binned categorical values. 

- An initial review has given us Accuracy scores of ~70-80% which is a considerable improvement from our previous model.

- Additional evaluation metrics (Precision, Recall, Specificity) will be explored and the use of ensemble methods to improve our model's power will be included as well. 


### Dashboard
