Preprocessing
=============

Before proceeding to the actual analysis, it is always a good idea 
to apply some preprocessing to the data, not only to familiarize 
with the content, but also to change file formatings if necessary, 
to look for missing and/or bad values in the dataset, to create 
simple visuals to get the first impression of a variable's nature.

Time issues
-------------
There are two time columns in all files downloaded from the Energinet 
dataset. One is the UTC time (HourUTC) and the other one is the local 
Danish time (HourDK). While changes due to day-light saving are reflected 
in the latter, the former is expected to have no such caveat unless 
any missing row is encountered. So, in order to find missing/repeating 
values, both local and UTC timings have been used in the preprocessing 
section. The rest of the analysis is performed with the UTC timing only. 

Lack of uniformity
-------------------
*This is an issue with the spot price files. For the production and 
consumption files, Energinet has already resolved the lack of uniformity 
among all years*. In an attempt to find any missing time stamps in the 
price data, it was found that for years 2006-2010, a single row has been 
missing for both price areas of Denmark. Upon closer investigation, it 
was identified as the problem with representing the day-light saving 
hour starting and ending. By the end of March, the clock jumps one hour 
forward, by adjusting the clock from local time 01:00 HourDK to 03:00 
HourDK. At the end of the day-light savings in October, after passing of 
local time 02:00 HourDK, the hour is set back an hour, thereby re-entering 
the time index 02:00 HourDK. Since Energinet follows the norm to represent 
both physical hours by 02:00 in local time, it is important to look into 
the UTC timings, which remain unaffected by day-light saving issues. After 
checking with the UTC timings it was found that for the first five years 
of simulation (2006-2010) the *first* 02:00 local hour (before resetting 
the clock) on the end of the day-light saving times is apparently missing. 
For the rest of the years, somehow, exactly same values have been provided 
for both 02:00 hours, before and after resetting the clock. Since these 
two are two separate physical time periods, it is extremely unlikely to 
have same values for each variable every year. The most likely explanation 
is Energinet sorted the data based on unique time stamps in the local 
hours, and the values given are either mean of the two hours or one 
overwritten on another. In either way, Energinet has not provided any 
useful insight in this matter yet, but the reader is encouraged to follow 
the upcoming reports and data discussion sections of Energinet's database 
for any further clarification. For now, what we are interested is to make 
the formats identical for all simulation years. Since majority of the years 
follow the pattern of repeating values for both 02:00 hours (HourDK), this 
is also adopted for the initial five years of simulation. 

Issues with the original files from Energinet
---------------------------------------------

#. Downloads are limited to the top 100000 rows of the data. So, each 
data file downloaded covers only a single year. For the ease of handling, 
these annual data files have been combined together in the preprocessing 
section to create one single data file for all price areas.

#. In the hydro power data, Energinet comments *'In older data included 
in “Local Power”'* which severely limits its usage due to lack of 
clarification on the way hydro power information is provided in the data 
over the years. 
  
#. In Energinet's database, commercial power is separated from local 
power in 2020. So, from 2020 onwards, local power production is updated 
as a combination of both local and commercial power productions. Since 
commercial power is otherwise missing for all earlier years, our modified 
data file contains the updated local power production and doesnot keep any 
separate column for commercial power production.

#. In an attempt to explain if the local power generation data provided 
by Energinet represents gross or net generations (before and after 
removing the effect of self consumption, respectively), it was found that 
for several instances, the *LocalPowerSelfConMWh* exceeds the combined 
generation from *LocalPowerMWh* and *CommercialPowerMWh* units. The most 
likely explanation is that the power production data provided by Energinet 
is the measured net delivery to the grid, i.e., the production minus the 
self-consumption. Since, we are interested only in the part of power that 
is actually delivered to the grid, we simply combine the *LocalPowerMWh* 
and *CommercialPowerMWh* to obtain the *LocalPowerProdMWh*. For all years 
before 2020, commercial power was not separated from the local power. In 
addition, LocalPowerSelfConMWh was set to zero (to be interpreted as null) 
for all time stamps in these years. So in line with the 2020 scenario 
mentioned above, it is assumed that the *LocalPowerMWh* provided by 
Energinet actually correspond to the ‘net’ local power generation.


#. From 2020, solar power is divided into three columns, depending on 
the panel's capacity. Until for the year 2020, all solar power is placed 
in *SolarPowerSelfConMWh*. Since this detailed description of solar power 
is missing for most of the simulation period we chose, we are restricting 
ourselves to the overall solar power production only. For all years before 
2020, the data from *SolarPowerSelfConMWh* is used for solar power production 
(the nomenclature used here by Energinet might be confusing to some users, 
but the description must clear it out) and from 2020 onwards, power generation 
from different capacities is combined to give an overall value of solar power 
generation.  
  
#. It must be noted that the solar power generation calculated by combining 
productions from different capacities is the net solar power generation. 
This means, this is the remaining power added to the grid that is left 
after self-consumption. Although this was not clarified by Energinet, it 
was personally calculated to check if any negative solar power generation 
value arises, when the production was wrongly assumed as gross power 
generation (i.e., before removing the selfconsumption part). The reader 
is encouraged to do his/her own analysis and to keep an eye on Energinet's 
future updates for any insight in this matter.  

File structure in the proprocessing section
-------------------------------------------

The preprocessing section has two main files: 

#. preprocessing.py
#. unifying_files.py

As the names suggest, the first file does several preprocessing tasks 
including checking for any missing values, specially due to day-light 
saving issues for both generation-consumption and electricity spot price 
files while the second file combines the generation-consumption and price 
files, creates a single file withall relevant information, checks and 
updates for 'bad' values like nan or negative values (wherever rendered 
unrealistic/inappropriate), removes any variable redundant for this project, 
and creates a set of quick visuals for easy overview. These two main program 
files must run in order as given above. Both these files use a set of 
user-defined functions compiled in a single file, called the pp_func.py 
(the preprocessing library file). 