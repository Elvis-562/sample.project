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

```python
|Event.Id |Investigation.Type| 	Accident.Number |	Event.Date| 	Location 	|Country |	Latitude |	Longitude |	Airport.Code |	Airport.Name 	... |	Purpose.of.flight |	Air.carrier
|Total.Fatal.Injuries 	|Total.Serious.Injuries 	|Total.Minor.Injuries 	|Total.Uninjured |	Weather.Condition| 	Broad.phase.of.flight |	Report.Status |	Publication.Date|
0 	20001218X45444 	Accident 	SEA87LA080 	1948-10-24 	MOOSE CREEK, ID 	United States 	NaN 	NaN 	NaN 	NaN 	... 	Personal 	NaN 	2.0 	0.0 	0.0 	0.0 	UNK 	Cruise 	Probable Cause 	NaN
1 	20001218X45447 	Accident 	LAX94LA336 	1962-07-19 	BRIDGEPORT, CA 	United States 	NaN 	NaN 	NaN 	NaN 	... 	Personal 	NaN 	4.0 	0.0 	0.0 	0.0 	UNK 	Unknown 	Probable Cause 	19-09-1996
2 	20061025X01555 	Accident 	NYC07LA005 	1974-08-30 	Saltville, VA 	United States 	36.922223 	-81.878056 	NaN 	NaN 	... 	Personal 	NaN 	3.0 	NaN 	NaN 	NaN 	IMC 	Cruise 	Probable Cause 	26-02-2007
3 	20001218X45448 	Accident 	LAX96LA321 	1977-06-19 	EUREKA, CA 	United States 	NaN 	NaN 	NaN 	NaN 	... 	Personal 	NaN 	2.0 	0.0 	0.0 	0.0 	IMC 	Cruise 	Probable Cause 	12-09-2000
4 	20041105X01764 	Accident 	CHI79FA064 	1979-08-02 	Canton, OH 	United States 	NaN 	NaN 	NaN 	NaN 	... 	Personal 	NaN 	1.0 	2.0 	NaN 	0.0 	VMC 	Approach 	Probable Cause 	16-04-1980
... 	... 	... 	... 	... 	... 	... 	... 	... 	... 	... 	... 	... 	... 	... 	... 	... 	... 	... 	... 	... 	...
88884 	20221227106491 	Accident 	ERA23LA093 	2022-12-26 	Annapolis, MD 	United States 	NaN 	NaN 	NaN 	NaN 	... 	Personal 	NaN 	0.0 	1.0 	0.0 	0.0 	NaN 	NaN 	NaN 	29-12-2022
88885 	20221227106494 	Accident 	ERA23LA095 	2022-12-26 	Hampton, NH 	United States 	NaN 	NaN 	NaN 	NaN 	... 	NaN 	NaN 	0.0 	0.0 	0.0 	0.0 	NaN 	NaN 	NaN 	NaN
88886 	20221227106497 	Accident 	WPR23LA075 	2022-12-26 	Payson, AZ 	United States 	341525N 	1112021W 	PAN 	PAYSON 	... 	Personal 	NaN 	0.0 	0.0 	0.0 	1.0 	VMC 	NaN 	NaN 	27-12-2022
88887 	20221227106498 	Accident 	WPR23LA076 	2022-12-26 	Morgan, UT 	United States 	NaN 	NaN 	NaN 	NaN 	... 	Personal 	MC CESSNA 210N LLC 	0.0 	0.0 	0.0 	0.0 	NaN 	NaN 	NaN 	NaN
88888 	20221230106513 	Accident 	ERA23LA097 	2022-12-29 	Athens, GA 	United States 	NaN 	NaN 	NaN 	NaN 	... 	Personal 	NaN 	0.0 	1.0 	0.0 	1.0 	NaN 	NaN 	NaN 	30-12-2022

88889 rows × 31 columns
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
count 	82805.000000 	        77488.000000 	         76379.000000 	          76956.000000       	82977.000000
mean 	  1.146585 	             0.647855 	           0.279881 	              0.357061 	          5.325440
std 	  0.446510 	             5.485960 	           1.544084 	              2.235625            27.913634
min 	  0.000000 	             0.000000              0.000000 	              0.000000 	          0.000000
25% 	  1.000000       	       0.000000              0.000000 	              0.000000 	          0.000000
50% 	 1.000000 	            0.000000 	             0.000000 	              0.000000 	          1.000000
75% 	  1.000000 	             0.000000 	           0.000000 	              0.000000 	          2.000000
max 	  8.000000 	             349.000000 	         161.000000 	            380.000000 	        699.000000
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
## Conclusion
* The most common type of aircraft used for travel is Cessna and it's also the riskiest since it has the highest number of fatalities among the aircraft.
* Boeing is the safest aircraft and I would advise the business shareholders to purchase the Boeing Aircraft since it offers low risk.
* The most common reason for travel is personal flights and so I  would recommend the shareholders to invest in personal flights with the Boeing aircraft.
* The weather condition mostly responsible for accidents or fatal injuries is VMC.
* Most accidents are due to human error since IMC has less number of Total fatal and serious injuries compared to VMC.
* Majority of the Injuries are non-fatal.





