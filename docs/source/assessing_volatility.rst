Assessment of volatility
==========================

Over the last couple of decades, a range of volatility estimation methods 
have been proposed in literature, including realized volatility, bipower 
realized variation, wavelet realized volatility, Fourier realized volatility. 
In this work, the following quantities have been defined and analyzed in 
the context of estimating a detailed scenario of volatility analysis in 
the day-ahead wholesale electricity market:

#. log returns (r)
#. realized variance (RV)
#. realized volatility (:math:`\sigma`)
#. positive/upside realized semivariance (:math:`RS^{+}`)
#. negative/downside realized semivariance (:math:`RS^{-}`)

Log returns (r)
-----------------

In this study,  the focus have been exclusively on the logarithmic returns. 
It is defined as the logarithmic price change between two adjacent hours:

:math:`r_{d,h} = \ln\left(S_{d,h}\right) - \ln\left(S_{d,h-1}\right)`

where :math:`S_{d,h}` denotes electricity spot price on hour h of day d of 
transaction. The simple reason for using log returns instead of directly 
measuring ramps is that the very nature of logarithmic properties offers 
normalization.

:math:`r_{d,h} = \ln\left(\frac{S_{d,h}}{S_{d,h-1}}\right)`

It is also worth mentioning here that the price time-series has been 
upscaled to avoid any negative values within logarithmic operation.

.. code-block:: python
   :emphasize-lines: 1

   el[col] = el[col] + np.abs(np.min(el[col])) + const
   el[r] = np.log(el[col] / el[col].shift(1))

.. figure:: /images/r_annual_bx.png
   :alt: annual variation
   :align: left
   :scale: 50 %

When r is positive, price jump indicates an increase in price and when 
r is negative, the jump indicates a decrease in price. The inter-annual 
variation of r is calculated over the simulation period. It depicts that
the variation in r on an average showed a declining trend until 2015, 
and then it again started to grow. It should be noted here that 2015 
was also the year with lowest average price among all the years simulated.
 
.. figure:: /images/r_months_bx.png
   :alt: seasonal variation
   :align: right
   :scale: 50 %

The seasonal variability of r is also calculated for both DK1 and DK2. 
It illustrates that the variability in price jumps is highest between 
June to October. Higher variation in jump suggests higher volatility
in the time series, which can be of concern to market players if not 
forecasted accurately. A closer inspection suggests an the overall 
variation in r remains in between -0.03 and 0.03, after removing
the extreme outliers for both DK1 and DK2.

.. figure:: /images/r_weeks_bx.png
   :alt: seasonal variation
   :align: left
   :scale: 50 %

The weekly variability of r shows a slightly reduced variations during 
the weekends, specially for DK2. Althogh this effect is less pronounced 
compared to other effects, such as inter-annual variability shown above,
it still shows its effect on the time series. 

.. figure:: /images/r_distrib.png
   :alt: histogram of r
   :align: right
   :scale: 60 %

A simple frequency distribution of hourly r values shows an extremely 
high peak around 0, indicating a large share of instances, when the 
price change from one hour to the next have been relatively small, in
both directions. However, the long tails of the distributions also 
suggest a few instances where the prices shown a large spike, either
due to technical issues or due to the effect of integrating a large 
shares of fluctuating renewables in the generation mix. The apparent 
symmetry of the two tails once again reinstate the mean reversion 
property of the electricity price time-series.

Realized variance (RV)
------------------------

In the context of this work, another important term is the realized 
variance (RV), which is the assessment of variation in daily returns 
obtained by analyzing its historical returns within a defined time 
period. The realized variance for each day is calculated as the sum 
of squared intra-day logarithmic returns, i.e.,:

:math:`RV_d = \sum_{h=0}^{23}r_{d,h}^2`

.. figure:: /images/RV_distrib.png
   :alt: histogram of RV
   :align: center
   :scale: 80 %

.. code-block:: python
   :emphasize-lines: 1,3

   el['r2'] = el['r'].pow(2)
   RV = pd.DataFrame()
   RV['RV_vals'] = el['r2'].dropna().resample('D').sum()

The figures below show the inter-annual and seasonal variation of daily
realized variance. A frequency distribution of the same is also presented
above. It shows that both DK1 and DK2 have similar distributions, except
the curve of DK2 is slightly flatter in the log-distribution. 

.. figure:: /images/RV_vals.png
   :alt: annual variation
   :align: left
   :scale: 100 %

Realized volatility (:math:`\sigma`)
-------------------------------------

Volatility has a specific technical meaning in finance theory. It is the 
degree of variation of a trading price series over time, measured by the 
standard deviation of the daily returns. It is to be noted that volatility 
does not measure the direction of price changes, merely their dispersion. 

:math:`\sigma = \sqrt{\sum_{h=0}^{23}r_{d,h}^2}`

The figures below depict the seasonal variation of volatility in terms of 
kernel density estimation.  

.. figure:: /images/sigma_vals.png
   :alt: monthly volatility
   :align: center
   :scale: 90 %

Realized semivariance (RS)
---------------------------

Although RV and :math:`\sigma` are good estimators of volatility, they both
fail to take into account the directionality of price changes. Thus, we use
semivariances to estimate this form of volatility which distinguishes between
upside and downside variations. 

:math:`RS^+_d = \sum_{h=0}^{23} r_{d,h}^2.I\left(r_{d,h} > 0 \right)`

:math:`RS^-_d = \sum_{h=0}^{23} r_{d,h}^2.I\left(r_{d,h} < 0 \right)`

Here :math:`I` is an indicator function taking the value 1 if the argument is
true.  A frequency distribution of upside and downside semivariances illustrates 
that higher share of instances with downside (i.e.,  price decreasing) jumps 
tend to populate the lower end of the spectrum leaving a more peaky and skewed 
distribution.  

.. figure:: /images/RS_distrib.png
   :alt: RS distribution
   :align: center
   :scale: 90 %