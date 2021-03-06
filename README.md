
# Eliminating Trend and Seasonality

## Objectives

* Remove non-stationarity of complex time-series with high seasonality.
* Use Differencing and Decomposition techniques for making time-series stationary. 
* Compare the effectiveness of differencing vs. decomposition. 

## Introduction

The simple trend reduction techniques discussed before don’t work in all cases, particularly the ones with high seasonality. Lets discuss two ways of removing trend and seasonality for such time-series. 

* **Differencing** – taking the differece with a particular time lag
* **Decomposition** – modeling both trend and seasonality and removing them from the model.

Let's once again import the passengers dataset and the `stationarity_check()` function from previous to apply these techniques and monitor the effect. 



```python
#Import necessary libraries
import warnings
warnings.filterwarnings('ignore')

import pandas as pd
from pandas import Series
import numpy as np

import matplotlib.pylab as plt
%matplotlib inline
plt.style.use('fivethirtyeight')

from matplotlib.pylab import rcParams
rcParams['figure.figsize'] = 15, 6

# Import adfuller


# Import the check_stationarity function from previous lab

# Import passengers.csv and set it as a time-series object. Plot the TS


```

## Differencing

One of the most common methods of dealing with both trend and seasonality is differencing. In this technique, we take the difference of the observation at a particular instant with that at the previous instant (i.e. Lag).  This mostly works well in improving stationarity. First order differencing can be done in Pandas using the `shift()` function to shift the values in the index by a specified number of units of the index's period. Details of `.shift()` can be viewed [HERE](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.shift.html).

1. Take the log of ts. 
2. Apply `shift()` to the output of 1. Use period of 1 (set by default)
3. subtract result of 2 from 1.
4. Plot the resulting time-series


```python
# Calculate the difference of log transformed ts with shifted log transformed ts and plot.
ts_log = None
ts_log_diff = None
```

This appears to have reduced trend considerably. Lets verify if this actually worked. First drop the NaN values and then use `stationarity_check()` function. 


```python
# Drop NANs and check for stationarity

```

We can see that the mean and std variations have small variations with time. Also, the Dickey-Fuller test statistic is less than the 10% critical value, thus the TS is stationary with 90% confidence. We can also take second or third order differences which might get even better results in certain applications. 

> **BONUS EXERCISE** Use second and third order differencing with `shift()` and compare the results. 

## Decomposing

Time series decomposition is a mathematical procedure which transforms a time series into multiple different time series. The original time series is often split into 3 component series:

>Seasonal: Patterns that repeat with a fixed period of time. For example, a website might receive more visits during weekends; this would produce data with a seasonality of 7 days.

>Trend: The underlying trend of the metrics. A website increasing in popularity should show a general trend that goes up.

>Random: Also call “noise”, “irregular” or “remainder,” this is the residuals of the original time series after the seasonal and trend series are removed.

### Additive or Multiplicative Decomposition?

To achieve successful decomposition, it is important to choose between the additive and multiplicative models, which requires analyzing the series. For example, does the magnitude of the seasonality increase when the time series increases?

![](http://kourentzes.com/forecasting/wp-content/uploads/2014/11/mseas.fig1_.png)


Fortunately, `statsmodels` provides the convenient `seasonal_decompose` function to perform such decomposition out of the box. Details of this function are available [HERE](http://www.statsmodels.org/dev/generated/statsmodels.tsa.seasonal.seasonal_decompose.html). 

Let's use this function to perform following tasks:
1. Import `seasonal_decompose` from statsmodels.
2. Apply `seasonal_decompose` to log transformed TS. 
3. Plot the trend, seasonality and residual. 


```python
# import seasonal_decompose
from statsmodels.tsa.seasonal import seasonal_decompose
decomposition = None

# Gather the trend, seasonality and noise of decomposed object
trend = None
seasonal = None
residual = None

# Plot gathered statistics

```

Here we can see that the trend, seasonality are separated out from data and we can model the residuals.

Using time-series decomposition makes it easier to quickly identify a changing mean or variation in the data. The plot above clearly shows the upwards trend of our data, along with its yearly seasonality. These can be used to understand the structure of our time-series. The intuition behind time-series decomposition is important, as many forecasting methods build upon this concept of structured decomposition to produce forecasts.

Lets check stationarity of residuals using our `check_stationarity()` function.

NOTE: drop NaN values first. 


```python
# Drop NaN values from residuals.


# Check stationarity

```

The Dickey-Fuller test statistic is significantly lower than the 1% critical value. So this TS is very close to stationary. We can try more advanced decomposition techniques as well which can generate better results.

## Summary: 
In this lesson, we saw how to remove both trend and seasonality from a time-series using differencing and decomposition. We saw how decmoposition allows us to better model the time-series for further analysis. After these steps, we can perform predictive analyses and modelling with time-series data which will be covered in following lab. 
