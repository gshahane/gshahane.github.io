---
layout: post
title: Predicting Vehicular Crashes in the State of Iowa
description: Machine Learning approach to make roads safer
keywords: Machine Learning, Predictive Modeling, R
tags: [Machine Learning, Predictive Modeling, R]
comments: true
---

<h2>Research Question</h2>
<i>Predicting the occurrences of vehicular crashes on roadways of the State of Iowa based on spatial and temporal features of weather and traffic data.</i>

<h2>1. Introduction</h2>
According to the National Safety Council report, approximately 38,300 people were killed and about 4.4 million injured in the road accidents United States in 2015. There are a variety of reasons that contribute to accidents. Some of the reasons are adverse Weather and Traffic conditions that cause accident prone situations. Predicting likelihood of vehicular crashes due to the effect of Weather and Traffic features would be a major step towards achieving better road safety.
</br>
![Freeway Traffic](https://static.pexels.com/photos/394797/pexels-photo-394797.jpeg)  

We attempts to create models that account for spatial and temporal features to determine likely vehicular crash conditions. It is expected that the findings from this paper would help civic authorities to take proactive actions on likely crash prone weather and traffic conditions. These models can also be distributed in form of an APIs and can be added as an additional safety layer on existing navigational applications for safer commutes.

<h2>Data Collection and Aggregation</h2>
The data is extracted from Open Data based on the records collected from the Road Weather Information System of the State of Iowa to accommodate factors like weather, road conditions and crash details. The RWIS stations are spread across the State of Iowa, as shown in the map as below.

![Imgur](http://i.imgur.com/csJwy0g.png)  

<b>i. State of Iowa - Crash Data Set (https://catalog.data.gov/dataset/crash-data)</b><br>
This data is from the State of Iowa Open data for the vehicular crashes from 2006 to 2016. It includes data for date, time, location, weather and the road details for the crashes.  

<b>ii. Iowa Environmental Mesonet (https://mesonet.agron.iastate.edu/request/rwis/fe.phtml?network=IA_RWIS)</b><br>
This data is from the State of Iowa- Road Weather Information System’s archives. It includes data for average wind speed, maximum wind speed, humidity, visibility, wind direction, precipitation rate, and etc.  

<b>iii. Iowa Environmental Mesonet (https://mesonet.agron.iastate.edu/request/rwis/traffic.phtml)</b><br>
This data is based on the State of Iowa’s Historic Traffic Data and is extracted from Iowa Environmental Mesonet (IEM). It includes data for average speed, average headway, occupancy, and other traffic related features.  

There were a lot of data points as the data was recorded for every 10 minutes interval for last 2.5 years. Since the Traffic and Weather data was going to be integrated with the Crash records, these data points were aggregated on hourly basis. Crash data was integrated with the aggregated records based on the closest RWIS station to the crash site. Finally, we created a dataset, with hourly record of Traffic and Weather on Highways of the State of Iowa with the Crash frequency observed for the hour.  

<h2>Applying Machine Learning Algorithms</h2>

We used many Machine Learning algorithms such as Linear Regression, Zero Inflated Binomial Regression, Decision Trees and Random Forest.

We used <b>Multivariate Regression</b> to as we observed some linear relationship between the parameters such as in Average Speed and Crashes. However, when we created a model, the accuracy was very low. Correlation coefficient between predicted and original outputs was around 0.20. We also found QQ plot to be not normal as shown in the figure below. Hence, it was clear than not every variable had a linear relationship with the output (#Crashes).

![Imgur](http://i.imgur.com/Y7QHnih.png)

Moving forward, we also thought of dealing with the sparse data (since there were only a few crashes for all the hours for all highways in Iowa over the years). We implemented <b>Zero Inflated Binomial Regression</b>. The algrithm creates a regression model based on non-occurance of an event. Among other parameters, we considered Zero traffic also to be the reason of not having any crashes. However, the prediction accuracy remained poor. We concluded that the mere non-occurance of one parameter was not the reason behind no crash situation.

We found the best method for prediction was the <b>Random Forest</b> method with a combination of down-sampling of majority class and up sampling of minority class to balance the three categories of prediction: No Crash (0), 1 Crash (1), and 1+ Crashes (2). This was we could balance the dataset. Random Forest algorithm avoids overfitting and predicts based on a variety of internal models it gave higher accuracy than any other models.

<h2>Result</h2>

The following variables were identified as the most important ones that contributed to the higher accuracy of the Random Forest model. We can see that the most important variables as Average Speed, Traffic Volume, Dew Point, Surface Temperature, Wind Direction, and Wind Speed.

![Imgur](http://i.imgur.com/2ycXw1a.png)

The following Confusion Matrix summarizes the prediction accuracy of the Random Forest model. the model was able to predict <b>the crash occurrence of 1 Crash (1) with an accuracy of approximately 68%</b>. It was also observed that over <b>20%</b> of the records were incorrectly predicted as crash occurrences (False positives). This however, was acceptable as it makes the model only more stringent in predicting crashes more accurately.  

![Confusion Matrix](https://raw.githubusercontent.com/gshahane/Vehicular-Crash-Prediction-using-Machine-Learning/master/images/result_randomForest_upDOwn.png)

<h2>Conclusion</h2>

Radom Forest model predicts vehicular crashes with approximately <b>65% accuracy</b> using Weather and Traffic Data available with the State of Iowa. The result is obtained by up-sampling and down-sampling the majority and minority classes to match the crash frequencies.

## Future Scope
The accuracy of the models can also be increased by including the data for previous years previous to 2015. With increase in prediction accuracy, as a potential use case, the developed models can be used with the real time Road Weather Information System data to predict the vehicular crash occurrences in real time. This will help Highway Authorities in taking precautionary actions towards avoiding crashes, and also caution the drivers to be more careful, and thereby making roads safer for everyone.

<h2>References</h2>
- Abdel-Aty, M. A., Pemmanaboina, R. (2006). Calibrating a Real- Time Traffic Crash-Prediction Model Using Archived Weather and ITS Traffic Data, vol. 7(2)
- Bin Islam, M. and Kanitpong, K (2008). Identification of factors in road accidents through in-depth accident analysis. In IATSS Research Vol.32 No.2, 58-67
- C. Oh, J. Oh, and S. Ritchie, “Real-time estimation of freeway accident likelihood,” presented at the 80th Annu. Meet. Transportation Research Board, Washington, DC, USA, 2001.

<h2> Links </h2>
- <a href="https://github.com/gaurav-shahane/Vehicular-Crash-Prediction-using-Machine-Learning/blob/master/Research_Paper_VehCrash_Prediction_MachineLearning.pdf">Research Paper</a>
- <a href="https://github.com/gaurav-shahane/Vehicular-Crash-Prediction-using-Machine-Learning/blob/master/Presentation_VehCrash_Prediction_MachineLearning.pdf">Presentation</a>
