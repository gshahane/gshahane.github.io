---
layout: post
title: Does raising Excise Tax discourage smoking?
description: Analyzing CDC surveys from 1996-2014 to study the effect.
keywords: R, Tableau, Statistics
tags: [R, Tableau]
comments: true
---

## Research Problem   
Study of the impact of Excise Taxes on Cigarette on the smoking population in the United States.

## 1. Introduction

As per the Centers for Disease Control and Prevention, Smoking remains the primary cause of preventable deaths in the US, Killing an estimate of over 480,000 citizens each year. Along with a few direct initiatives, to target a bigger population of cigarette smokers, the government usually uses means of increasing excise tax on cigarette.  

The study focuses on whether changes in the State Excise Tax on Cigarette are really effective on the ground level? Will it affect a variety age groups in the smoking population equally? To uncover the answers to these questions, we decided to verify the relationship between taxation and the behavior of smoking population belonging to various age groups in all the states of USA.

## 2. Data Collection and Transformation

Our primary source of information is the website of Centers for Disease Control and Prevention (www.cdc.gov). CDC has a huge repository of data repository pertaining to various health related conditions. We will use the behavioral risk factor data related to tobacco use from the year 1996 to 2014 for our analysis.

####	2.1 Behavioral Risk Factor Data on Tobacco Use Survey
This dataset contains record of surveys conducted every year for determining the tobacco usage including cigarette smoking in United States. The dataset includes variables such as year, location, percentage of population smoking cigarette, age groups.

####	2.2 Excise tax on Combustible Tobacco Products
This dataset records excise tax for cigarette per pack in dollars for all the states from 1996. We combined this data set with Behavioral Risk Factor Data on Tobacco Use dataset and are using it to measure effect of these taxes on % of smoking population in every state in United States.

## 3. Research Questions
1. Is there a relationship between state level excise tax on a pack of cigarettes and the percentage of cigarette smoking population in that state?

2. If there is a relationship between the state excise tax and percent of smoking population, is the relation same for all age groups?

## 4. Data Analysis

We can start with plotting a scatterplot (as shown below) for the Smoking Population Percentage Vs State Excise Tax. We can see that there is a pattern emerging through the data which tells us that the Smoking Population is decreasing with the increase in State Excise Tax. From a <b>Correlation Coefficient of (r) -0.5422938,</b> we can affirm the relationship. This answers our first research question.

![Imgur](http://i.imgur.com/hR2lbTq.png)

Let's plot the same scatterplot for all the age groups to find out if the State Excise Tax affects all the groups equally.  

![Imgur](http://i.imgur.com/K77Tzkj.jpg)

The scatterplot tells us that the rate of change in Smoking Population is different for all the age groups. In order to find out the group that is most and the least affected among the above. We find the best fit lines as below for the age groups using Linear Regression.

<b>For Age 18-24 Years:</b>  
% Smoking_Population_Age18to24 = 29.8720 - 3.4638 * State Excise Tax

<b>For Age 25-44 Years:</b>  
% Smoking_Population_Age25to44 =27.6920 - 3.6560 * State Excise Tax

<b>For Age 45-64 Years:</b>  
%Smoking_Population_Age45to65 = 24.0853 - 3.2025 * State Excise Tax

<b>For Age 65+ Years:</b>  
%Smoking_Population_Age65plus = 10.8653 - 1.5286 * State Excise Tax


## 5. Findings

- Increase in Excise tax <b>negatively correlated</b> with the percentage of smoking population in United States.  

- As per our prediction, <b>$1</b> rise in Excise Tax on Cigarette packet will result in decrease in smoking population by approximately <b>3%</b>.

- Making cigarette expensive affected the <b>least to the age group of 18 to 24 years,</b> even though they might have the lowest income of all the age groups.

- Making cigarette expensive affected the <b>most to the age group of 45 to 64 years,</b> even though they might be earning more than any other age group.

We can conclude that the policy of controlling smoking behavior works well for the middle aged citizens (45-64 years), rather than the youth (18-24 years). As per the findings, appropriate changes can be done in the policy or any other alternatives can be explored to influence the target age groups.

One additional interesting observation that we found was via bar plots for all the age groups as below.

![Imgur](http://i.imgur.com/zFTVw0B.png)

The older population, most likely, have been smoking since a long time and hence the tax raise doesn’t help reduce the smoking population much. Also, if we see there is a steep fall of percent of smoking population after 45-64 years. Giving this observation we feel that this is a potential indicator that many smokers belonging to this age group might not make it 65 years or more

## 6. Limitations

1.	<b>Federal taxes not considered:</b> Variation in the Federal Taxes can also bring a change in the smoking percentage and can lead them to inaccurate results. As the next steps, we can use the combined effect of Federal and State Excise Taxes to analyze their effect on Smoking Population Percentage.

2.	<b>Restricted Range</b>: The best fit line as shown in the scatterplots in the research question analysis only applies to the range of data (1996-2014) we have used for analysis. If the data are made available by CDC, we can generalize the results better.

3.	<b>Causation:</b>  We cannot infer the correlation as causation as there can be other factors too which affected the percentage of Smoking Population in United States. As only 29% of the variance in the Smoking Population Percentage is explained by State Excise Tax Rate, we can explore more parameters that might affect the Smoking Population Percentage.


## 7. Attachments

- <a href = "https://github.com/gaurav-shahane/Smoking_Vs_ExciseTax_Data_Analysis/blob/Update/Project_Paper_The%20study%20of%20Higher%20Excise%20Tax%20on%20Cigarette%20on%20Smoking%20Population.pdf">Research Paper<a>

- <a href="https://github.com/gaurav-shahane/Smoking_Vs_ExciseTax_Data_Analysis/blob/master/Project_Presentation_The%20study%20of%20Higher%20Excise%20Tax%20on%20Cigarette%20on%20Smoking%20Population.pptx"> Project Presentation</a>

## 8. Reference Datasets

- Data on Smoking and Tobaco Use, Office on Smoking and Health, National Center for Chronic Disease Prevention and Health Promotion, Retrieved 15th September 2015, from http://www.cdc.gov/tobacco/data_statistics/surveys/index.htm
