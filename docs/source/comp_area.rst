Comparing price areas
=======================

Denmark consists of two synchronous areas, Western Denmark (DK1) and Eastern 
Denmark (DK2), which are connected through the Great Belt Power Link. They 
are well connected with the neighboring countries, like Germany, Sweden and
Norway (only connected to DK1, not DK2), which are all members of the NordPool
power market. Due to this strong connectivity and large market integration, 
higher level of risk sharing (in terms of ramps and spikes) between several 
interconnected bidding zones is possible, thereby reducing the probability 
of energy crises and energy shortages occurring in any given market. In this
study we have used *correlation* measure, which is a bivariate analysis, to 
estimate the strength of association between the price time-series from the
two bidding zones considered here. A correlation coefficient varies between 
+1 and -1, with values closer to these two extremes indicate perfect degree 
of association between the two variables while values closer to zero indicate
a weak relation between them. 

.. figure:: /images/price_corr_coeff.png
   :alt: Correlation between price areas
   :align: left
   :scale: 50 %

The dynamics of electricity price in DK1 closely follows that of DK2. In fact, 
a comparison between these two time-series present itself with a Pearson 
correlation coefficient of 0.58, however removing a few outliers by restricting
values between [-200, 500], leads to a significantly improved correlation 
coefficient of 0.84. This restriction causes a loss of only 0.02% data points,
but significantly improves the correlation. The image here shows that there 
are a few very large values in both time-series, not necessarily occuring at 
the time, indicating effects from different sources that can influence the 
market price. Technical issues like development of bottleneck conditions or 
maintenance periods of large central power plants coinciding with low wind 
generation periods, can also contribute to such factors. 

In this work, three different forms of correlation coefficients have been used.

#. Pearson's correlation coefficient r 
#. Spearman's :math:`\rho` 
#. Kendall's :math:`\tau` coefficient

.. figure:: /images/price_pearson_kendall.png
   :alt: Inter-annual variation of correlation coefficient
   :align: left
   :scale: 50 %

Pearson's correlation coefficient is the most popular measure of correlation 
in data science. Pearson's coefficient measures linear correlation, while the 
Spearman and Kendall coefficients compare the ranks of data. The Spearman's 
:math:`\rho` is very similar to Kendall's :math:`\tau`, however the latter is
more robust and generally more preferred than the former. The correlation 
between DK1 and DK2 for each simulation year is shown in the figure here in 
terms of Pearson and Kendall's measures. It shows that almost all years has 
coefficients above 0.6, except 2009, 2010 and 2013. A closer look into the 
time-series of these three years reveals presence of large spikes in the 
data of one bidding zone and not on the other. The original data (prior to
applying any filter) also shows a strong linear relationship between DK1 and 
DK2 price time-series [€] with the following results:

.. list-table:: Result of Linear Regression
   :widths: 75 25
   :header-rows: 1

   * - Measures
     - Values
   * - slope
     - 0.65
   * - intercept
     - 15.31
   * - r value
     - 0.57