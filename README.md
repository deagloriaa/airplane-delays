# US Airplane Delays

By: Dea Gloria A. N.

------

This project was made as a part of Coursework requirement for ST2195 - Programming for Data Science at University of London.

We will be looking at the flight data between 2003-2004 of various airlines in the United States to answer the 5 given questions using both Python and R:

1. When is the best time of day, day of the week, and time of year to fly to minimise delays?
2. Do older planes suffer more delays?
3. How does the number of people flying between different locations change over time?
4. Can you detect cascading failures as delays in one airport create delays in others?
5. Use the available variables to construct a model that predicts delays.

The data processing are done through a separated script uploaded in this repository:
a. ([Python Script](https://github.com/deagloriaa/airplane-delays/blob/main/plane-delays%20Python.ipynb))
b. ([R Script](https://github.com/deagloriaa/airplane-delays/blob/main/plane-delays%20R.html))

------

### 1. When is the best time of day, day of the week, and time of year to fly to minimise delays?

First, we assume a flight is considered delayed when the arrival delay (ArrDelay) is greater than zero. We choose arrival delay instead of departure delay (DepDelay) because a flight could be delayed in the departure but then the plane speed up on the way, so it does not counter an arrival delay. Hence at the end, the arrival delay is used, specifically on all the positive values because negative observations imply that the plane arrives earlier than expected. Furthermore, out of 24 hours a day, we group the departure time per 3 hours interval so that we obtain 8 different bins to get the best time of day to fly to minimize delays.

![Average Delay](https://github.com/deagloriaa/airplane-delays/blob/main/pictures/avg_delay.png)

From this table, we can see the departure time sorted based on the average delay in minutes in ascending order. The ‘new_dep’ represents the 3-hours’ time interval in the departure time, the ‘avg_delay’ represents average delay in minutes, and the ‘count_of_delay’ represents the count of delayed flights. 

We observe that the lowest average delay (12.49 mins) happened between 3 to 6 AM whereas the highest average delay (84.27 mins) happened between 12 to 3 AM. Interestingly, even though it has the highest average delay, the count of delay is the least during that period. This implies that the delay does not occur often but once there’s a delay, the delay is going to be very long.

![Average Flight Delay Per 3 Hours](https://github.com/deagloriaa/airplane-delays/blob/main/pictures/avg_delay_viz.png)

Count of delays implies how often the delay can occur during the specified period. However, the average delay implies how long a delay can occur in average. Hence, we will use the average delay rather than the count of delays as our benchmark to avoid long-waiting delay.

In a passenger’s point of view, it would be best to take a flight which has a departure time around 3 to 6 AM as it has the least average delay. Meanwhile in the airline companies’ perspective, generally they should minimize as many delays as possible. Specifically, from this chart, they should reduce the delays between 12 to 3 AM as the average delay between these hours is the highest although the count of delayed flights is the lowest among all.

![Daily Average Delay](https://github.com/deagloriaa/airplane-delays/blob/main/pictures/avg_delay_day.png)
![Daily Average Delay Viz](https://github.com/deagloriaa/airplane-delays/blob/main/pictures/avg_delay_day_viz.png)

Grouping by daily basis (1 is Monday and 7 is Sunday), we can observe that the average delay ranges around 23-27 minutes. Hence, there does not seem to be a significant difference in which day a passenger chooses to fly. However, we still can suggest that Saturday would be the best day of week to fly as it has the least average delay time and lowest count of delays. 

![Quarter Average Delay](https://github.com/deagloriaa/airplane-delays/blob/main/pictures/avg_delay_qtr.png)

Lastly, in quarterly basis, there is a significant difference in the average delay time between both years. Overall, the average delay is much higher in 2004 for every quarter (red). Meanwhile, the delay pattern in 2003 (blue) went up and down along the year. In 2004, the average delay time increased from the first quarter to the second quarter and then decreased gradually until the end of the year. The average delay is minimum on quarter two in 2003 across the year at 22.89 minutes/flight. Whereas in 2004, Q2 has the maximum average delay at 28.93 minutes/flight.


### 2. Do older planes suffer more delays?

In the plane dataset, there are some missing and invalid values that we must clean beforehand. Afterwards, we merge the plane dataset and the flight dataset based on every aircraft (TailNum) and compute the average delay and count of delays, also considering what year the plane is manufactured.

![Delay by Year of Manufacture](https://github.com/deagloriaa/airplane-delays/blob/main/pictures/avg_delay_by_year.png)

Obviously, planes that are old (manufactured way before 2003 and 2004) have more flight hours than those which are just manufactured recently. Thus, there is also a higher chance for these older planes to have more delays in terms of its count. However, there might also be chance that the older planes are not used frequently between 2003 and 2004, resulting the count of flights of old planes is not as many as we expected. Therefore, we decided to use the average delay of every plane and plot it against the plane manufacture year.

If older planes suffer more delays, we will see there’s a declining trend on the average delay as the year goes towards present. However, we do not see such trend. In fact, there are 2 recent planes that has a high average delay (possibly because of an unexpected delay and only happened in a little frequency of time). Overall, we can see that the planes are roughly delayed around the same amount of time in average, no matter whether it is an old plane or not. Therefore, we can conclude that older planes do not suffer more delays in terms of average delay time. 

### 3. How does the number of people flying between different locations change over time?

As we do not know the exact number of passengers flying for every flight, we assume that it is the same for every flight. Hence, we will count the number of flights in order to see the difference between both years. Observing the trend of people flying between different locations over time can be done in two ways.

![Change 1](https://github.com/deagloriaa/airplane-delays/blob/main/pictures/chg_ot_1.png)
![Change 2](https://github.com/deagloriaa/airplane-delays/blob/main/pictures/chg_ot_2.png)

The first approach is by comparing every route for both years. Initially, we create a new column named ‘org_dest’ then count the total number of flights separated into two table by years. Afterwards, we merge the table and take the ‘difference’ between total flights in 2003 and 2004. As the flight count might differ for each route (could be extremely far from one to another route), we compute the percentage of change by dividing the difference with ‘total_flight_2003’ to equally compare all the routes. Finally, we arrange which route has the greatest and least percentage count of flights change. A negative value in the percentage change implies that the total flight is decreasing from 2003 to 2004 (shown on the left table), whereas the positive value represents an increasing number of flights (shown on the right table).

![Origin Change](https://github.com/deagloriaa/airplane-delays/blob/main/pictures/org_comp_2003.png)
![Destination Change](https://github.com/deagloriaa/airplane-delays/blob/main/pictures/dest_comp_2003.png)

The second approach also compares the percentage difference of total flights in 2003 and 2004 but separated by each origin and destination instead of a united route. We will have a look at two maps generated using the Plotly package to help us understand which areas have significant change in the number of flights in both years. The legend implies percentage difference in terms of zero decimal places (e.g. 5 implies 500%). 
In general, the number of people flying between locations does not tremendously change in most areas. However, there are some locations that have extreme difference in the number of flights between 2003 and 2004. For example, Erie International Airport (IATA code: ERI) has the greatest negative difference as the origin and destination for both years. This implies that flights that are from and to ERI increased tremendously from 2003 to 2004.  Other locations that have significant difference can be seen on the table shown on the markdown/notebook.

### 4. Can you detect cascading failures as delays in one airport create delays in others?

![Routes Top 5](https://github.com/deagloriaa/airplane-delays/blob/main/pictures/count_delay.png)

The first step that we will take to detect the cascading failure is to check the top 5 routes that have the greatest number of flights, assuming that these airports are busier than the others. We observe that the route with highest number of flights occurs from Los Angeles International Airport (LAX) to San Diego International Airport (SAN) and the second top route is the other way round. Therefore, we will inspect flights that depart from or arrive to LAX or SAN as a sample case.

![Sample routes](https://github.com/deagloriaa/airplane-delays/blob/main/pictures/flight_LAX_SAN.png)

The next step is to apply the filter on the origin and destination. Additionally, we choose the arrival delay to be greater than 60 minutes as we are trying to capture serious delays instead of little delay time. Then we take relevant columns such as year, month, day, the plane’s unique identifier (TailNum), expected and real departure time, expected and real arrival time, the departure delay, the arrival delay, origin, and destination. For sampling purpose, we will observe in the first ten rows and see if there’s a delay created in one location causing a continuous delay in the next location.

The first evidence that we can find is on a plane with tail number N342. The plane was supposed to leave at 16:30 from LAS and arrive to LAX at 17:40. However, there’s an 85-minute delay during departure and consequently caused a 78-minute arrival delay. So, the plane ended up arriving at 18:58 at LAX when the next flight was scheduled at 18:10. The first delay caused a 75-minute departure and arrival delay on the second flight. It is possible that the delays are constant because it is the plane’s last flight, so it need not to worry about delaying another flight.

The second case can be seen on plane N450UA which was supposed to depart at 08:15 from ORD but got delayed for 2 hours and 3 minutes. We can see that the arrival delay (102 minutes) is lesser than the departure delay, implying that the plane tried to minimize causing another delay. This was proven on the second flight of N450UA, which has a departure delay of 93 minutes and an arrival delay of 84 minutes.

From these first ten rows, we can see two cases that got delayed in one location and ended up creating delay on the next stop. Hence, if we are taking a whole population of this data, even not only the severe delays, we might be able to see many more of such cases.

### 5. Use the available variables to construct a model that predicts delays.

Since we want to predict the arrival delay, which is a numerical continuous variable, we classify this case as a supervised learning – regression task. We choose to use a linear regression model, namely ridge and lasso regression, and random forest. In R, we will make use of the mlr3 package. 

The first step is to select variables that can be obtained before the flight happens. The aim is to make the model reusable to predict future events (i.e. flights that have not happened). Afterwards, we created a regression task with arrival delay as the target variable that we want to predict. We also set Mean Square Error (MSE) as the measurement of the model. To make the model reproducible using the same splitting method, we use the set.seed() function. After that we split the data into train and test. For ridge, we set the alpha parameter in the learner as 0. Whereas for LASSO, we set alpha equals to 1.

![Ridge](https://github.com/deagloriaa/airplane-delays/blob/main/pictures/Ridge.png)
![Lasso](https://github.com/deagloriaa/airplane-delays/blob/main/pictures/LASSO.png)
![Random Forest](https://github.com/deagloriaa/airplane-delays/blob/main/pictures/RF.png)

Both linear regressions show similar result for MSE, around 1005, which is quite high. Whereas the random forest shows a lower MSE at 988.36. Since we would prefer model that has the lowest MSE, we would prefer Random Forest over Ridge and LASSO. However, the weakness of random forest is it tends to overfit the training data. Nevertheless, the model can still be improved using a correct hyperparameter tuning, increasing the sample size, or we can try to use another regression model.

![Eval Ridge](https://github.com/deagloriaa/airplane-delays/blob/main/pictures/evalR.png)
![Eval Lasso](https://github.com/deagloriaa/airplane-delays/blob/main/pictures/evalL.png)
![Eval RF](https://github.com/deagloriaa/airplane-delays/blob/main/pictures/evalRF.png)

In Python, we are using the scikit-learn package. The first step that we take is to split the explanatory variables and the target variable. Afterwards we convert it to numpy array using the function to_numpy(). Then, we split the data into train and test, similarly with R, 70% on training and 30% on test. We fit the model using Ridge, LASSO, and Random Forest then predict the test data using these models. Evaluating using MSE as the measure, Ridge has a slightly lesser MSE than LASSO, but both linear regressions yield a very high MSE value. Additionally, the R2 score which implies how strong the explanatory variables can explain the target variable shows a very low value. 

For the Random Forest algorithm, we decided to take sample of 50,000 observations as it consumes a lot of memory. Comparing to the previous learners, Random Forest has a small MSE in the train dataset but a very high MSE in the test dataset. This exhibits the problem of overfitting where the model mostly follows the pattern of the training data. However, this can be solved by using more dataset to train provided that there is enough memory to perform this algorithm.

------
### Side note:

As this is my very first project and I am the first batch to take this module, I believe there is still lack of analysis/usage of machine learning algorithm which can still be improved. However, I put this projoect here as a stepping stone of my learning journey in understanding Data Science.

For this project, I nearly received a First Class Honours grade. In the future, some improvements will be made.
