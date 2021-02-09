# travel-time-prediction
Estimation of ETA and ETD from the Train Travel Time Data

## 1. Importing Libraries

## 2. Load Data 

## 3. Basic Info on Data

|runDate |	stations|trainCode|	trainStationId	|scheduledArrival|	scheduledDeparture	|actualArrival|	actualDeparture|	distance|	dayCount|	ArrivalDelay	|DepartureDelay|

|----------|------------|----------|---------|---------|--------|-------|--------|-------|-------|------|----------|---|
0	2020-01-01	FZR	12138	2657812	NaN	2020-01-01 21:40:00	2020-01-01 00:00:00	2020-01-01 21:40:00	0	0	0	0
1	2020-01-01	FDK	12138	2659393	2020-01-01 22:05:00	2020-01-01 22:07:00	2020-01-01 22:16:00	2020-01-01 22:18:00	32	0	11	11
2	2020-01-01	KKP	12138	2658108	2020-01-01 22:23:00	2020-01-01 22:25:00	2020-01-01 22:37:00	2020-01-01 22:38:00	45	0	14	13
3	2020-01-01	GJUT	12138	2734206	2020-01-01 22:38:00	2020-01-01 22:40:00	2020-01-01 22:56:00	2020-01-01 22:57:00	61	0	18	17
4	2020-01-01	GNA	12138	2742572	2020-01-01 22:54:00	2020-01-01 22:56:00	2020-01-01 23:14:00	2020-01-01 23:15:00	76	0	20	19
## 4. Correcting Inconsistencies

## 5. Data Wrangling, Feature Engineering and EDA

1. Finding Unique Values in every Feature
2. Dropping Redundant Columns
3. Imputing NaN
4. Dropping some feautures
5. Breaking `datetime` to columns namely year, month, date, hours, minutes, secs


``` Unique Values in runDate =  (50,)  
 Unique Values in stations =  (349,)  
 Unique Values in trainCode =  (15,)  }
 Unique Values in trainStationId =  (349,)  
 Unique Values in sA =  (24480,)  
 Unique Values in sD =  (23505,)  
 Unique Values in aA =  (25594,)  
 Unique Values in aD =  (25496,)  
 Unique Values in distance =  (573,)  
 Unique Values in dayCount =  (3,)  
 Unique Values in ArrivalDelay =  (501,)  
 Unique Values in DepartureDelay =  (455,) 

```


After finding the unique values in all the features we get to know that the data is from the time span runDate 2020-01-01 to 2020-02-19 among 337 stations with 15 trains trainCode . Train Stations having their 337 trainStationID with Arrival and Departure frequency of 25051 in this time span. Trains take around dayCount from 0 to 2

Since, stations are already coded with their trainStationID we can remove one of them

### 5.1 After Imputing the data
Now we know that ArrivalDelay, DepartureDelay were derived columns but we can use those columns to derive the missing values in scheduledArrival and scheduledDeparture.

For the missing values in test sA and sD; train sD. We will drop those values in both test and train.

### 5.2  Dropping some features

Since `Arrival Delay` is a derived column we can remove the `actualArrival` and `actualDeparture` from the train data

### 5.3 Breaking down the datetime values

1. Dropping the year 2020 from all the datetime formats
2. Dropping the `second` from datetime values. 
3. Breaking all the date time values in `test` and `train` data.

Not taking year and seconds because of having the same value all along

## 6. Model Selection

1. DecisionTreeRegressor
2. RandomForestRegressor


## 6.1 Feature Importance

```
Feature Importance:

trainCode -- 0.25
trainStationId -- 0.25
distance -- 0.14
dayCount -- 0.13
runDate_month -- 0.08
runDate_date -- 0.04
sA_month -- 0.03
sA_day -- 0.02
sA_minute -- 0.02
sA_hour -- 0.01
sD_month -- 0.01
sD_day -- 0.01
sD_hour -- 0.01
sD_minute -- 0.01
```

## 6.2 Score Values
Checked whether feature Importance is bogus by removing the `trainCode` feature and got to know that regressor score becomes `0.1113` but when the `trainCode` is supplied the score becomes mpre than `93`. But since `DepartureDelay` cannot be passed the score remains out to be `0.77763`.

## 7. Model Building
