Heterogeneous AutoRegressive Model of Realized Variance (HAR-RV)
=================================================================

In this work, volatility forecast is accomplished by implementing 
a HAR-RV model. This model has gained wide popularity over the years
for consistently producing good quality forecasts in spite of its 
simplicity. HAR-RV models are known to successfully reproduce the 
major features of financial returns and is considered to be one of 
the most powerful benchmark models for volatility forecasting. The 
formulation of standard HAR-RV model is based on a straightforward 
extension of the ARCH (or GARCH) class of models. The HAR-RV model 
incorporates an additive feature of volatility components defined 
over specific temporal horizons since predictability can be achieved 
by tracing average levels of volatility over those lag periods.

Implementing HAR-RV model
--------------------------

The standard HAR-RV model uses fixed lag indices to mirror different 
components of volatility on daily, weekly and monthly scales. As such, 
in the HAR-RV model, the conditional variance of discretely sampled 
returns are parameterized as a linear function of daily, weekly and 
monthly volatility and can be expressed as:

:math:`RV_{d,t+1} = \beta_0 + \beta_1 RV_{d,t} + \beta_2 RV_{w,t} + \beta_3 RV_{m,t} + \epsilon_0`

where, :math:`RV_{d,t}`, :math:`RV_{w,t}` and :math:`RV_{m,t}` are 
the daily, weekly and monthly RVs respectively, and :math:`\beta` s 
are the coefficients and :math:`\epsilon_0` is the error term. 
Mathematically this is a multi-variate linear regression model and 
can be solved directly with ordinary least square (OLS) method. 
Solving this equation yields the predicted RV for the next day. In 
this work, the model is trained with data from 2006-2019 and fitted 
data is used to predict daily volatility of the last year, 2020. The 
model quality is evaluated using three different statistical measures: 
mean square error (MSE), r-squared (RSE) and mean absolute error (MAE). 

Results of HAR-RV model
------------------------

.. figure:: /images/result.png
   :alt: HAR-RV results
   :align: center
   :scale: 90 %

After fitting the model onto the training dataset, it is used to predict 
the RV of the following day. Then model predicted results are compared with 
the actual data and the results are shown in the figure above. It shows 
fairly good agreement between the actual and the predicted values in terms 
of reproducing the dynamics/patterns, however the discrepancy in magnitudes 
remains a concern. The predicted values are significantly higher in magnitude 
than the actual values, since the test year 2020 was in fact an year of 
particularly high values of RV, preceded only by 2008, for both DK1 and DK2.
Additionally, the training data contained multiple instances of significantly 
large outliers and OLS is known to be quite sensitive to this.  While further 
improvement is surely possible, one must be careful not to overfit the model, 
as this can reduce model performance when applied to a new (previously unknown 
to the model) set of data. The figure above shows that in spite of the mismatch 
in absolute values, the predicted values capture the essence of the time-series 
very well. However, the discrepancy between the two emerges following any large 
spike in the data set. The table below provides quantitative measures on the 
degree of agreement between the actual and predicted values through three 
different types of correlation coefficients and three different measures of 
error.  All three correlation coefficients are quite high, specially for DK1,  
indicating satisfactory performance by the model.  The remaining discrepancies 
are planned to be addressed separately in a later study.

.. list-table:: Result of Linear Regression
   :widths: 5 5 10 20 20 20 20
   :header-rows: 1

   * - area
     - :math:`R^2`
     - :math:`Adj-R^2`
     - :math:`\beta_0`
     - :math:`\beta_1`
     - :math:`\beta_2`
     - :math:`\beta_3`
   * - DK1
     - 0.012
     - 0.011
     - 0.0157 [0.004]
     - 0.1082 [0.015]
     - -0.0020 [0.041]
     - 0.0245 [0.078]
   * - DK2
     - 0.019
     - 0.018
     - 0.0179 [0.004]
     - 0.1327 [0.015]
     - -0.0023 [0.041]
     - 0.0836 [0.075]