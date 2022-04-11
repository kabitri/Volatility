Nature of the time-series
==========================

An overview of the dynamic nature of the time-series is presented here. 
When looking for unusual values in the spot price data, it was found 
that there have been several spikes in the time series, with the most 
prominent one being in DK1 on June 7, 2013 - when spot prices sky
rocketed to 2000 €, which is almost 53 times higher than the average.
While this may apparently seem like a spurious value, a deeper 
investigation and literature review supported its legitimacy. It turns
out that on that day DK1 reached a technical maximum curtailment
price of 2000 €/MWh. This was due to the revisions on thermal power 
plants that coincided with grid maintenance and low wind power production
at the same time.  

In the following section, the nature of electricity price dynamics 
in the day-ahead market of NordPool is discussed. We will achieve
it through the following three sub-tasks:

#. Time-series decomposition
#. Auto-correlation effects
#. Effect of generation and consumption

Time-series decomposition
--------------------------

.. figure:: /images/ts_decomp.png
   :alt: Price time-series decomposition
   :align: left
   :scale: 100 %

Time-series of a real world entity usually contains cumulated information
from several factors. Decomposing a time-series help to clearly visualize
and understand the effect of individual component separately. In this 
context, the electricity price time-series is decomposed into three main
components - trend, seasonality and noise. Each component contain succesive 
insight into the true nature of the time series, and understanding them 
not only helps interprete results of overall analysis, but also aid in 
improved forecast accuracy. In this work, time-series decomposition is
applied using the STL function from the *statsmodels* library. It uses  
straight-forward method that decompose time-series using a low-pass filter
which makes it robust to the outliers - a feature that is often neglected
in the classical (seasonal_decompose function) approach to time-series 
decomposition. Additionally, STL can handle any type of seasonality and 
allows more control to the user in deciding the rate of change of the 
seasonal component. Due to all these attractive features, STL function 
has been chosen in this work. It is applied on the monthly data to get 
the features shown in the figure. 

.. code-block:: python
   :emphasize-lines: 1,2

   from statsmodels.tsa.seasonal import STL
   res = STL(data, period=period).fit()
   fig = res.plot()
   ax = fig.get_axes()
   ax[0].plot(res.observed)
   ax[1].plot(res.trend)
   ax[2].plot(res.seasonal)
   ax[3].plot(res.resid)

Auto-correlation effects
-------------------------

Now that we have seen that our electricity price time-series exhibit 
clear trend and seasonality components, it can be inferred that it
inherently carries a serial dependence with a lagged version of itself
- an analysis best studied with Auto Correlation Function (ACF) and 
Partial Auto Correlation Function (PACF). 

.. figure:: /images/acf_pacf_qq.png
   :alt: Correlation effects
   :align: center
   :figwidth: 100 %

The analysis relies on the assumption that the current value `S_{t}` 
is somehow dependent on its lagged values `S_{t-1}, S_{t-1}, S_{t-3}` 
and so on. This allows to build a linear regression model for our problem. 
The null hypothesis is formulated such that the time-series samples 
are independent and identically distributed random variables. The blue 
shaded region in ACF and PACF plots indicate the confidence interval 
used to test the null hypothesis. In this case, we have used the monthly 
data and saw that the values begin to showly fall with increasing lag 
values, with first six values remaining outside the blue area indicating 
that the time-series contain significant auto-correlation and the effect 
of seasonality (the ridge and crest of annual curves alter over a six 
months period) - further emphasizing the strong influence of variable
wind power in the generation mix as well as the gross consumption needs
on determining the price level. Unlike ACF, which explains how the 
present value `S_{t}` of a time-series is correlated with its past 
values (`S_{t-1}, S_{t-2}...S_{t-k}`) at lag `k`, PACF represents 
the autocorrelation between `S_{t}` and `S_{t-k}`, not accounted for 
by lags between 1 and (`k-1`). In our analysis, there is a single 
spike at the first lag in PACF followed by small random values, which
indicate an Auto Regressive (AR) process of order 1 (number of large 
spikes in PACF determines the order of AR process). The *statsmodel* 
library is used in this work to extract the values and confidence 
intervals of the auto-correlation function. 

.. code-block:: python
   :emphasize-lines: 1,2

   import statsmodels.api as sm
   data = input_data.resample('M').mean()

   sm.graphics.tsa.plot_acf(data, lags=nlag, zero=False)
   sm.graphics.tsa.plot_pacf(data, lags=nlag, zero=False)
   sm.qqplot(data)

Effect of generation and consumption
-------------------------------------

.. figure:: /images/windshare_price_corr.png
   :alt: Correlation between wind share and price
   :align: center
   :figwidth: 95 %

Electricity price in the day-ahead market is determined through the merit
order curve where the bids for each hour of the following day from buyers 
and sellers are arranged in order and the clearing (equilibrium) price is 
decided from the intersection of those two curves. Generation sources like 
wind and solar have near zero marginal costs, making them appear at the 
bottom of the merit order curve. Peak power plants, which operate only to 
cover peaks in demand, usually have the highest operating costs and often
appear at the top of the merit order curve. In between these two extremes,
are nuclear, coal and combined cycle gas plants. Now, with increasing share
of wind in the generation mix (when large volume of electricity demand is
covered by wind power), this merit order mechanism results in a relatively
low clearing price. However, if the wind production is low, specially in
situations when electricity demand remains high, the demand is covered with
more expensive generations, thereby increasing the clearing price. This 
calls for our attention in understanding the importance of electricity demand 
and the share of variable renewable sources like wind in the generation mix. 

.. figure:: /images/con_price_corr.png
   :alt: Correlation between electricity consumption and price
   :align: center
   :figwidth: 95 %

Electricity consumption in Denmark follows clear seasonal cycles with 
increased demand during cold winter months and reduced electricity needs
in summer. Electricity consumption also follows a diurnal pattern with 
two distinct peaks - the primary one being in the mid-morning when people
start to work and industries begin to operate heavy machineries and the
secondary peak appears in the evening when people go back home and turns 
of daily appliances like TV and dishwashers. This unique feature of 
electricity consumption with this two humped diurnal curve is reproduced
by the electricity price time series as well, indicating how strongly the
level of electricity consumption influence the price. 

.. figure:: /images/con_price_daily.png
   :alt: Diurnal pattern of consumption and price
   :align: center
   :figwidth: 75 %