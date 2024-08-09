# Time Series Analysis of CO<sub>2</sub> Emission

**Basic Info**:
Time Series Analysis of averaged monthly measurements CO<sub>2</sub> emission data from Mauna Loa observatory in Hawaii.
This study is an extension of the 3rd assignment of the course, 'Physics and Finance' at Uppsala University.
I have reformatted the data such that the data file ['ex3_1.dat'] now contains three columns: the date in decimal form, the averaged monthly CO<sub>2</sub> concentration expressed in μmole/mole of dry air, and the month as a number (Jan = 1,..., Dec = 12).

**Notebook**: 
All the analysis is collected in one jupyter-notebook, [TimeSeries_Analysis_CO2.ipynb](https://github.com/arnobmukherjee1988/TimeSeries_Analysis_CO2_emission/blob/main/TimeSeries_Analysis_CO2.ipynb).

**Analysis and Results**:
- Loading the datafile in pandas data frame with proper formatting (data formatting)
- Data preprocessing to check for null values, duplicate entries, outlier detection
- Visualization of the time series data
- Trend analysis by decomposing the series into trend, seasonality, and residual series.
- Observed moving average and moving standard deviation in the time series. The non-constant rolling mean and some variation in the rolling standard deviation suggest that the time series data is non-stationary.
- Confirmed and quantified the stationarity through ADF statistic and p-value from Augmented Dickey-Fuller (ADF) Test
- Used iterative method to make the series stationary:
    1. implement first differencing and do ADF test --> if not stationary go to next step
    2. implement second differencing and do ADF test --> if not stationary go to next step
    3. implement combined log and first-order differencing and do ADF test
- Resulted in low p-val from ADF after second step. We set d = 2
- Auto correlation function (ACF) and partial Auto correlation function (PACF) analysis to determine the significant lags
- Compared my own ACF, PACF code result with acf, pacf function from statsmodels.tsa.stattools library.
  Perfect match for ACF for all lags, match for PACF for lower lags
- Train, test data split
- ACF, PACF analysis results in p = 1, q = 1. Now we have our ARIMA model parameters (p, d, q) = (1, 2, 1)
- Implimented ARIMA (1, 2, 1)
- Analyzed the model performance via metrices Mean Absolute Error (MAE), Root Mean Square Error (RMSE).
  Found poor model performance, MAE: 0.983, RMSE: 1.151
- Looking at the seasonality in the series, implemented SARIMA(1, 2, 1) X (1, 1, 1, 12) with 12 month lags. Additionally implemented SARIMA(1, 2, 1) X (1, 1, 1, 6) with 6 month lags.
  SARIMA(1, 2, 1) X (1, 1, 1, 12): MAE = 0.5960.596, RMSE = 0.7630.763
  SARIMA(1, 2, 1) X (1, 1, 1, 6): MAE = 0.5470.547, RMSE = 0.6480.648 ==> better model performance
- I then ran auto-arima model to check the results from the optimum model parameters (2, 0, 0) X (2, 1, 0, 12) ==> no so good fit with relatively high MAE and RMSE.
  Manual model with ACF, PACF analysis performs better that auto-arima.
- Stock price forecast using Prophet model from Meta, best performance with MAE = 0.2115, RMSE = 0.2690.
" Prophet is a procedure for forecasting time series data based on an additive model where non-linear trends are fit with yearly, weekly, and daily seasonality, plus holiday effects. It works best with time series that have strong seasonal effects and several seasons of historical data." -- [Visit Prophet](https://facebook.github.io/prophet/)