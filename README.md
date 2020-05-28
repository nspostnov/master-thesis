This repository consists of all supplementary materials for understanding the 
realization of algorithms that are used in the master's thesis.

All Jupyter Notebook files consist of the description and comments of the code in 
Russian but this README file makes the same things in English. 

The structure of the materials is following:
1. database\_creation.ipynb
2. database\_description.ipynb
3. liquidity\_modeling.ipynb
4. liquidity\_modeling\_results.ipynb
5. chains\_identification.ipynb
6. liquidation\_problem.ipynb (the main script)
7. add\_figures\_tables.ipynb

# 1. Database Creation (database\_creation.ipynb)
This Jupyter Notebook file is created for the database creation and filling in the 
localhost. You must have initial datafiles from MOEX Exchange in the .txt format
like OrderLog20190603.txt. You can contact with MOEX Exchange to get this data or 
contact with Higher School of Economics to get the same data from Archive that 
covers the time interval from 2015 to 2016. 

The global parameters that you can change are 
1. DB\_NAME - the name of database of PostgreSQL that you want to create
2. PATH - the path to the directory that consists of initial data files in the 
format .txt (it was described earlier). 
3. SEPARATOR - the separator of the fields in these .txt files (by default it is ',').

Attention: you should make sure that you have enough disk storage to fulfill the 
database (inaccurate estimation of this is the sum of all file weights). 

The description for table fields is following:
1. NO - number of record (unique, bigint)
2. SECCODE - the ticker of the instrument (string up to 20 symbols)
3. BUYSELL - identificator to buy or sell (char string with 'B' of 'S')
4. TIME - the time of record in the int format (bigint)
5. ORDERNO - the number of order (bigint)
6. ACTION - the type of order (1 for setting, 0 for removing, 2 for execution)
7. PRICE - the price of order (float)
8. VOLUME - the volume of order (bigint)
9. TRADENO - the number of trade if it occurs (bigint, can be NULL)
10. TRADEPRICE - the price of trade if it occurs (real, can be NULL)

The primary key of table is the number of record ("NO").  

# 2. Database Description (database\_description.ipynb)
This file consists of the code to count the volumes \& the activity of the market 
according to each ticker. Also the selection of tickers is provided based on the 
sum of volume index.  

# 3. Liquidity Modeling (liquidity\_modeling.ipynb)
This file consists of materials to build the limit order book and the empirical 
function of transaction costs according to the Perold's Implementation Shortfall. 
After that for all tickers from the previous selection the estimation of tightness
and depth of the market is provided. The model separates this estimation to the 
2 cases: to buy and to sell. The model to estimate is following:

<img src="https://render.githubusercontent.com/render/math?math=\Theta(v_t)=\alpha_t v_t^{\beta_t}">

# 4. Liquidity Modeling Results (liquidity\_modeling\_results.ipynb)
This file makes the container of all results that are obtained in the previous file
and plots the dynamics of the tightness and depth estimation of each ticker.

# 5. Chains Identification (chains\_identification.ipynb)
This file implements the logic for identification the chains to estimate the volume 
of High Frequency Trading in the data according to the ideas of Hasbrouck J., Saar G. 
Low-Latency Trading // Journal of Financial Markets. — 2013. — Vol. 16. — P. 646–679.

The algorithm can be described in the following way. We create the indicator of the 
order that is classified or not. It means that we must know whether we already 
classified the order to some high frequency trading chain or not 
(identified\_orderno). Also, we create the chain\_number identificator to combine 
some orders into one chain. 

The algorithm is the same as some recursive methods of search. Thus, the steps are
following:
1. If the order is classified then go next. 
2. If the order is not classified yet then start new chain (if the last chain at 
this quote VOLUME is finished) or try to continue the last chain (if the time 
criteria is ok: time gap between orders must be not greater than 1 second). 
3. In the cases when we can identify the order to different chains we select the first
one (by time critera).  

# 6. Liquidation Problem (liquidation\_problem.ipynb)
This file implements the logic for 
1. Transaction Costs function parametrization
2. Estimation the volatilities of the asset's prices
3. The logic for Basin-Hopping \& Trust Regions Methods Implementation
4. The comparison of the results that is described the end of the third chapter

# 7. Build and Plot Beautiful Figures (add\_figures\_tables.ipynb) 
This script has some logic from the previous code files that is need for 
building the beautiful plots and graphs for the final report that is compatible with 
the Russian GOST. 
