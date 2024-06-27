Confusion Matrix Chart
array([[36490,     0],
       [ 4421,     0]], dtype=int64)
Accuracy Score : 0.8919361540905869
Classifications
              precision    recall  f1-score   support

           0       0.89      1.00      0.94     36490
           1       0.00      0.00      0.00      4421

    accuracy                           0.89     40911
   macro avg       0.45      0.50      0.47     40911
weighted avg       0.80      0.89      0.84     40911

Question: How well does the logistic regression model predict the occurrence and severity of a storm, by customers out across the US?

Answer:

Not great.. When attempting to use the data in determining the "customers_out" value based on all the other predictors, ~7%.. However, we swapped the y varibale for best performance with the logistic regression model to the "outage_event" binary descriptor for a simpler and more appropriate assessment for the logistic regression determining factor of y/n. The results are almost 90% accurate.

Our first idea for this model was for attempting to predict the likeliness of # of power outages by loation and date, however, the dataset is limited in the variables being measured for this model's decision. We narrowed down to a binary "outage_event" analysis to determine how accurate log regression can tell us we have a bad storm, with the data we currently have. The ability to include more years/data points such as windspeed, tide levels, and amount of precip for more weather points, as well as informatoin such as utility construction type (OH vs UG), may provide help in determining potential outage events AND severity. In conclusion, this is only a small portion of the data required for a model to predict # of customers out for a storm event, but can be expanded on for better modeling and accuracy.

Another limitatoin with this model type (logistic regression) is that it's best used to determine the likeliness of something to occur, true/false. We simplified our data points by labeling them "outage event" vs "non-outage event" to test it's feasibility. We believe this analysis would be MUCH more helpful if we had the extra data points mentioned above, and altered our model to run multi-linear regression. If we continued to use our Y variable for the amount of customers out, then used the X variables of storm intensity, month, outage event, and the extra data points mentioned previously, our model may become more accurate and useful to determine serverity and likliness.

Let's try to run this data through a simple linear regression model to see how it performs on predicting customers out....
