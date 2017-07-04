---
layout: post
title: Tutorial &#58 Medicare Data Analysis and Visualization.
description: Data Cleaning and Analysis using Python-Pandas and Seaborn.
keywords: Medicare, Python, Data Analysis, Tuorial
tags: [Tutorial, Python, Data Analysis, Visualization, Seaborn]
comments: true
---
The dataset is from Center for Medicare and Medicaid Services which contains costs of 100 most frequently billed discharges billed under medicare from almost 3000 US hospitals. Here is the groundwork of cleaning and analyzing the data as the first step in understandig the data.

For a detailed understanding of the data, please check this [source link]( https://www.cms.gov/Research-Statistics-Data-and-Systems/Statistics-Trends-and-Reports/Medicare-Provider-Charge-Data/Inpatient2011.html) or [this pdf](https://www.cms.gov/Research-Statistics-Data-and-Systems/Statistics-Trends-and-Reports/Medicare-Provider-Charge-Data/Downloads/Inpatient_Methodology.pdf)


****  
### Table of contents

**Data Preprocessing**  

1.1 [Data preprocessing for Missing values, Outlier Detection, Statistical Summary and Datatypes Conversion](#dataprep)

**Analyzing the columns of the dataset**

2.1 [DRG Codes](#drg)  
2.2 [Provider ID](#p_id)  
2.3 [Provider City and Provider State](#p_citystate)  
2.4 [Total Discharge](#tot_discharge)  
2.5 [Average Charges and Payments](#avg_cp)  
2.6 [Payments made by beneficiary and/or third party](#pay_ben)  

**Exploratory Visualizations and Conclusions**  

3.1 [Is Medicare really effective in bringing cost down for patients?](#compare)  
3.2 [Are there strong differences among healthcare costs for all US states?](#map)  
3.3 [Top 10 Treatments (DRGs) that consume the highest Average Medicare Payments](#topten)  
****

### 1.1 Data Pre-processing <a name="dataprep"></a>
Let's first import the dataset in CSV form.


```python
#import
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np
from IPython.core.interactiveshell import InteractiveShell
InteractiveShell.ast_node_interactivity = "all"

%matplotlib inline
df = pd.read_csv("IPPS.csv")

#Let's check a few lines with column names for visually observe the data.
df.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DRG Definition</th>
      <th>Provider Id</th>
      <th>Provider Name</th>
      <th>Provider Street Address</th>
      <th>Provider City</th>
      <th>Provider State</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>039 - EXTRACRANIAL PROCEDURES W/O CC/MCC</td>
      <td>10001</td>
      <td>SOUTHEAST ALABAMA MEDICAL CENTER</td>
      <td>1108 ROSS CLARK CIRCLE</td>
      <td>DOTHAN</td>
      <td>AL</td>
    </tr>
    <tr>
      <th>1</th>
      <td>039 - EXTRACRANIAL PROCEDURES W/O CC/MCC</td>
      <td>10005</td>
      <td>MARSHALL MEDICAL CENTER SOUTH</td>
      <td>2505 U S HIGHWAY 431 NORTH</td>
      <td>BOAZ</td>
      <td>AL</td>
    </tr>
    <tr>
      <th>2</th>
      <td>039 - EXTRACRANIAL PROCEDURES W/O CC/MCC</td>
      <td>10006</td>
      <td>ELIZA COFFEE MEMORIAL HOSPITAL</td>
      <td>205 MARENGO STREET</td>
      <td>FLORENCE</td>
      <td>AL</td>
    </tr>
    <tr>
      <th>3</th>
      <td>039 - EXTRACRANIAL PROCEDURES W/O CC/MCC</td>
      <td>10011</td>
      <td>ST VINCENT'S EAST</td>
      <td>50 MEDICAL PARK EAST DRIVE</td>
      <td>BIRMINGHAM</td>
      <td>AL</td>
    </tr>
    <tr>
      <th>4</th>
      <td>039 - EXTRACRANIAL PROCEDURES W/O CC/MCC</td>
      <td>10016</td>
      <td>SHELBY BAPTIST MEDICAL CENTER</td>
      <td>1000 FIRST STREET NORTH</td>
      <td>ALABASTER</td>
      <td>AL</td>
    </tr>
  </tbody>
</table>
</div>

Dataset Continued..

<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Provider Zip Code</th>
      <th>Hospital Referral Region Description</th>
      <th>Total Discharges</th>
      <th>Average Covered Charges</th>
      <th>Average Total Payments</th>
      <th>Average Medicare Payments</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>36301</td>
      <td>AL - Dothan</td>
      <td>91</td>
      <td>$32963.07</td>
      <td>$5777.24</td>
      <td>$4763.73</td>
    </tr>
    <tr>
      <th>1</th>
      <td>35957</td>
      <td>AL - Birmingham</td>
      <td>14</td>
      <td>$15131.85</td>
      <td>$5787.57</td>
      <td>$4976.71</td>
    </tr>
    <tr>
      <th>2</th>
      <td>35631</td>
      <td>AL - Birmingham</td>
      <td>24</td>
      <td>$37560.37</td>
      <td>$5434.95</td>
      <td>$4453.79</td>
    </tr>
    <tr>
      <th>3</th>
      <td>35235</td>
      <td>AL - Birmingham</td>
      <td>25</td>
      <td>$13998.28</td>
      <td>$5417.56</td>
      <td>$4129.16</td>
    </tr>
    <tr>
      <th>4</th>
      <td>35007</td>
      <td>AL - Birmingham</td>
      <td>18</td>
      <td>$31633.27</td>
      <td>$5658.33</td>
      <td>$4851.44</td>
    </tr>
  </tbody>
</table>
</div>


The first thing to check is always the **dimentions of the data** to understand not only the number of variables but also the number of records that we are dealing with. Here, we have 12 variables with 163,065 rows of records.


```python
df.shape
```




    (163065, 12)



The immediately next thing to check would be the **number of missing records**. This can be done using isnull() function of python.


```python
(df.isnull()).sum()
```




    DRG Definition                          0
    Provider Id                             0
    Provider Name                           0
    Provider Street Address                 0
    Provider City                           0
    Provider State                          0
    Provider Zip Code                       0
    Hospital Referral Region Description    0
     Total Discharges                       0
     Average Covered Charges                0
     Average Total Payments                 0
    Average Medicare Payments               0
    dtype: int64



Since there are no missing values, we don't need to take any measures to insert/delete records in this dataset for now.

Let's check **datatypes for each of the variable**.  
This will help us in analyzing numeric and categorical datatypes in separate ways.


```python
df_dtype = df.dtypes
df_dtype
```




    DRG Definition                          object
    Provider Id                              int64
    Provider Name                           object
    Provider Street Address                 object
    Provider City                           object
    Provider State                          object
    Provider Zip Code                        int64
    Hospital Referral Region Description    object
     Total Discharges                        int64
     Average Covered Charges                object
     Average Total Payments                 object
    Average Medicare Payments               object
    dtype: object



Let's rename the columns for our convenience. We can convert the column names to lowercase with an underscore between the words.


```python
df.columns = ['drg', 'provider_id', 'p_name',
       'p_address', 'p_city', 'p_state',
       'p_zip', 'h_ref_region',
       'total_discharge', 'avg_cov_charge',
       'avg_total_payment', 'avg_medicare_payment']
df.columns
```




    Index(['drg', 'provider_id', 'p_name', 'p_address', 'p_city', 'p_state',
           'p_zip', 'h_ref_region', 'total_discharge', 'avg_cov_charge',
           'avg_total_payment', 'avg_medicare_payment'],
          dtype='object')



We should **convert the datatypes to the right datatypes** to treat each variable in the right way.


```python
df[['provider_id', 'p_zip']]= df[['provider_id', 'p_zip']].astype(dtype= 'object')

#Removing $ from amounts while converting them from strings to numeric (Eg. $ 12,345 to 12345)
df[['avg_cov_charge', 'avg_total_payment', 'avg_medicare_payment']] = \
df[['avg_cov_charge', 'avg_total_payment', 'avg_medicare_payment']].replace('[\$,]', '', regex=True).astype('float').astype('int64')
df_dtype = df.dtypes
df_dtype
```




    drg                     object
    provider_id             object
    p_name                  object
    p_address               object
    p_city                  object
    p_state                 object
    p_zip                   object
    h_ref_region            object
    total_discharge          int64
    avg_cov_charge           int64
    avg_total_payment        int64
    avg_medicare_payment     int64
    dtype: object



Finally, we have categorical variables such as DRG, Provider details (Name, ID, Address, & etc.) and numeric variables such as Total number of discharges and a variety of charges for the treatments.  

**We will now analyze every variable for their distributions in order to understand the data and underlying processes better.**

### 2.1 DRG Codes <a name="drg"></a>

DRG Codes are the groups of similar medical conditions and procedures followed by hospitals. These codes will help us understand the treatments on a general level based on how that group of conditions is treated.


```python
pd.set_option('display.max_rows', 100)
df_drg = df['drg'].value_counts().reset_index()
df_drg.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>index</th>
      <th>drg</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>194 - SIMPLE PNEUMONIA &amp; PLEURISY W CC</td>
      <td>3023</td>
    </tr>
    <tr>
      <th>1</th>
      <td>690 - KIDNEY &amp; URINARY TRACT INFECTIONS W/O MCC</td>
      <td>2989</td>
    </tr>
    <tr>
      <th>2</th>
      <td>292 - HEART FAILURE &amp; SHOCK W CC</td>
      <td>2953</td>
    </tr>
    <tr>
      <th>3</th>
      <td>392 - ESOPHAGITIS, GASTROENT &amp; MISC DIGEST DIS...</td>
      <td>2950</td>
    </tr>
    <tr>
      <th>4</th>
      <td>641 - MISC DISORDERS OF NUTRITION,METABOLISM,F...</td>
      <td>2899</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_drg.tail()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>index</th>
      <th>drg</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>95</th>
      <td>315 - OTHER CIRCULATORY SYSTEM DIAGNOSES W CC</td>
      <td>859</td>
    </tr>
    <tr>
      <th>96</th>
      <td>473 - CERVICAL SPINAL FUSION W/O CC/MCC</td>
      <td>846</td>
    </tr>
    <tr>
      <th>97</th>
      <td>917 - POISONING &amp; TOXIC EFFECTS OF DRUGS W MCC</td>
      <td>843</td>
    </tr>
    <tr>
      <th>98</th>
      <td>251 - PERC CARDIOVASC PROC W/O CORONARY ARTERY...</td>
      <td>727</td>
    </tr>
    <tr>
      <th>99</th>
      <td>885 - PSYCHOSES</td>
      <td>613</td>
    </tr>
  </tbody>
</table>
</div>



These are the variety of treatments for which the amounts have been charged. Moving ahead, we can aggregate charges on this level to get an overall picture of costs associated with the diagnosis and treatments across USA.

### 2.2 Provider IDs <a name="p_id"></a>

Provider IDs are the hospital IDs that provided care for the patients. We have exactly 3337 unique hospitals in the dataset.


```python
len(df['provider_id'].unique())
```




    3337



Below are a few providers who provide only one treatment. Similarly, we can check for the number of treatments offerred by the other providers in the dataset. For this, we can count values of provider IDs and reset index to get a list as follows.


```python
df_p_id = df['provider_id'].value_counts().reset_index()
df_p_id[df_p_id['provider_id'] == 1].head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>index</th>
      <th>provider_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3242</th>
      <td>360274</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3243</th>
      <td>450860</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3244</th>
      <td>30131</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3245</th>
      <td>270086</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3246</th>
      <td>340168</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Statistics for the counts of treatments provided by the providers
df_p_id['provider_id'].describe()
```




    count    3337.000000
    mean       48.865748
    std        31.298726
    min         1.000000
    25%        20.000000
    50%        48.000000
    75%        78.000000
    max       100.000000
    Name: provider_id, dtype: float64



This signifies that there are 3337 healthcare providers, the maximum number of type of treatments provided by a provider are 100. Average number of treatments provided are 48.

Moving forward, to understand the distribution of DRGs across the providers, we can visualize the data as follows.


```python
#Treatment counts by Provider counts
df_p_id_counts = df_p_id['provider_id'].value_counts().reset_index()
df_p_id_counts = df_p_id_counts.sort_values(by='index')
df_p_id_counts.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>index</th>
      <th>provider_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>95</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2</td>
      <td>48</td>
    </tr>
    <tr>
      <th>7</th>
      <td>3</td>
      <td>47</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4</td>
      <td>50</td>
    </tr>
    <tr>
      <th>58</th>
      <td>5</td>
      <td>30</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Same with Seaborn Countplot
sns.set(font_scale=1.4)
plt.figure(figsize=(20, 25))
sns.countplot(y=df_p_id['provider_id'], color = 'orange')
plt.xlabel("Count of Providers providing the treatments (DRGs)")
plt.ylabel("Number of treatments at the provider")
plt.title("Number of Treatments Vs Number of Providers");
```


![png](https://raw.githubusercontent.com/gshahane/gshahane.github.io/master/_posts/Inpatients_Data_Medicare_CMS_files/Inpatients_Data_Medicare_CMS_29_0.png)


There is a sufficient representation for low, medium and high numbers of treatment providers in the dataset. We can subset the data if we want to analyze the data for general, multipurpose or specialty healthcare providers.

### 2.3 Provider City and Provider State <a name="p_citystate"></a>

We can check the distribution of providers and the treatments offerred by them in the dataset. This will help us in concluding fairly based on the dataset. For the states with not sufficient representation in the data, we can either exclude them from our broad conclusions or analyze them separately with respect to the numbers they generate. The difference between the records gathered from the variety of states could be based on population, area or number of cities covered in the respective states.


```python
d = df[['p_city', 'p_state']]
f = d.groupby(['p_state']).size().reset_index().sort_values(by = 0, ascending = False)
f.columns = ['p_state', 'count']
plt.figure(figsize = (20, 10))
sns.barplot(x = 'p_state', y = 'count', data = f,  palette="Blues_d" )
plt.show();
```


![png](https://raw.githubusercontent.com/gshahane/gshahane.github.io/master/_posts/Inpatients_Data_Medicare_CMS_files/Inpatients_Data_Medicare_CMS_33_0.png)


Some states are over represented in the data (like CA, TX, FL, NY) and some states are under-represented (like DE, VT, WY, AK). Around more than half the states that cover more than 2000 treatments have a sufficient enough number for any analysis. Any state-wise conclusions need to take in consideration the number of records from the states.

### 2.4 Total Discharge <a name="tot_discharge"></a>

Total Disclosures represent the total number of discharges filled by the provider during recording of the data. The number of discharges correspond to the charges mentioned in the columns next to it.


```python
df.total_discharge.describe()
```




    count    163065.000000
    mean         42.776304
    std          51.104042
    min          11.000000
    25%          17.000000
    50%          27.000000
    75%          49.000000
    max        3383.000000
    Name: total_discharge, dtype: float64



As the minimum of discharges are at least 11 for any treatment. The numbers seem okay to represent the costs associated with the treatment. The maximum number of discharges are 3383. However, since the next columns are averages of the costs associated with these discharges, the higher number of discharges from a few providers don't affect the average costs in a negative way. Instead, it is better to have a large number of sample of treatment costs to arrive at the right average figures for the next columns.  

Distribution of discharges are as follows.


```python
dis_count = df.total_discharge.value_counts().reset_index()
```


```python
plt.figure(figsize = (20,10))
fig_dist = sns.distplot(a = dis_count['index'], kde = False, rug = True)
fig_dist.set(xlabel = "Number of Discharges", ylabel = "Number of Providers")
plt.title("The number of Discharges by Providers count");
```


![png](https://raw.githubusercontent.com/gshahane/gshahane.github.io/master/_posts/Inpatients_Data_Medicare_CMS_files/Inpatients_Data_Medicare_CMS_39_0.png)


The following is the record with the discharge value.  
This is quite possible in a hospital in New York due to its dense population. We can keep the record for now.


```python
df[df['total_discharge']== 3383]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>drg</th>
      <th>provider_id</th>
      <th>p_name</th>
      <th>p_address</th>
      <th>p_city</th>
      <th>p_state</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>112969</th>
      <td>470 - MAJOR JOINT REPLACEMENT OR REATTACHMENT ...</td>
      <td>330270</td>
      <td>HOSPITAL FOR SPECIAL SURGERY</td>
      <td>535 EAST 70TH STREET</td>
      <td>NEW YORK</td>
      <td>NY</td>
    </tr>
  </tbody>
</table>
</div>

Dataset Continued..

<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>p_zip</th>
      <th>h_ref_region</th>
      <th>total_discharge</th>
      <th>avg_cov_charge</th>
      <th>avg_total_payment</th>
      <th>avg_medicare_payment</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>10021</td>
      <td>NY - Manhattan</td>
      <td>3383</td>
      <td>53113</td>
      <td>19023</td>
      <td>14880</td>
    </tr>
  </tbody>
</table>
</div>

### 2.5 Average Charges and Payments <a name="avg_cp"></a>  

**Average Coverage Charge:** Provider's average charge for the treatment.  
**Average Total Payments:** Average total payments made to the provider including payments made by the patient or any third parties.  
**Average Medicare Payments:** Average Payments made by Medicare for its share of the treatment charges.


```python
df_cp = df[['avg_cov_charge','avg_total_payment','avg_medicare_payment']]
df_cp.describe()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>avg_cov_charge</th>
      <th>avg_total_payment</th>
      <th>avg_medicare_payment</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>163065.000000</td>
      <td>163065.000000</td>
      <td>163065.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>36133.478840</td>
      <td>9707.016006</td>
      <td>8494.017717</td>
    </tr>
    <tr>
      <th>std</th>
      <td>35065.365859</td>
      <td>7664.635364</td>
      <td>7309.466560</td>
    </tr>
    <tr>
      <th>min</th>
      <td>2459.000000</td>
      <td>2673.000000</td>
      <td>1148.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>15947.000000</td>
      <td>5234.000000</td>
      <td>4192.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>25245.000000</td>
      <td>7214.000000</td>
      <td>6158.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>43232.000000</td>
      <td>11286.000000</td>
      <td>10056.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>929118.000000</td>
      <td>156158.000000</td>
      <td>154620.000000</td>
    </tr>
  </tbody>
</table>
</div>



Average coverage charges are **\$36133** for the given period for hospital-treatments.  
Average Total Payment and Average Medicare Payements are **\$9707** and **\$8494** approximately.   

There is one outlier for maximum values which we can explore more about the charge as below.


```python
df[df['avg_cov_charge'] == 929118]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>drg</th>
      <th>provider_id</th>
      <th>p_name</th>
      <th>p_address</th>
      <th>p_city</th>
      <th>p_state</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>39562</th>
      <td>207 - RESPIRATORY SYSTEM DIAGNOSIS W VENTILATO...</td>
      <td>50441</td>
      <td>STANFORD HOSPITAL</td>
      <td>300 PASTEUR DRIVE</td>
      <td>STANFORD</td>
      <td>CA</td>
    </tr>
  </tbody>
</table>
</div>

Dataset Continued..

<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>p_zip</th>
      <th>h_ref_region</th>
      <th>total_discharge</th>
      <th>avg_cov_charge</th>
      <th>avg_total_payment</th>
      <th>avg_medicare_payment</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>94305</td>
      <td>CA - San Mateo County</td>
      <td>11</td>
      <td>929118</td>
      <td>156158</td>
      <td>154620</td>
    </tr>
  </tbody>
</table>
</div>


The case is Stanford Hospital, from California.  
Let's plot the data and check if there are any more cases like this.


```python
sns.set(font_scale = 1.4)
f, axes = plt.subplots(3, figsize = (20, 10), sharex = True)
sns.distplot(a = df['avg_cov_charge'], ax = axes[0])
sns.distplot(a = df['avg_total_payment'], ax = axes[1])
sns.distplot(a = df['avg_medicare_payment'], ax = axes[2]);
```


![png](https://raw.githubusercontent.com/gshahane/gshahane.github.io/master/_posts/Inpatients_Data_Medicare_CMS_files/Inpatients_Data_Medicare_CMS_47_0.png)


Let's plot the same plot removing outliers.


```python
trial_cp = df[df['avg_cov_charge'] < np.percentile(a= df['avg_cov_charge'] , q=99)]

f, axes = plt.subplots(3, figsize = (20, 10), sharex = True)
sns.distplot(a = trial_cp['avg_cov_charge'], ax = axes[0])
sns.distplot(a = trial_cp['avg_total_payment'], ax = axes[1])
sns.distplot(a = trial_cp['avg_medicare_payment'], ax = axes[2]);
```


![png](https://raw.githubusercontent.com/gshahane/gshahane.github.io/master/_posts/Inpatients_Data_Medicare_CMS_files/Inpatients_Data_Medicare_CMS_49_0.png)


Removing the outlier, the distribution of the amount seems to be a bit normal with skew to the right.

### 2.6 Payments made by beneficiary and/or third party <a name="pay_ben"></a>

We need a new column for calculating the charges not covered by Medicare which will be a subtraction of Average Total Payment and Average Medicare Payment. These payments are made by beneficiary and/or Third parties.


```python
trial_cp['ben_payment'] = trial_cp['avg_total_payment'] - trial_cp['avg_medicare_payment']
trial_cp['ben_payment'].describe()
```

    C:\Users\gaura\Anaconda3\lib\site-packages\ipykernel\__main__.py:1: SettingWithCopyWarning:
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead

    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      if __name__ == '__main__':





    count    161434.000000
    mean       1198.384467
    std        1084.434915
    min           0.000000
    25%         776.000000
    50%         935.000000
    75%        1216.000000
    max       75999.000000
    Name: ben_payment, dtype: float64



Almost 75% of the payments are below **\$1200**. The share is significantly less than the total charges paid by Medicare **(\$8066).**  
Let's check the distribution.


```python
sns.set(font_scale = 1.4)
plt.figure(figsize=(20,10))
sns.distplot(a = trial_cp['ben_payment']);
```


![png](https://raw.githubusercontent.com/gshahane/gshahane.github.io/master/_posts/Inpatients_Data_Medicare_CMS_files/Inpatients_Data_Medicare_CMS_54_0.png)


Outlier as below.


```python
np.percentile(a = trial_cp['ben_payment'], q = 99)
```




    5357.6700000000128



Let's check the distribution without the outlier.


```python
sns.set(font_scale = 1.4)
plt.figure(figsize=(20,10))
sns.distplot(a = trial_cp['ben_payment']);
```


![png](https://raw.githubusercontent.com/gshahane/gshahane.github.io/master/_posts/Inpatients_Data_Medicare_CMS_files/Inpatients_Data_Medicare_CMS_58_0.png)


Again a normal distribution for the payments being done by beneficiary or third parties.  

### 3.1 Is Medicare really effective in bringing cost down for patients? <a name="compare"></a>
Let's analyze all the payments together in the below chart and visualization.


```python
trial_cp = trial_cp[trial_cp['ben_payment'] < np.percentile(a = trial_cp['ben_payment'], q = 99)]
trial_cp.describe()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>total_discharge</th>
      <th>avg_cov_charge</th>
      <th>avg_total_payment</th>
      <th>avg_medicare_payment</th>
      <th>ben_payment</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>159819.000000</td>
      <td>159819.000000</td>
      <td>159819.000000</td>
      <td>159819.000000</td>
      <td>159819.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>43.057678</td>
      <td>33510.831303</td>
      <td>9192.326250</td>
      <td>8066.525257</td>
      <td>1125.800994</td>
    </tr>
    <tr>
      <th>std</th>
      <td>51.406718</td>
      <td>26573.732407</td>
      <td>6443.517402</td>
      <td>6291.596767</td>
      <td>656.887381</td>
    </tr>
    <tr>
      <th>min</th>
      <td>11.000000</td>
      <td>2459.000000</td>
      <td>2673.000000</td>
      <td>1148.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>17.000000</td>
      <td>15799.000000</td>
      <td>5199.000000</td>
      <td>4156.000000</td>
      <td>775.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>27.000000</td>
      <td>24787.000000</td>
      <td>7116.000000</td>
      <td>6067.000000</td>
      <td>932.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>49.000000</td>
      <td>41772.000000</td>
      <td>10971.000000</td>
      <td>9816.000000</td>
      <td>1199.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>3383.000000</td>
      <td>176743.000000</td>
      <td>90701.000000</td>
      <td>89857.000000</td>
      <td>5357.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
sns.set(font_scale = 1.4)
fig, axes = plt.subplots(nrows = 3, ncols = 1, figsize = (20, 10), sharex = True )
#sns.distplot(a = trial_cp['avg_cov_charge'], ax = axes[0])
#axes[0].set_xlabel("Average Coverage Charge")
sns.distplot(a = trial_cp['avg_total_payment'], ax = axes[0])
axes[0].set_xlabel("Average Total Payment")
sns.distplot(a = trial_cp['avg_medicare_payment'], ax = axes[1])
axes[1].set_xlabel("Average Medicare Payment")
sns.distplot(a = trial_cp['ben_payment'], ax = axes[2])
axes[2].set_xlabel("Average Beneficiary Payment");
```


![png](https://raw.githubusercontent.com/gshahane/gshahane.github.io/master/_posts/Inpatients_Data_Medicare_CMS_files/Inpatients_Data_Medicare_CMS_62_0.png)


<span style ="color: blue"> **Observations:** </span>
We can observe that the Medicare is the **biggest** part of the payments made to providers. It helps patients by keeping the healthcare costs affordable for all the conditions. This is great for Medicare beneficieries.

### 3.2 Are there strong differences among healthcare costs for all US states? <a name="map"></a>

We can visually examine the geographical distribution of the Average Medicare Payment per treatment in order to assess cost of the healthcare in that state.


```python
#Log transform
import math
df_temp = df.groupby("p_state")['avg_medicare_payment'].mean().reset_index().sort_values("avg_medicare_payment", ascending = False)
df_temp1 = df_temp.copy()
```


```python
# Mapping the data
import folium
# definition of the boundaries in the map
state_geo = r'us-states.json'

US_COORDINATES=(37.0902, -98.7129)

# creation of the choropleth
map = folium.Map(location=US_COORDINATES, zoom_start=3.4)

map.choropleth(geo_path = 'state.geo.json',
              data = df_temp1,
              columns = ['p_state', 'avg_medicare_payment'],
              key_on = 'feature.properties.STUSPS10',
              fill_color = 'YlOrRd',
              fill_opacity = 0.7,
              line_opacity = 0.2,
              legend_name = 'Average Medicare Payment per treatment')  

map
```

![img](https://raw.githubusercontent.com/gshahane/gshahane.github.io/master/_posts/Inpatients_Data_Medicare_CMS_files/map.JPG)


<span style ="color: blue"> **Observations:** </span> As expected, Healthcare is costly in California and New York as the cost of living is on the higher side in these states. We can also observe higher healthcare costs in Alaska.

### 3.3 Top 10 Treatments (DRGs) that consume the Highest Average Medicare Payments <a name="topten"></a>


```python
df_temp = df.groupby('drg')['avg_medicare_payment'].mean().reset_index().copy()
df_temp.sort_values("avg_medicare_payment", ascending = False, inplace  = True)
df_temp = df_temp.head(10).copy()
df_temp
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>drg</th>
      <th>avg_medicare_payment</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>92</th>
      <td>870 - SEPTICEMIA OR SEVERE SEPSIS W MV 96+ HOURS</td>
      <td>41898.968051</td>
    </tr>
    <tr>
      <th>91</th>
      <td>853 - INFECTIOUS &amp; PARASITIC DISEASES W O.R. P...</td>
      <td>37818.265988</td>
    </tr>
    <tr>
      <th>21</th>
      <td>207 - RESPIRATORY SYSTEM DIAGNOSIS W VENTILATO...</td>
      <td>36029.077386</td>
    </tr>
    <tr>
      <th>52</th>
      <td>329 - MAJOR SMALL &amp; LARGE BOWEL PROCEDURES W MCC</td>
      <td>35457.778455</td>
    </tr>
    <tr>
      <th>66</th>
      <td>460 - SPINAL FUSION EXCEPT CERVICAL W/O MCC</td>
      <td>24043.126126</td>
    </tr>
    <tr>
      <th>30</th>
      <td>252 - OTHER VASCULAR PROCEDURES W MCC</td>
      <td>21367.408341</td>
    </tr>
    <tr>
      <th>26</th>
      <td>246 - PERC CARDIOVASC PROC W DRUG-ELUTING STEN...</td>
      <td>21052.381679</td>
    </tr>
    <tr>
      <th>67</th>
      <td>469 - MAJOR JOINT REPLACEMENT OR REATTACHMENT ...</td>
      <td>20859.329399</td>
    </tr>
    <tr>
      <th>23</th>
      <td>238 - MAJOR CARDIOVASC PROCEDURES W/O MCC</td>
      <td>19674.601518</td>
    </tr>
    <tr>
      <th>70</th>
      <td>480 - HIP &amp; FEMUR PROCEDURES EXCEPT MAJOR JOIN...</td>
      <td>19616.487621</td>
    </tr>
  </tbody>
</table>
</div>



We can plot the same data as follows:


```python
#Converting Medicare Payments into units of thousands for the ease of understanding the plots
df_temp["avg_medicare_payment"] = df_temp["avg_medicare_payment"]/1000
df_temp.sort_values("avg_medicare_payment", ascending = True, inplace = True)

sns.set(font_scale=2.5)
fig, ax = plt.subplots(figsize = (35,15))
y_pos = np.arange(len(df_temp["drg"]))
height = 0.8

ax.barh(y_pos,df_temp["avg_medicare_payment"], height , color='r')

plt.yticks(y_pos+height/2, df_temp["drg"])
plt.xlabel("Total Medicare Expenditure in USD thousands")
plt.ylabel("DRG codes")
plt.title("Top 10 Treatments (DRGs) that consume the highest Average Medicare Payments")
plt.tight_layout(pad=5)
plt.savefig("Top 10 Treatments (DRGs) that consume the highest Average Medicare Payments")
plt.show();
```


![png](https://raw.githubusercontent.com/gshahane/gshahane.github.io/master/_posts/Inpatients_Data_Medicare_CMS_files/Inpatients_Data_Medicare_CMS_72_0.png)


We can observe the most prevalent diagnosis and treatment procedures that consume the most Medicare Payments. Let's explore them in detail with respect to their overall spread as follows.


```python
#Creating a new variable that stores the above 10 DRGs.
drg_levels = list(df_temp.drg.unique())
drg_levels.reverse()

#Getting data ready for boxplot for the DRG codes indicated above.
df_box = df.copy()
df_box = df_box[df_box['drg'].isin(drg_levels)]
df_box.sort_values("avg_medicare_payment", ascending = False, inplace =  True)

```


```python

#X ticks creation
labels = dict([(x[:3], x[6:]) for x in drg_levels]);
labels_codes = [x[:3] for x in drg_levels];
x_pos = np.arange(len(drg_levels));

#Boxplot creation
sns.set(font_scale = 1.2);
fig = plt.figure(figsize=(15,12));
sns.set_style("white");
fig = sns.boxplot(x="drg", y="avg_medicare_payment", data=df_box, order = drg_levels,palette="PRGn");
plt.tight_layout(pad = 1);
plt.title("Top 10 Treatments (DRGs) that consume the Highest Average Medicare Payments ");
c = plt.xticks(x_pos,labels_codes);
c = fig.set(xlabel = "DRG Codes", ylabel = "Average Medicare Payment")


# table creation
col_labels=['DRG Codes Dictionary'];
row_labels= labels_codes;
table_vals=[[labels[x]] for x in labels_codes ]
# the rectangle is where I want to place the table
the_table = plt.table(cellText=table_vals,
                  colWidths = [0.1]*100,
                  rowLabels=row_labels,
                  colLabels=col_labels,
                  loc='upper right');
the_table.scale(8, 2)
plt.show();
#plt.text(12,3.4,'Table Title',size=8)
```









![png](https://raw.githubusercontent.com/gshahane/gshahane.github.io/master/_posts/Inpatients_Data_Medicare_CMS_files/Inpatients_Data_Medicare_CMS_75_1.png)


<span style ="color: blue"> **Observations:** </span> The results are surprising as two of the most prevalent diagnosis that consume the Highest Average Medicare Payments are related to **food poisoning/infections**. While these are avoidable conditions, it will need some work on the part of government to address the causes. This could be an opportunity to enhance health of citizens as well as a source of huge savings too.  

Another observation could be that the top four categories have a lot of spread (standard deviation) as well as outliers that may indicate severe condition of the patients. This is another indicator that requires some attention for any possibility of cost saving.

### Dataset Source:  
Centers for Medicare and Medicaid Services, Inpatient Charge Data FY 2011, Retrieved from https://www.cms.gov/Research-Statistics-Data-and-Systems/Statistics-Trends-and-Reports/Medicare-Provider-Charge-Data/Inpatient2011.html


```python

```
