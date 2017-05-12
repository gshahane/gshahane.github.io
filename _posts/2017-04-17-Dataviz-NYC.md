---
layout: post
title: The Green Taxi &#58 Not So Green Anymore?
description: Visualizing patterns in New York Cabs Data using Tableau
keywords: Visualization, R, Tableau
tags: [Visualization, R, Tableau]
comments: true
---

<p>New York loves its taxicabs.<br><br>There are over 20,000 Yellow and Green taxicabs in New York. The additional e-hail services such as Uber and Lyft are bringing a lot more number of taxis in the New York region. Prior to 2013, most of the outer boroughs of New York were underserved by yellow cabs and were dominated by unlicensed drivers. <br><br>Introduced in 2013, Green taxis were meant to serve the outer boroughs(Bronx, Queens, Brooklyn, Staten Island and Manhattan above the 110th Street) to tackle the illegal taxi problem. Almost 3 years since its launch, as per various news articles, Green taxi is not doing that well. It would be interesting to know how the Greens are performing based on Taxi Trip Records in New York.</p>

<p>Let’s take a dive into their performance:</p>

<h2>New York Green Taxi Visualizations</h2>

<h3>1. General trends in the performance of Green Taxi for Jan-June 2016.</h3>

![General Stats](https://raw.githubusercontent.com/gshahane/NYC-Green-Taxi-Data-Visualization/master/Visualization%20Images/01%20General%20Stats.png)

Line graph shows that the maximum number of pickups from January - June 2016 occurred in Brooklyn with number of trips being about 3.5M and revenue being $ 55M, the highest of all boroughs. The month-wise pickups show that most of the pickups were around Brooklyn, Northern Manhattan and Queens, while the outer boroughs (Bronx and Staten Island) did not experience much pickups.<br>
<br>
<h3>2. Green Taxi trip numbers are declining.</h3>
![Pickups in New York Boroughs](https://raw.githubusercontent.com/gshahane/NYC-Green-Taxi-Data-Visualization/master/Visualization%20Images/02%20Pickups%20in%20New%20York%20Boroughs.png)
The road for Green taxis had been difficult for many reasons, primarily being the no hail southern manhattan zone. With the introduction of ‘Go Arro’ and ‘Way2ride’ apps around Aug 2015, things changed. The ridership of Green Taxi increased in the latter half of 2015, but after the initial boost however, the number of trips have been decreasing steadily for last few months starting March 2016.<br>
<br>
<h3>3. Drop in the number of trips similar for every borough.</h3>
![Drop Offs in New York Boroughs](https://raw.githubusercontent.com/gshahane/NYC-Green-Taxi-Data-Visualization/master/Visualization%20Images/03%20Dropoffs%20in%20New%20York%20Boroughs.png)
The drop patterns followed a similar trend across the boroughs with the number of trips increasing from August 2015 and uniformly decreasing starting March 2016.<br>
<br>
<h3>4. Is Green Taxi serving the outer Boroughs well?</h3>
![Pickups in New York Boroughs](https://github.com/gshahane/NYC-Green-Taxi-Data-Visualization/raw/master/Visualization%20Images/04%20Pickup%20locations%20in%20New%20York%20Boroughs.png)
About 50% of the total pickups occurred in the highlighted regions of Brooklyn and central Manhattan, showing that the regions away towards the outer boroughs were highly underserved<br>
<br>
<h3>5. Like pickups, the drop locations are concentrated too.</h3>
![Pickups in New York Boroughs](https://raw.githubusercontent.com/gshahane/NYC-Green-Taxi-Data-Visualization/master/Visualization%20Images/05%20Dropoff%20locations%20in%20New%20York%20Boroughs.png)
Almost all the drops occur in the regions where the passengers were picked up from. The map shows the where the drop locations of 50% of the taxi pickups were.<br>
<br>
<h3>6. Taxi flow mostly one directional.</h3>
![Pickups in New York Boroughs](https://raw.githubusercontent.com/gshahane/NYC-Green-Taxi-Data-Visualization/master/Visualization%20Images/06%20Network%20-%20Inter-Borough%20Taxi%20Flow.png)
Majority of Inter-Borough taxi traffic flows towards Southern Manhattan. Since Green Taxi is not allowed to pickup passangers from Southern Manhattan, they are not only missing on the trips originating from Southern Manhattan to the outer boroughs, but also they stuck in traffic, and lose time while returning empty from the area.<br>
<br>
<h3>7. The Yellow Taxi at par with the Green Taxi in serving outer Boroughs.</h3>
![Pickups in New York Boroughs](https://raw.githubusercontent.com/gshahane/NYC-Green-Taxi-Data-Visualization/master/Visualization%20Images/07%20Green%20Vs%20Yello%20Pickups.png)
For Green Taxicabs, most of the pickups were centered on Southern Manhattan and the nearby area. Bronx is comparatively underserved to the other boroughs. The Yellow taxis follow a similar pickup pattern but the number of pickups in the Northern Manhattan region are much more as compared to the Green taxis. This reports that Green taxis are failing to keep up in regions even where maximum pickups occur and lose revenue to Yellow and other taxis.<br>
<br>
<h3>8. Fading difference between the Yellow and the Green Taxi.</h3>
![Pickups in New York Boroughs](https://raw.githubusercontent.com/gshahane/NYC-Green-Taxi-Data-Visualization/master/Visualization%20Images/08%20Green%20Vs%20Yello%20Dropoffs.png)
For Green taxicabs, most of the drops patterns of the Green and the Yellow taxis are very similar. Green taxis does serve Brooklyn better than Yellow taxis to some extent but areas like Bronx are still underserved. The introduction of the mobile apps Arro and Way2ride have created equal opportunity for Green Taxis but the nature of trip followed by both the taxis are very similar thus defeating the entire purpose of introducing the greens in the first place.<br>

Based on falling numbers of Green Taxi and dialution of its purpose of serving New York Boroughs, we can ask questions such as:
<b>what can be done to strengthen Green Taxi further? or do we need Green Taxi at all?</b>

<h2>Tools used for implementation</h2>
Data cleaning and aggregation: <b>R, Excel</b> <br>
Creation of Visualizations: <b>Tableau, R, Adobe Photoshop</b>

<h2>References:</h2>
- (Reuben Fischer-Baum, Sep 22, 2015) Green Taxi Stays Close to Center, Retrieved Sep 12, 2016 from https://fivethirtyeight.com/datalab/new-yorks-green-cabs-stay-close-to-the-city-center/
- (Post Editorial Board, Oct 22, 2015) It’s not easy driving green: Uber & a new cab crisis, Retrieved  Sep 12, 2016 from http://nypost.com/2015/10/22/its-not-easy-driving-green-uber-a-new-cab-crisis/
