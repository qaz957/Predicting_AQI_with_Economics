# Team Zephyr Capstone Project

Tableau Dashboard:
Deliverable 2:
https://public.tableau.com/app/profile/andrew.eamonn.walters/viz/DashboardForSegment2/DashboardSeg2?publish=yes

Tableau Dashboard:
Deliverable 3:
https://public.tableau.com/app/profile/andrew.eamonn.walters/viz/DashboardForSegment3/InteractiveofAQI?publish=yes

Google Docs Dashboard: 
https://docs.google.com/presentation/d/1_gqyLhzalK05Y0FORbXTnmVFL3UMtM-kZME660mvdj8/edit#slide=id.p

Google Sheets Presentation:
https://docs.google.com/presentation/d/1IWXcgVthBSGJK0BXKmbCIxYhjlpTGMZ_kiGkcdTwyrs/edit#slide=id.p




## Predict Changes In Air Quality Based On Industry

### Abstract

This project looks at data on air quality by county and the growth of certain industries over time to determine if there is a correlation between certain industries growth and changes in air quality by county. Air Quality can be tied to many socioeconomic factors, and so it is important to track the AQI to determine which industries have a sizeable impact on those factors. 

### Questions to answer:

What industries, if any, have a noticeable impact on Air Quality?

What have the general trends over 10 years been within certain industries?

What have the general trends over 10 years been in Air Quality?

### Data

Economic data from the Census Bureau has been taken to identify employment in certain areas by county. The air quality index comes from the EPA and is isolated to 3 years, 2010, 2015 and 2019 to use for training and testing. The year 2020 is likely a significantly different year for employment and air quality so it was not included in the analysis. After cleaning the data and joining it to the air quality data, there are 477 counties in the US that have complete data that can be trained on. With outliers removed for each year, there are approximately 380 rows remaining in each dataset.

### Database

The data is stored in an AWS S3 bucket and linked to PGAdmin.

### Machine Learning Module: 
 
Predicting Median AQI by Year (Outcome) with Industry Employment by County (Predictors). 
 
Linear Regression Model: testing significance of Industry on Air Quality Index 

- Multiple linear regression was chosen since the initial form of the data has quantitative values for each predictor variable (employment percentage within a county) and a continuous numerical outcome variable (Median_AQI for a county)

- During the training of the model, the dataset was split into and 80% training set and a 20% testing set using Python's train_test_split function from the skearn.model_selection library.

- The initial model had very low predictive value (R^2 ~ 0.01) with 1% of the variation of median AQI being explained by variation in the employment percentage by industries.

- To improve the model we decided to reduce features by evaluating VIF (variance inflation factor) scores that measures multicollinearity. Features with VIF scores above 10 were removed and the predictive power of the model did not improve. 

- Outliers had not yet been removed from the datasets so the next step was to remove percentages that were more than 1.8(IQR) above the 75th percentile or were 1.8(IQR) below the 25th percentile. The R^2 value did not improve after the removal of outliers.

Binning Values:

- Instead of predicting a continous outcome variable the median AQI values are binned into 4 separate categories in the attempt to use classification models to predict how the industries affect which category the AQI fits in for each county.

Decision Tree: Classifying Median_AQI based on industry employment percentages

- This classifier has been used as our initial attempt at determining if a classification method can be used for our binned categorical values. 

- The final Accuracy scores vary from ~78-90% which is a considerable improvement from our previous model.

- Additional evaluation metrics (Precision, Recall, F1 Score) are included. 

- Future work for this project could include the use of ensemble methods (bagging and boosting) to improve the model's predictive power. 


### Dashboard

### Project Requirements
Libraries and code to run the project files:
- pip install numpy
- pip install pandas
- pip install sklearn
- from statsmodels.stats.outliers_influence import variance_inflation_factor
- from sklearn.model_selection import train_test_split
- from sklearn.tree import DecisionTreeClassifier
- from sklearn import metrics
- from sklearn.tree import export_text
- from sklearn.metrics import classification_report
