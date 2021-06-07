# btc_price_prediction
 This is a project on prediction of bitcoin price fluctuation using SARIMAX Model. 

#BITCOIN PRICE PREDICTION USING SARIMAX MODEL 

I also wrote a research paper on it and published in an international publication IJRSET which has a impact factor of 7.512. Link:- http://www.ijirset.com/upload/2020/november/37_6_Bitcoin1.pdf


#Step by Step Methodology

[Every step here is written with reference to the steps done in jupyter notebook file uploaded in the repository.So refer the file while going through these steps] 

First part is Data Analysis.Dataset is taken from Kaggle website and it is up-to-date and trustworthy data.It is a csv file containing bitcoin exchanges for the time period of January 2012 to September 2020, with minute to minute updates of OHLC (Open, High, Low, Close), Volume in BTC and indicated currency, and weighted bitcoin price. Timestamps are in Unix time.Timestamps without any trades or activity have their data fields filled with NaNs.Dataset csv file is read using pandas library in python. 

Before proceeding further ,Timestamp which is in Unix time should be converted to normal datetime format.After converting,it has been resampled into different frequencies-daily,monthly,yearly and quarterly.

After plotting the prices of bitcoin from 2012 to 2020 we can see that from 2012 to 2017 the bitcoin price was constant ,and thereafter there was an unusual fluctuation in the price.Now we have to convert this problem into time series to remove this unusual trend and effectively evaluate it.

Before finding out the parameters and building the model,we have to check whether the data is stationary or not.The criteria is that the p value should be less than 0.05 for the data to be stationary.Statistical test named Augmented dickey fuller test has to be done for the stationarity check of the data.After performing the test,the p value of original data came to be greater than 0.05 which means it is not stationary.

For making the data stationary , seasonal decomposition of the data has to be performed.After plotting the seasonal trend of the data can be seen i.e. the repeated variations that is occuring at specific intervals.This is the reason,Seasonal Autoregressive Integrative Moving Average with Exogenous Regressor (SARIMAX) model which is the extension of ARIMA model is more suitable as it supports the direct modeling of the seasonal component of the series.

For converting it into stationary,we have to do differencing.The differencing process is basically taking each datapoint and subtracting it to the previous data.First of all to convert the data into normality,Box-Cox transformation is used.After that, performing seasonal and regular differentiation results in a p value less than 0.05.
After doing this steps the plot will show that the data is stationary.

Now we can proceed further to building the model.For implementing the model,we have to find out the parameters that can build the best suited model.There are three parameters p,d,q of which p is the order of AR model i.e. AR model lags,q is the order of MA model i.e. MA model lags and d is the number of differencing needed to make the data stationary.The Initial approximation of parameters can be done using Autocorrelation and Partial Autocorrelation Plots.Identification of an AR model can be done with the help of PACF.For an AR model, the theoretical PACF sudden falls past the order of the model which means if there is a sudden drop in the partial autocorrelation after the first higher autocorrelation,that point will be the AR model lags.In our case as there is no sudden decrease in the autocorrelation,the AR model lags will be 0.Identification of an q parameter i.e.MA model lags is often best done with the ACF rather than the PACF.For an MA model, there will be a exponential decay rather than sudden decrease.That point of autocorrelation will be the q parameter.The exponential decay is at the point 1 after the first higher autocorrelation.Therefore the q parameter will be 1.As the number of regular differencing needed was 1,the d parameter will be 1.

After that,algorithm should be built to perform the model selection process.One of the criteria for the best model is that the AIC value should be minimum.The algorithm will choose the best suited model with parameters.This step will show the summary of the best model.As we approximated the p,d,q values before,the algorithm also shows the same result.This model has the minimum AIC compared to other models.The seasonal AR part with 12 months lag(ar.S.L12) and seasonal MA part with 12 months lag(ma.S.L12) has the minimum p value which means it is significant and it is best suited to include in the model.Apart from p,d and q parameters of regular ARIMA model ,there are another three parameters P,D,Q for seasonality.As there was one seasonal differencing needed to make the data stationary,the D parameter will be 1.Therefore the best model suited is SARIMAX(0, 1, 1)x(2, 1, 1, 12) where the order p=0,d=1,q=1 and seasonal order P=2,D=1,Q=1 and 12 is the seasonal lags.

As we have done the box-cox transformation before,inverse of box-cox transformation has to be done to transform the forecasted values back to their original units.After this process,we can proceed to prediction part. These are the steps to process the data and apply the algorithm.
