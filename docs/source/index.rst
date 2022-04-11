.. RMVA documentation master file, created by
   sphinx-quickstart on Sat Mar 26 14:17:16 2022.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to RMVA's documentation!
================================

**Risk Management via Volatility Assessment (RMVA)** is a Python library for a 
comprehensive analysis of electricity price volatility in the day-ahead
market. For this project, model domain is chosen to be the Danish power
system, with two synchronous bidding zones of west and east Denmark, 
which are part of the NordPool electricity trading arena. 

Electricity generation, consumption and price data are provided by Energinet,
which is the Danish Transmission System Operator (TSO). All data downloaded
from the following link <https://www.energidataservice.dk>.

.. note::
   This project is under sctive development.

.. toctree::
   :maxdepth: 2
   :caption: Contents:

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

Contents
---------

.. toctree::
   data
   preprocessing
   ts_nature
   comp_area
   low_high_freq
   assessing_volatility
   har_rv