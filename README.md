Our complete end-to-end data pipeline and SDLC analysis on public utility outage and storm data to perform sampling and learning on past outage/storm events. 

We sourced our data from Eagle - Eye Energy Outage, and NCEI Storm events. We used their APIs to perform an ETL process where we merged our data sets together by the FIPS code for state and county, then cleaned and prepped the data after importing it into our AWS S3 buckets. Once the data was prepped in our cloud hosted env, We connected to it to perform visualizations in PowerbI, and ran further analyses with AI regression models to see if it can be used to predict storm severity and # of customers out.



Below is a summary of our analyses performed on a series of datasets we found on public web pages for electric utility outage data merged with weather events. 

**Logistic regression on "Outage event" yes/no?:**
Confusion Matrix Chart
array([[36490,     0],
       [ 4421,     0]], dtype=int64)
**Accuracy Score : 0.8919361540905869**
Classifications
              precision    recall  f1-score   support

           0       0.89      1.00      0.94     36490
           1       0.00      0.00      0.00      4421

    accuracy                           0.89     40911
   macro avg       0.45      0.50      0.47     40911
weighted avg       0.80      0.89      0.84     40911

Question: How well does the logistic regression model predict the occurrence and severity of a storm, by customers out across the US vs predicting an "outage event" by including the customers out count?

Answer:

When attempting to use the data in determining the "customers_out" value based on all the other predictors, ~7%.. so not very well. However, when comparing the y varibale in the log model to the "outage_event" descriptor, the determining factor of y/n for an event are almost 90% accurate.

Our first idea for this model was for attempting to predict the likeliness of # of power outages by loation and date, however, the dataset is limited in the variables being measured for this model's decision. We narrowed down to a binary "outage_event" analysis to determine how accurate log regression can tell us we have a bad storm, with the data we currently have. The ability to include more years/data points such as windspeed, tide levels, and amount of precip for more weather points, as well as informatoin such as utility construction type (OH vs UG), may provide help in determining potential outage events AND severity. In conclusion, this is only a small portion of the data required for a model to predict # of customers out for a storm event, but can be expanded on for better modeling and accuracy.

Another limitatoin with this model type (logistic regression) is that it's best used to determine the likeliness of something to occur, true/false. We simplified our data points by labeling them "outage event" vs "non-outage event" to test it's feasibility. We believe this analysis would be MUCH more helpful if we had the extra data points mentioned above, and altered our model to run multi-linear regression. If we continued to use our Y variable for the amount of customers out, then used the X variables of storm intensity, month, outage event, and the extra data points mentioned previously, our model may become more accurate and useful to determine serverity and likliness.

Let's try to run this data through a simple linear regression model to see how it performs on predicting customers out....



................................................
**Multi-linear regression on "customers out" varibale to determine an estimate based on given independent variables:**
# Fit the model using the training data
linear_reg.fit(X_train, y_train)

# Make predictions using the testing data
y_pred = linear_reg.predict(X_test)

# Evaluate the model
mae = mean_absolute_error(y_test, y_pred)
mse = mean_squared_error(y_test, y_pred)
r2 = r2_score(y_test, y_pred)
...................................................................................................
Mean Absolute Error: 467.0639942368089
Mean Squared Error: 4564907.249323451
**R-squared: 0.16878970949313044**
Coefficients: [ 0.00000000e+00  9.64308978e-10 -2.60484280e+02  2.40663570e+01
  3.24518824e+03]
Intercept: 2902.2844677702215
YEAR	FIPS	customers_out	month	storm_intensity
769049	2014	39035201411	17	11	2
341027	2014	36029201412	6	12	7
997971	2014	36083201411	193	11	7
498201	2014	8031201411	3	11	7
453388	2014	6029201412	64	12	5


With multilinear regression, we can see that there is room for improvement on our data and analysis attempts for predicting customers out. If we included the additional data points stated above, my suspicion is this model would be more accurate. Currently, with our 5 "x" independent variables, we're receiving approximate 17% explanation in the variance of the dataset based on this r^2 value. More data to sample, and more variables would help increase the accuracy. However, it is still a very nuanced and complex analysis. Models that predict the weather attempt similar things, and we often see incorrect predictions from our weather reporters, even with their extensive tools and knowledge.

A summary of our coefficients will explain the effect our x variables have on the result.. Firs 0.0000 indicates the "year" column is a non changing value and has no effect. This is correct as it is just the year 2014. If we incorporate more years of data, I'd be interested in seeing the effect year has on our results. With climate change, I'd expect the higher year values, to have higher effects on our predicted results...

The FIPS score is a "coded" location based assessment and the coefficient o 9.6-10 indicates a number close to zero and negligible effect on our model. Perhaps we should narrow this down to REGION, or only run these models on a single region, excluding other natrual differences in climate through out the continental US.

The month coefficeint.. indicates a negative relationship as months go up, outages decrease by approximately 260. This makes sense, as most outages occurr btween spring and fall.

The storm intensity is also self explanatory, indicating the higher the intensity weighting, the numbers go up on avg 240 per intensity score.

Lastly, the outage event boolean, suggests a strong positive relationship. Since 1 always equals an event, it will always have higher numbers, showing this result.
