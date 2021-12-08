# Winter Resort Visitation Prediction

*Capstone Data Science Project / Flatiron Data Science Bootcamp*

By: [Lenore Perconti](https://github.com/lperconti)

![resort](Visuals/SkiResortBanner.png)

## Overview

In this project I look at three winter resorts and utilize weather and event data to predict visitation for each resort. 

### The Stakeholder / Business Problem

Winter Ski and Snowboard resorts are seasonal attractions that provide uphill transporation for recreational skiiers and snowboarders. Most resorts sell "season passes" and multi-day passes which allow the consumer to visit the resort on a day of their choosing, making forcasting for visitation difficult. 

Weather plays a significant role in a resort operations and can serve as an attractant or deterrant for visitation. If customers have the flexibility to only visit the resort on days that the weather is favorable though is another matter. There's a perception that 'powder days' consistantly attract big crowds, however holidays and weekends consistantly see large turnouts as well. 

Determining how many people on a given day will show up and ski or snowboard at a resort is important for these resorts. Management needs to plan resources and staffing accordingly. Anticipating too many visitors results in cutting labor at the last minute and potentially wasting perishable resources such as food. Anticipating too few visitors may result in understaffing, too few resources, and unsatisfactory conditions and service for customers. 

Larger winter resort entities such as VAIL have teams of business analysts and data scientists to help business anticipation and forcasting. Smaller resorts on the other hand, do not have these resources. In my informal interviewing I found resorts deploying visitation predictions based on historical visitation data, weather, local events and key individuals' years of experience and insight. One of the resorts I interviewed had an excel spreadsheet that would weigh different factors such as weather and date to generate a prediction that helped the resort budget for visitation and the actual visitatoin is "typically within 10 - 15 % variance" of the budgeted visits. 

## Project Goals
There are two main goals for this project:

- Produce daily visitation prediction models based on weather and calendar events for three different day-use resorts.
- Analyze the predictive power and therefore feesability using of weather and calendar events for ski resort daily visitation prediction.

The results of these models can be used to help business anticipation for day-use ski resorts and lay a foundatoin for future modeling and improvements.

## Data

To simplify my quest to prediciting visitation, I solicited visitation data from only day-visit resorts in the Pacific Northwest Region. This means these resorts do not have overnight accomodations at the resort. Three resorts volunteered daily visitation data for this project. Data was given on the condition that the specific resort not be identified publically. Therefore the resort name and any location-based information or potential identifying information associated with this dataset are withheld from this repository.

#### Resort 1 

**Resort 1** is close to a regionally large metropolitan area (approx 455,000 people). The resort's lift capacity is 9,980 riders/hour. 

Resort 1 generously gave four seasons' worth of daily visit totals in an excel document for use in this project. The daily visits in the given data were further broken down into daily season pass holder visits, day ticket visits, day visits, and night vists. 

A couple notes on sourcing the data for this particular ski area: 

* `is_school_out` indicates if the public school district did not have class on a day that normally would have school (weekday). These included Winter Break, MLK Day, Presidents' Day, and Spring Break. Weekends (Sundays and Mondays) surrounding school closures such as Thanksgiving, Presidents Day, were not counted as a `SCHOOL_OUT` day. I was able to source these dates from the local school district's website. There could be other schools in the surrounding area such as colleges, private schools with school out days that this data does not capture. 

* `is_holiday` indicates if the date is a federally (USA) recognized holiday. Examples include: Thanksgiving, Christams Eve, Christmas Day, New Years Eve, New Years Day, MLK Day, and Presidents' Day. 

* Mountain Weather Data was gathered from a NOAA weather station on the mountain where the resort is located.

* Town weather Data was gathered from the metropolitan area's airport. 

* All weather measurements are in Standard (degrees fahrenheit, inches)

After cleaning Visitation stats: 

**Daily Visits:**

- count    484
- mean     2422
- std      1710
- min      48
- max      7144

#### Resort 2

**Resort 2** is our smallest ski resort with the smallest population center nearby, approx 85,000 people. They report that their limiting factor for daily visitations is their parking lot, which typically tops capacity off at around 3,000 visitors, and therefore prevents the mountain from becoming too overcrowded. Their lift capacity is 5300 riders per hour. This mountain is a 501(c)3 nonprofit organization, and they generously gave 6 seasons worth of daily visitor data for use in this project. 

It's also worth noting that this moutain is closed on Tuesdays and Wednesdays unless those days fall during the holidays. 

A couple notes on getting the data for this particular ski area: 

* Resort 2 Mountain Weather Data was gathered from a nearby SNOTEL station that was not at the ski area (6.9 miles away). This ski area has a private weather station that has only started gathering weather data and displaying it online in the last year. 
* Wind data was not availible for the nearest town
* This ski resort only was open a handful of days during 2018 due to lack of snow and unusually warm weather all winter. 
* This resort has a relatively short season, December through April, resulting in less data points to work with

After cleaning: 

- Dataset count  468.000000

Daily Visitors stats:
- mean     992
- std      587
- min      88
- max      2944

#### Resort 3

This resort is our largest ski resort in both capacity (21,803 passenger/hr) and the population area (about 2 million) center close to the resort. 

A couple notes on getting the data for this particular ski area: 

* `SCHOOL_OUT` is if the public school district did not have class on a day that normally would have school (weekday). Examples include winter break, MLK Day, and teacher prep days. Weekends surrounding school closures such as Thanksgiving, Presidents Day, were not counted as a school out day. I was able to source this from the school district website. 
* Mountain Weather Data was gathered from a NOAA weather station on the mountain the resort is located, however this weather station is on a different face of the mountain and sometimes different aspects of this mountain can experience differences in weather, especially in snow and wind. 
* Wind data was not availible from the mountain weather station although my contact at the resort mentioned wind is a key factor in detering visitors or sometimes closing down the resort.

After cleaning stats:

- Dataset count: 851

Daily Visitors stats:

- mean     2869
- std      2014
- min      118
- max      8719

### Methods

#### EDA:
For each resort, I used the combined visitation, weather, and event data and performed an exploritory analysis on the data. This analysis included: 

* Remove Duplicate data points
* Removed 0 visit days
* Create categorical columns for Month and Day of week
* Addressed Null Values
* Addressed any outliers
* Explored correlation and and dropped highly correlated variables
* Indexed with Date
* Visualized data for distribution shape

#### Modeling:
I used a regression analysis to estimate the relationships between the dependent (visits) and independent variables in each dataset. A simple Linear Regression model made sense to build a baseline model with. The R-squared score tells us how much of the total variability in the data is explained by the model and is very useful when comparing different models. This will help us establish and determine if there is a realationship between our independent variables and the visitation this ski resort experiences. We can also use regression analysis to determine which specific features are good predictors for the target and which are not. 

Regression analysis is fairly interpretable, and being able to explain these models to a non-technical audience at ski resorts wil be key to the models being used for prediction. 

The final reason for using Regression Analysis is the ability to generate predictions, which is what this analysis is all about: seeing if we can predict visitation for ski resorts. 

To set up modeling, I first split the data from each resort into X (independent) and Y(dependent) dataframes. Then I split the data into train and test splits. Since we are working with a fairly small datasets for each resort, we will be using cross validation on our training data to score the models without having to utlize the test data until a best model has been determined. 

Each resort was run through iterative models, starting with Linear Regression with Standard Scaling and Encoding as a baseline. Decision Tree Regressors, Random Forest Regressors, Gradient Boosting Regressors, and XGBoost were all trained and fitted to the data with varying levels of success. See the individual notebooks for more details. The summary of the baseline and best results are below:  

## Results

#### Resort 1

* Baseline Model: Linear Regression
   * Mean CV Score: 0.579

* Best Model: XGBoost
   * Mean CV Score: 0.778
   * Test Score: 0.778
   * Mean Difference abs(predicted - actual): 687.81

#### Resort 2

* Baseline Model: Linear Regression
   * Mean CV Score: 0.368

* Best Model: Random Forest Regression
   * Mean CV Score: 0.367
   * Test Score: 0.301
   * Mean Difference abs(predicted - actual): 311

#### Resort 3

* Baseline Model: Linear Regression
   * Mean CV Score: 0.645

* Best Model: Gradient Boosting Regressor
   * Mean CV Score: 0.726
   * Test Score: 0.748
   * Mean Difference abs(predicted - actual): 786.74

## Conclusions

Overall Comments: 

It was interesting to run regression models on three ski resorts and get three different models back. The resorts with larger datasets and more weather data to work with had better results (Resort 1 and 3). Results from Resort 2 suggest that for smaller resorts with shorter seasons, more data may be needed, or further research is needed to account for the particular capacity limits of their parking lot. 

Overall, while weather and calendar events may have strong overall predictive power for ski resort visitation with Resort 1 showing 78% and resort 3 showing 75% in the model score. 

Specific resort conclusions are below: 

### Resort 1 Conclusions: 

For Resort One, we found that XGBoost Model with hyperparamters of `eta` = 0.2 and `max_depth` = 3 explained 78% of the variance in our data. 

The Feature Importance report shows that in general, Calendar features are more important than weather related features when predicting visitation.

Out of weather related features, Temperature in town and mountain snowdepth are important features. Mountain precipitataion was found to be a weak predictor of visitation. 

### Resort 2 Conclusions: 

For Resort 2, we found that a Random Forest Regressor Model with hyperparamters of `criterion` = 'mse', `max_depth` = None, `max_features` = 'sqrt', `n_estimators` = 14, explained 30% of the variance in our data. 

In general this dataset does not have as much of predictive power as our larger resort datasets. 

The Feature Importance report shows that in general, weather for this resort has stronger feature importance than date. This may be due to the fact that this resort maxes out around 3,000 visitors due to parking, and doesn't have the flexibility to take more visitors on. 

### Resort 3 Conclusions: 

For Resort 3, we found that Gradient Boosting Regressor with hyperparamters of `criterion`='friedman_mse', `n_estimators`=300, and `min_samples_split`= 2, explained 75% of the variance in our data. 

The Feature Importance report shows that in general, a mix of Calendar and weather features are important for predicting visitation.

Top predictors include: Saturday, Sunday, Average Temperature in town and on the mountain and Mountain Snow depth. 

Using the model to predict visitors: 

The Mean difference between predictions and actual is 787. That means that on any given day the predictor could be +/- 786 visitors off, or more Considering that the standard deviation of the target is 2,014, our predictor is well within 1 standard deviation and successful. 

### Would these Models be useful to ski resorts?   

For Resort 1, the mean difference between predictions and actual is 687.81. That means that on any given day the model could be +/- 688 visitors off, or more considering that the standard deviation of the target for Resort 1 is 1709.93, our predictor is well within 1 standard deviation and successful.

In reality when the ski resort goes to predict visitors, it may not make busienss sense to completely rely upon this model for staffing and resources. Parking 600 more cars than expected could strain personnel in the parking department, or preparing food for 600 more visitors than what actually came may result in loss of food product. 

These models by no means are intended to be the sole tool off which to predict visitors. It is intended to investigate the predictive power of calendar and weather factors, as well as help resorts with forecasting a ballpark visitation and then using information that a human element can apply such as: special events occuring, marketing, and long term weather trends. For example, a powder day after a dry spel may draw more visitors than a powder day after two weeks of consistant quality snowfall.

## Next Steps and Future Research

There's a lot that can be done with this model to improve it. Let's explore a few ways: 

* **Pre-Sale information and specific ticketing data** Given pre-sale ticketing information, factoring in blackout dates for season pass holders, and analyzing season pass holder visitation trends, it's possible to make the model more accurate. 

* **Weather Forecasting** I'd also like to find a way to use day-before forecasted weather information in the model. People deciding to ski or snowboard based on weather conditions tend to make the decision based on what the forecast says in the day or days before, not what actually happens that day. 

* **Pre/Post Pandemic** I would also like to take a deeper dive into how visitation trends have changed pre vs post pandemic. with more people working flexible and remote jobs, visitation has changed for ski resorts and modeling can help resorts stay on top of these chnages. 

## For More Information

See the full analysis in the Jupyter notebooks linked below or for a non-technical audience, the [Presentation Slides]. 

### Repository Structure

```
├── data                                          -> This file stores the .csv files of resort and weather data
├── images                                        -> This file stores images
├── .gitignore                                    -> Large and sensitive files to ignore
├── README.md                                     -> This is the overivew of the project, you are reading it now
├── presentation.pdf                              -> This is a PDF copy of the non-technical presentation slides accopmanying this project
├── Resort_1_EDA_Modeling_notebook.ipynb          -> Resort 1 Jupyter Notebook - EDA, Modeling and Results
├── Resort_2_EDA_Modeling_notebook.ipynb          -> Resort 2 Jupyter Notebook - EDA, Modeling and Results
├── REsort_3_EDA_Modeling_notebook.ipynb          -> Resort 3 Jupyter Notebook - EDA, Modeling and Results
├── Winter Resort Presentation.pdf                -> PDF Version of presentation slides
```

   [Lenore Perconti]: <https://github.com/lperconti>
   [Presentation Slides]: <https://docs.google.com/presentation/d/1njUu7dxNmD7zyrAf2w2S1ZsSsA5Z7SHtOiTVbAewyrQ/edit?usp=sharing>
   [Resort_1_EDA_MODELING_notebook.ipynb]: <https://github.com/lperconti/Visitation/blob/main/Resort_1_EDA_Modeling_notebook.ipynb>
