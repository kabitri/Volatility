About the data
================
In this project, variability of electricity time series data (including electricity prices in the day-ahead market, electricity generation and consumption) has been analyzed to understand how the interplay of several of these components may influence the day-ahead market price of electricity. Given this objective, our analysis so far is restricted to data resolution on the hourly scale and above. To keep the analysis as much general as possible, technical details have been omitted to avoid any unwanted case sensitivity. 

Data source
-----------
All data provided by Danish Transmission System Operator, Energinet. Data downloaded from <https://www.energidataservice.dk>. Latest data downloaded on 09 September, 2021. 

Data type
---------
This project deploys analysis of the following data sets.

#. electricity prices in the day-ahead market
#. gross electricity consumption
#. electricity generation from several sources, like central and local power plants, solar, onshore and offshore winds, hydro etc. 

Electricity price data is the core component of the analysis while consumption and generation data are used to explain any influence they may exert on the price dynamics.
 
All data downloaded from Energinet is in .csv format.

Period covered
--------------
The simulation period covers a 15 year old duration from 2006-2020. All data is in hourly resolution and upscaled in certain cases as per requirements.

Quick overview
--------------

.. figure:: /images/price_overview.png
   :alt: Overview of price dynamics
   :align: left
   :scale: 50 %

   An overview of electricity prices over the simulation period.