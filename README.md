# Team Zephyr Capstone Project

Google Sheets Presentation:
https://docs.google.com/presentation/d/1IWXcgVthBSGJK0BXKmbCIxYhjlpTGMZ_kiGkcdTwyrs/edit#slide=id.p




## Predict Changes In Air Quality Based On Industry

### Abstract

This project looks at data on air quality by county and the growth of certain industries over time to determine if there is a correlation between certain industries growth and changes in air quality by county. Air Quality can be tied to many socioeconomic factors, and so it is important to track the AQI to determine which industries have a sizeable impact on those factors. 

### Questions to answer:

Can the Air Quality of a U.S. county be determined based on the industry employment percentages? (in the years 2010, 2015, 2019)

Can the data model determine which industries are most impactful towards air quality?

What are the limitations of decision tree modelling for determining trends and predictive analytics?

### Data

Economic data from the Census Bureau has been taken to identify employment in certain areas by county. The air quality index comes from the EPA and is isolated to 3 years, 2010, 2015 and 2019 to use for training and testing. The year 2020 is likely a significantly different year for employment and air quality so it was not included in the analysis. After cleaning the data and joining it to the air quality data, there are 477 counties in the US that have complete data that can be trained on. With outliers removed for each year, there are approximately 380 rows remaining in each dataset.

- Data from EPA was scraped with this script and joined into one data frame

      # Def current year variable
      year = 2022

      # For loop iterating through the files, getting Median AQI for every year per county
      for i in range(int(year-1979)):

        url=f"https://aqs.epa.gov/aqsweb/airdata/annual_aqi_by_county_{year}.zip"
        rawAQI_df=pd.read_csv(url)

        AQI_df = rawAQI_df[["State", "County", "Median AQI"]]
        AQI_df.rename(columns = {"Median AQI" : f"{year}_Median_AQI"}, inplace = True)

        # Continuously join data to make one data frame
        join_df = pd.merge(join_df, AQI_df, how="left", on = ["State", "County"])

        year = year-1
        
- Census data had to be collected manually

- EPA and Census data had to be reformatted so that it could be joined together

### Database

The data is stored in an AWS S3 bucket and linked to PGAdmin.

<img   src="https://github.com/qaz957/Team_Zephyr/blob/main/Images/DB_Details_AWS.JPG"  alt="AWS RDS"  title="AWS RDS" style="display: inline-block; margin: 0 auto; max-width: 300px">

<img   src="https://github.com/qaz957/Team_Zephyr/blob/main/Images/S3_Files.JPG"  alt="S3 Files"  title="S3 Files" style="display: inline-block; margin: 0 auto; max-width: 300px">

<img   src="https://github.com/qaz957/Team_Zephyr/blob/main/Images/Tbl_Logins.JPG"  alt="TBL"  title="TBL" style="display: inline-block; margin: 0 auto; max-width: 300px">

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

- The final Accuracy scores vary from ~78-90% which is a considerable improvement from the previous model.

- Additional evaluation metrics (Precision, Recall, F1 Score) are included to evaluate the utility of the classification method. 

Improvement of Decision Tree Model:

- Future work for this project could include the use of ensemble methods (bagging and boosting) to improve the model's predictive power.

- Also, addressing any imbalance in the data with imbalanced learning methods may improve evaluation metrics.

- Additionally, tree pruning can be used to remove the furthest down leaves and branches sequentially to check for any increase in the evaluation metrics.

### Dashboard


Tableau Dashboard:
Deliverable 2:
https://public.tableau.com/app/profile/andrew.eamonn.walters/viz/DashboardForSegment2/DashboardSeg2?publish=yes

Tableau Dashboard:
Deliverable 3:
https://public.tableau.com/app/profile/andrew.eamonn.walters/viz/DashboardForSegment3/InteractiveofAQI?publish=yes

Final Dashboard:
https://public.tableau.com/app/profile/andrew.eamonn.walters/viz/DashboardFinal_16734784157570/InteractiveofAQI?publish=yes

Google Docs Dashboard: 
https://docs.google.com/presentation/d/1_gqyLhzalK05Y0FORbXTnmVFL3UMtM-kZME660mvdj8/edit#slide=id.p


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
