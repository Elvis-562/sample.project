# Determining Which Aircrafts Offer Lower Risk To Business Stakeholders
## Project Overview
* A detailed findings of which aircraft  would offer lower risk to shareholders as they take in this new venture.
* Cleaning the data to ensure there no missing values.
* Communicate insights using visualizations.
## Business Understanding
The goals for this project are:
* Analyze data from the National Transportation Safety Board from 1962 to 2023 that deal with aviation accidents.
* Find the shareholders what aircrafts offer low risks to invest in.
* Determine what aircrafts offer high risks to the investors.
* Provide information to the head of new aviation division on which aircrafts the shareholders should purchase.
Once these goals are met we can identify which aircrafts are suitable for any stakeholder to invest in with minimal loss and projections for profit in the future in the aviation industry.
## Data Understanding
The data  comes from [Kaggle](https://www.kaggle.com/datasets/khsamaha/aviation-accident-database-synopses) which has information about aviation accidents from 1962 to 2023.

To load the dataset we use pandas library.

```python
import pandas as pd
import warnings
warnings.filterwarnings("ignore")
df=pd.read_csv("AviationData.csv",encoding= "latin-1",low_memory=False)
df
```
The data contains 31 columns, from this data we should be able to identify aircrafts that offer high and low risks to the shareholders.

This data contains missing values so my first step is to clean this data.

```python
#We can get information about this dataset
df.info()
```
```python
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 88889 entries, 0 to 88888
Data columns (total 31 columns):
 #   Column                  Non-Null Count  Dtype  
---  ------                  --------------  -----  
 0   Event.Id                88889 non-null  object 
 1   Investigation.Type      88889 non-null  object 
 2   Accident.Number         88889 non-null  object 
 3   Event.Date              88889 non-null  object 
 4   Location                88837 non-null  object 
 5   Country                 88663 non-null  object 
 6   Latitude                34382 non-null  object 
 7   Longitude               34373 non-null  object 
 8   Airport.Code            50132 non-null  object 
 9   Airport.Name            52704 non-null  object 
 10  Injury.Severity         87889 non-null  object 
 11  Aircraft.damage         85695 non-null  object 
 12  Aircraft.Category       32287 non-null  object 
 13  Registration.Number     87507 non-null  object 
 14  Make                    88826 non-null  object 
 15  Model                   88797 non-null  object 
 16  Amateur.Built           88787 non-null  object 
 17  Number.of.Engines       82805 non-null  float64
 18  Engine.Type             81793 non-null  object 
 19  FAR.Description         32023 non-null  object 
 20  Schedule                12582 non-null  object 
 21  Purpose.of.flight       82697 non-null  object 
 22  Air.carrier             16648 non-null  object 
 23  Total.Fatal.Injuries    77488 non-null  float64
 24  Total.Serious.Injuries  76379 non-null  float64
 25  Total.Minor.Injuries    76956 non-null  float64
 26  Total.Uninjured         82977 non-null  float64
 27  Weather.Condition       84397 non-null  object 
 28  Broad.phase.of.flight   61724 non-null  object 
 29  Report.Status           82505 non-null  object 
 30  Publication.Date        75118 non-null  object 
dtypes: float64(5), object(26)
memory usage: 21.0+ MB
```

This shows us the number of columns and rows in the dataset.It also shows the number of values in each column.
```python
df.describe()
```
```python
       	Number.of.Engines 	Total.Fatal.Injuries 	Total.Serious.Injuries 	Total.Minor.Injuries 	Total.Uninjured
count 	   82805.000000       77488.000000 	        76379.000000 	         76956.000000          82977.000000
mean 	    1.146585      	    0.647855 	              0.279881 	           0.357061 	            5.325440
 std 	     0.446510 	          5.485960              	1.544084 	          2.235625 	            27.913634
min      	0.000000 	          0.000000 	            0.000000             	0.000000 	            0.000000
25%      	1.000000 	         0.000000 	             0.000000 	            0.000000 	            0.000000
50%      	1.000000 	         0.000000 	             0.000000 	            0.000000 	            1.000000
75% 	     1.000000 	         0.000000 	             0.000000 	            0.000000 	            2.000000
max 	     8.000000 	        349.000000 	            161.000000 	          380.000000 	          699.000000
```

This shows the aggregation data of the numeric values in the dataset.
```python
# We can now get the number of missing values in the dataset
df.isnull().sum()
```
```python
Event.Id                      0
Investigation.Type            0
Accident.Number               0
Event.Date                    0
Location                     52
Country                     226
Latitude                  54507
Longitude                 54516
Airport.Code              38757
Airport.Name              36185
Injury.Severity            1000
Aircraft.damage            3194
Aircraft.Category         56602
Registration.Number        1382
Make                         63
Model                        92
Amateur.Built               102
Number.of.Engines          6084
Engine.Type                7096
FAR.Description           56866
Schedule                  76307
Purpose.of.flight          6192
Air.carrier               72241
Total.Fatal.Injuries      11401
Total.Serious.Injuries    12510
Total.Minor.Injuries      11933
Total.Uninjured            5912
Weather.Condition          4492
Broad.phase.of.flight     27165
Report.Status              6384
Publication.Date          13771
dtype: int64
````

The only columns that don't contain missing values are Event.Id, Investigation.Type, Accident.Number and Event.Date

Since some of columns have large number of missing values we will drop the columns, such columns include:
* Air carrier
* Latitude
* Longitude
* FAR.Description
```python
#drop column containing large number of missing values which include  Air.carrier , Latitude  and many others.
df=df.drop(["Air.carrier","Latitude","Longitude","FAR.Description","Schedule","Aircraft.Category","Airport.Code","Airport.Name","Amateur.Built"],axis=1)
df
```
 
We can also drop  rows and fill in null values using mean, mode and median.

I dropped rows in columns containing  few missing values which include columns make, model, location and Country

I filled the  missing values in the rest of the categorical data such as Purpose of flight and Injury severity with mode.

For the numerical data I filled in the null values with the median of each respective column.

I also removed duplicates in the data using
```python
#check for  duplicates 
df.duplicated().any()
```
```python
True
```

Since duplicates were present, I went ahead and removed the duplicates using the code shown below.
```python
#removing duplicates
df.drop_duplicates(subset=["Event.Id","Investigation.Type","Accident.Number","Event.Date","Location","Country","Injury.Severity","Make","Model","Purpose.of.flight","Total.Fatal.Injuries","Total.Uninjured"],keep="first",inplace= True)
df
```

The number of duplicates was just two.
## Data Analysis

This is where I made  use of visualizations to generate insights for the business stakeholders.

Here, I am going to display the  visuals that I used in tableau.

![Top 10 Riskiest Aircrafts](https://github.com/user-attachments/assets/fc3f8fca-3649-43c8-a6fb-b65abf99258f)
This Bar chart shows the relationship between total fatal injuries and make.
## Observation
* The make with the highest number of fatalities is Cessna hence Cessna will offer high risks if shareholders were to invest in the aircraft.
   

![Top 10 Safest Aircrafts](https://github.com/user-attachments/assets/5c029dad-9ffa-4e66-8688-2d20173c12f0)
## Observation
* The safest aircraft as displayed by the bar chart is Boeing.
  
![Total Uninjured by Purpose Of Flight(1)](https://github.com/user-attachments/assets/700ffe7f-0d7d-4e40-b4d5-3922e557534c)

This is  a bar chart showing the  relationship between purpose of flight and the number of uninjured passengers.
## Observation
* The most common reason for passengers to take flights is personal flights.
## Interactive dashboard

I made use of three interactive dashboards.
![Dashboard Comparing Riskiest and Safest Aircrafts](https://github.com/user-attachments/assets/02f67a0a-1b80-4313-a448-49ede1d9f8b5)

This is a dashboard comparing the riskiest and the safest aircraft.

![Dashboard Comparing Purpose of flight](https://github.com/user-attachments/assets/9d111a31-74cb-4050-a74b-95d6a03772ef)

This is a  dashboard comparing the total fatal injuries and uninjured in purpose of flight.

![Dashboard Comparing Fatal Injuries and Uninjured by Model and Weather Condition](https://github.com/user-attachments/assets/5e2763ad-5511-498d-825b-8d41291aa7b4)

The dashboard above compares total fatal injuries and total uninjured by model and weather condition.

## Conclusion
* The most common type of aircraft used for travel is Cessna and it's also the riskiest since it has the highest number of fatalities among the aircraft.
* Boeing is the safest aircraft and I would advise the business shareholders to purchase the Boeing Aircraft since it offers low risk.
* The most common reason for travel is personal flights and so I  would recommend the shareholders to invest in personal flights with the Boeing aircraft.
* The weather condition mostly responsible for accidents or fatal injuries is VMC.
* Most accidents are due to human error since IMC has less number of Total fatal and serious injuries compared to VMC.
* Majority of the Injuries are non-fatal.





