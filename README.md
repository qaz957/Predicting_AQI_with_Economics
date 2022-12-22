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
 
Taking Median AQI by Year and Industry Employment by County. 
 
Linear Regression Model testing significance of Industry on Air Quality Index 

We plan to test a few other models as well, but the goal is to find a model that can consistently predict air quality changes based on inudstry growth.

### Dashboard

We will be utilizing Tableau to show how or raw data has been transformed to information that can be easily visualized and understood by someone with no background knowledge on the subject. 

#### Draft of how the final vizualization of data will be displayed showing AQI and Industry Data
![Visualization](https://github.com/qaz957/Team_Zephyr/blob/main/Dashboard/Tableau%20Story.png)
