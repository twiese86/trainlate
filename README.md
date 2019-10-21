## Udacity ML Capstone - Thomas Wiese
# Domain Background
The Long Island Railroad operates one of the largest commuter rail networks in the U.S.. As an employee, I have access to nonpublic data which includes the location and arrival time of trains based on onboard GPS position. This data is known internally as TIMACS. 

A few academic papers have been published attempting to predict train delays including:

http://www.diva-portal.org/smash/get/diva2:1217917/FULLTEXT01.pdf

This paper inspired some parts of this analysis, the engineers here used RFR:

https://arxiv.org/pdf/1806.02825.pdf

I’d also be interested in publishing my results in a scholarly journal if I can get permission from the LIRR to share the data. I am currently a Ph.D. student in Information Science and it is in my best interest to expand my publication record.

# Problem Statement
At the Long Island Railroad, train delays occur between stations due to a number of factors. Train delays can have a negative impact on customer experience and cause a negative reputational impact. For this study, TIMACS train location data and the LIRR train schedule will be compared to determine areas that experience delays at a high frequency. Further, train delays when heading West during the AM peak for trains which arrive at Penn Station are of particular concern. I intend to develop a Machine Learning model which predicts how late individual trains will arrive at Penn Station.

# Datasets and Inputs

The TIMACS dataset being utilized will be during the month of February 2019 and contain the following data:

<class 'pandas.core.frame.DataFrame'>
RangeIndex: 1334202 entries, 0 to 1334201
Data columns (total 33 columns):
FULL_TRAIN_NUM           1334202 non-null object
TRAIN_NUM                1334202 non-null object
VERSION                  1334202 non-null int64
RUN_DATE                 1334202 non-null object
DIRECTION                1334202 non-null object
PASS_BRANCH_NAME         1301909 non-null object
IS_REVENUE               1334202 non-null int64
IS_DIESEL                1334202 non-null int64
PEAK_CD                  1334202 non-null object
ACT_LEAD_ENGINE          1324087 non-null object
ACT_TAIL_ENGINE          156640 non-null float64
INSERTED                 1334202 non-null int64
CANCELLED                1334202 non-null int64
CANCELLED_AT_LOCATION    20323 non-null object
SCHED_CAR_COUNT          1301909 non-null float64
ACT_CAR_COUNT            1324087 non-null float64
ACT_LEAD_ENGINE.1        1324087 non-null object
ACT_TAIL_ENGINE.1        156640 non-null float64
CREW_NUM                 1247280 non-null object
SEQUENCE_NUM             1334202 non-null int64
LOCATION_CODE            1334202 non-null object
LOCATION                 1334202 non-null object
SCH_DTM                  606458 non-null object
SCH_STOP_TYPE            232489 non-null object
SCH_TRACK                606458 non-null object
ARR_TRACK                15293 non-null object
GPS_ARR_DTM              1143161 non-null object
GPS_ARR_SOURCE_TYPE      1143161 non-null object
GPS_ARR_OTP              415692 non-null float64
DEP_TRACK                219276 non-null object
GPS_DEP_DTM              218422 non-null object
GPS_DEP_SOURCE_TYPE      218422 non-null object
GPS_DEP_OTP              217844 non-null float64
dtypes: float64(6), int64(6), object(21)
memory usage: 335.9+ MB

Some fields that I believe will be relevant include FULL_TRAIN_NUM, LOCATION, SCH_DTM, SCH_STOP_TYPE, GPS_ARR_DTM, GPS_ARR_OPT
----------------------------------------------------------
    
>   - FULL_TRAIN_NUMBER - The numerical designation for all train cars associated with the train
    - LOCATION - The location of the train at the time the record was generated
    - SCH_DTM - The time that the train is scheduled to arrive based on the LIRR timetable
    - SCH_STOP_TYPE - The type of location that the train was at when the record was generated. S is equivalent to a train station and is what we are interested in for the purpose of this study.
    - GPS_ARR_DTM - The time that the GPS record was actually generated when the train arrived
    - GPS_ARR_OPT = SCH_DTM - GPS_ARR_DTM; The difference between the scheduled arrival time and the actual GPS arrival time in seconds. **There is a notable issue here**, SCH_DTM and GPS_ARR_DTM do not include seconds in this data file. I believe we can reasonably assume that SCH_DTM is always at 0 seconds after the minute, however we would need the raw data to determine the GPS_ARR_DTM variation from schedule.

# Solution Statement
We will predict how late a given west bound peak train will arrive at Penn Station

# Benchmark Model
Since this model intends to predict a nonlinear continuous feature, a random forest decision tree regression will be used.

In the reviewers comments they suggested using linear regression. However, there is a time series element to this model and linear regression doesn’t seem appropriate. I will utilize xgBoost for the final model and RFR from sklearn as the benchmark.

# Evaluation Metrics
Mean Squared Error will be the evaluation metric since it is resistant to outliers

# Project Design
The data will be cleaned, analyzed, and normalized if required. Then the RFR model will be trained and the MSE will be evaluated for the testing set in order to determine model effectiveness.

To further clarify, I will drop columns that are not relevant to my problem. I will fill null values where needed, and I will convert df elements to the appropriate data types. I will also perform encoding where necessary to transform categorical variables into integers.


