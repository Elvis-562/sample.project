# Determining Which Aircrafts Offer Lower Risk To Business Stakeholders
## Project Overview
* A detailed findings of which aircraft  would offer lower risk to shareholders as they take in this new venture.
* Cleaning the data to ensure they're no missing values.
* Communicate insights using visualizations.
## Business Understanding
The goals for this project are:
* Analyze data from the National Transportation Safety Board from 1962 to 2023 that deal with aviation accidents.
* Find the shareholders what aircrafts offer low risks to invest in.
* Determine what aircrafts offer high risks to the investors.
* Provide information to the head of new aviation division on which aircrafts the shareholders should purchase.
Once this goals are met we can identify which aircrafts are suitable for any stakeholder to invest in with minimal loss and projections for profit in the future in the aviation industry.
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
Event.Id 	Investigation.Type 	Accident.Number 	Event.Date 	Location 	Country 	Latitude 	Longitude 	Airport.Code 	Airport.Name 	... 	Purpose.of.flight 	Air.carrier
Total.Fatal.Injuries 	Total.Serious.Injuries 	Total.Minor.Injuries 	Total.Uninjured 	Weather.Condition 	Broad.phase.of.flight 	Report.Status 	Publication.Date
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



