# **California AQI Analysis**

## **Sushant Harischandra Vema  ||  December 2021**

### **_1. Introduction_**

Between 2020 and 2021, California has experienced over 17,000 fires and nearly 7 million acres have burned ([https://www.fire.ca.gov/stats-events/](https://www.fire.ca.gov/stats-events/)). As climate change accelerates, the risk of wildfires worsens. Along with structural damages and loss of life, wildfires create hazardous air quality events, which cause serious health concerns for vulnerable groups.  This report explores the relationship between AQI, wildfire, and traffic data in order to understand the impact that fires have on air quality in California.

### **_2. Open-Ended EDA_**

Visualization 1:  AQI levels by month

![image_0](https://user-images.githubusercontent.com/76192765/147841167-af288aa3-9466-4e50-8b40-d449ecbe0c6b.png)

Above is a line plot using the seaborn package which displays the temporality of AQI levels (y-axis) over the months (x-axis).  The largest spikes occur during peak wildfire season between the months of August and October.  I was able to leverage these insights about the data's temporality by incorporating the date as a feature in the modeling portion of the project.

Visualization 2: AQI correlation with selected features

![image_1](https://user-images.githubusercontent.com/76192765/147841166-bb4dac24-804a-4f06-9f10-2c92e04ce3eb.png)

This visualization is a pairplot that shows the correlation between multiple features in the dataset and AQI.  Features of particular interest were wind speed, nitrogen dioxide levels, ozone, temperature, and sulphur dioxide levels.  This plot aided me in better choosing features for my models by determining which features share the strongest correlation with AQI levels.

**_EDA performed:_**

1. The first part of EDA I conducted was creating a new temporal split of the data.  I explored whether weekdays or weekends are associated with higher levels of AQI.  The findings proved that (as expected) weekdays displayed higher levels of AQI than weekends.

2. Another mode of exploration was to repeat EDA analysis on the new data in part two of my project.  While I expected this would yield significantly different results than the EDA in part one, I in fact found similar trends to those in part 1.

**_Open-Ended Questions for Further EDA:_**

The EDA indicated which features may yield the best results in predictive modeling, there is more room for future EDA and investigation.  Some additional questions include:

* Which particulates are most harmful to air quality? In other words, which particulates are associated with the worst AQI levels?

* In contrast, what are some features that may be associated with optimal AQI levels? For instance, proximity to forests, frequency of solar powered homes, and low level of traffic.  By finding features that are correlated with better air quality perhaps I can improve AQI through subsidizing these activities/natural solutions.

* Rather than correlation, can we investigate causation of poor AQI?

* How is agricultural and food production associated with AQI?  Merging farming/agricultural datasets could provide useful insights to this question.

### **_3. Problem_**

**_Hypothesis and its Feasibility:_**

**_	_**

I hypothesize that wildfires are strongly positively correlated with higher AQI levels.  More precisely, I believe that using features from the merged wildfire dataset such as acres burned, number of fires within a county, and duration of a fire; I can predict AQI categories with over 70% accuracy in my model.  If the open modeling section yields at least 70% accuracy using the wildfire features, my hypothesis is acceptable. If not, I will reject the hypothesis.  Not only does this dataset exist, but I was able to merge this external dataset and reach a conclusion with respect to my stated hypothesis; thus, my hypothesis is indeed feasible. In this case, my target/response variable (Y) is AQI level and my design matrix/features (X) are the multiple features from my merged wildfire dataset.

### **_4. Answer and Interpretation_**

**_Results:_**

Because my random forest model in the modelling sections scored an accuracy below 70% I reject my hypothesis that wildfires can be used to predict AQI.  However, my wildfire data in  conjunction with my epa data is indicative of average monthly aqi at a given site at a given.  In other words, the correlation observed in my linear model proved that there indeed is a positive correlation between AQI and the selected wildfire features.

### **_5. Modeling_**

**_Preliminary Modeling:_**

![image_2](https://user-images.githubusercontent.com/76192765/147841165-e03e3c1e-56e2-4a91-9cc8-4a30f7076364.png)

In, My preliminary modeling first merged the traffic dataset (AADT) with my AQI data.  I then executed some feature engineering and used log AADT, Temperature, NO2, Setting, and Elevation as the features for my design matrix (X/inputs).  I then employed a random  forest regressor model to predict AQI (Y/output).  My task, however, was to categorize my AQI predictions as good, moderate, unhealthy for sensitive groups, unhealthy, very unhealthy, or hazardous.  Thus, after predicting AQI I sorted the output into these categories and checked the performance of the model using the validation/testing dataset to calculate model error.

**_Further Modeling:_**

![image_3](https://user-images.githubusercontent.com/76192765/147841164-96b26ecb-bd01-4a3c-9b47-8e6bc0ceab12.png)

The open ended modeling portion of the project involved introducing an external dataset containing many wildfire features which was merged upon county code of the AQI dataset.  The features used in this model are median_dailyacres (median of daily acres burned in all fires for a particular county thus far); mean_FireDurationDays (average length in days of all the fires for a particular county thus far); and count_ExtremeOrActiveBehavior (the number of fires amongst all the fires for a particular county thus far which have been recorded as having extreme or very active behavior) - a binary feature. The features were selected to test the hypothesis that more severe fires would lead to substantially worse air quality in surrounding regions.  The output/response variable for the model was AQI which could be categorized into different levels.  With such an outstanding result from the guided modeling using a random forest regressor, I decided to again use the same model type for this section.  The reason I chose to regress is because the hypothesis stated that I believed wildfire features were significantly (positively) correlated with AQI levels.

### **_6.	Modeling Evaluation and Analysis_**

**_Guided Modeling Evaluation:_**

The error metrics used were the binary classification error (whether the correct class was predicted) and the CV error (K Fold cross validation).  The features I selected for this model are log AADT, Temperature, NO2, Setting, and Elevation.  The NO2 variable proved to be the strongest feature of the model due to it’s high correlation with AQI. This is to be expected as AQI is calculated using particulate matter like N02.  Initially, a linear model was used to predict AQI but the accuracy was low even after trying different features and tuning the hyperparameters of the model. The model yielded a binary classification score of 83% which is much higher than the baseline of 50%. That is, the model was a strong predictor of AQI.

**_Open-Ended Modeling Evaluation:_**

**Part 1**

The first model that I used was a RandomForestClassifier model. It’s target variable was the annual county aqi. We calculated the accuracy of the model and displayed it below.

The second model that I used was a linear regression model that had a target variable as the annual county aqi. The metrics that were used to evaluate the performance of the model were Mean Absolute Error (MAE), Mean Square Error (MSE) and the R2 score.

The third model that I used was the RandomForestRegressor model that had the same target variable and metrics used to evaluate it as the second model that I used.

Baseline Model: The baseline model only includes the wildfire dataset and is absent of granular data like month-by-month temporal data and specific site locations within a county. The reason I used this as the baseline model was so that I could get a basic understanding of the general trends between the county and the AQI. The model did not perform well because of the lack of a large number of data points which was largely limited by the way the data was presented - granularity: each row represented an individual fire in a particular state.

The metrics and the plots for the above mentioned baseline model are shown below:

The plot below is for the Linear regression model that I implemented. The MSE and MAE are definitely low due to the fact that the range of the points is very low as well. However, it is not clear that a linear trend is correct for the actual and predicted values.

![image_4](https://user-images.githubusercontent.com/76192765/147841163-6f80580f-7677-4e3e-ad4e-aef1fc52d4b3.png)

**Part 2**

Improved Model: Taking into consideration the problems that I faced using the baseline model and the fact that it was not performing well with the less granular data set, I decided to use a data set with more granularity. We achieved this by merging the dataset that I gained from part 10 of the ipython notebook with the fire data set that I used in the baseline model. The way that the granularity was increased was because the dataset that I merged on was grouped by county code, site number and month. This means that for every single site in a given county, every month had a mean AQI value associated with it. This resulted in an increase in the number of data points. This also improved the model since the number of features that the AQI values depended on increased from 1 to 3. I used the same 3 models that I stated above.

The metrics and the plots for the above mentioned baseline model are shown below:

The plot below is for the Linear regression model that I implemented. As can be seen, the number of points increasing definitely improved the regression line that was fit to the model. The MSE and MAE definitely increased due to the fact that the range of the points increased. However, it is much more clear now than before that a linear trend is correct for the actual and predicted values.

![image_5](https://user-images.githubusercontent.com/76192765/147841162-c34b7094-74a6-4ff0-8c65-688732702924.png)

### **_7. Model Improvement_**

**_Model Problems:_**

1. Originally I implemented a linear regression model to predict AQI from the merged dataset and chosen features.  Even after attempting many variations of chosen features I still lacked a reasonable accuracy score.

2. Another problem that I ran into during my modeling was underfitting.  I was concerned that if I added too many features the model would have an excellent training score but output substantially lower test scores.

3. Finally, I realized that even though the model had improved after changing the model type and adding features I still had room for improvement.  Specifically, I noticed that the daily traffic feature had extremely large values and may stifle the model’s performance.

**_Model Solutions:_**

1. To solve the first problem I changed the model to a random forest regressor instead of a linear regression model.  the reason for doing so is that this model is better suited for non-linear data and the features lacked the clear linear relationship needed for a successful linear model.  After simply rebuilding a new model (the random forest regressor) I observed approximately a 23% improvement in our AQI classification accuracy.

2. While overfitting certainly poses a threat to a model’s non-training accuracy, adding features can also yield far better results when it has a larger design matrix to "learn" from.  In the case of the model, adding more features actually improved both training and testing accuracy.

3. To amend this issue I conducted some simple feature engineering on the daily traffic variable by taking a logarithmic transformation.  This proved to be more easily digestible for the model and again boosted both the training and test set accuracy.

### **_8. Future Work_**

**_Further research:_**

My exploration provided valuable insights regarding the relationship between wildfires and AQI.  But wildfires and traffic rates are only a small piece of the puzzle in terms of what impacts the air quality.  There are a plethora of other factors/features to explore with respect to air quality.  One such direction I could delve into is how agriculture/food production impacts air quality in its region.  Current revenue in the food industry is approaching 9 trillion dollars ([https://www.statista.com/outlook/cmo/food/worldwide](https://www.statista.com/outlook/cmo/food/worldwide)) and is expected to grow about 4.5% in the coming years.  The wide scale production of food comes at a cost; this cost comes in the form of the animals’ waste and the food they need to eat, the packaging of food, and the transportation needed for delivery.  The amalgam of these threats from the food industry likely harm the quality of the air.  This particular direction for further research is interesting because the harmful effects of the food industry on the environment are widely discussed but its direct relationship to air quality lacks the necessary data analysis.
