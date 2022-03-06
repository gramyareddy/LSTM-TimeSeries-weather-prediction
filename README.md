# LSTM-TimeSeries-weather-prediction
The folder consists of :
1. Datsets: Split to Training of 15Yrs' data, test on last 3Yrs' data
2. DeepLearning: Consists of DeepLearning solution, the training model, the ipy notebook of training, inference.
I was able to implement only the deep learning solution because of time constraint and had other interviews lined up. wanted to try SARIMA with a univariate version and VARMAX maybe for including a few other params
3. MapBox plots: has original and forecasted temperatures plotted on US map


Data Preprocessing:
===================
1. Cleaned the data(removed NaN)
2. Took 18yrs' data for train test
3. Replicated covariates for all Lat/Lons on each day with respective temperatures
4. From Targets, I will be predicting the tmp_2m value and hence took that as Y and ignored its statistical components.
5. Using all the covariates as features for models, the pacific sea surface temp, atlantic sst, ENSO index, Nino indices for EL Ninoeffects... reading up about all these terms was fun , though dint get the hang of it completely yet
6. SORTED The data such that entire timeframe's data is clubbed together for each Lat Lon,
Since we'll be training the data up in sequence : the data format is such that for Lat/Lon 1: data for all 15Yrs is present, followed by next area and so on.....


Deep Learning Solution:
=======================

Data format
----------
1. A lag of 14 is taken: i.e: On a given day, in ordder to predict the average temperature of two weeks, starting two weeks from now, Im using past data of 2 week timeframe:
 So to predict on 01.01.2020 , the avg temperatures of 15.01.2020-22.01.2020 , I use the data 19.12.2019-01.01.2020
2. All the data is sorted on Lat/Lon, such that data , of same area is clubbed together.
3.Trained on a basic LSTM model with 64 units on 2 layers for around 20 epochs
4. Inference/ test is done such that for a given date , 197 predictions are made, 1 each for each lat/lon, and taking past 2week values of each location seperately 
5. Moreover on date of prediction, I use only past values and never the future ones, so that no data leakage occurs.

Results : ERROR/ACCURACY  : TEST RMSE 1.04

RESULTS CSV
===========================
DeepLearning/result_temperature_forecast.csv has the results , date ,lat lon, original temp and forecasted temperature, The error was low, only 1-2 deg of error overall
The results have been made for everday from 2019.07-2021-07 where each day I predict the avg of 2weeks,2 weeks from then.

Plotly: Plotting the  temperatures with mapbox
==============================================
Plotted both original and predicted temperatures as color density map for the given dates
