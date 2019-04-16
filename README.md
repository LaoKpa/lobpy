# lobpy


This package provides tools for a tractable class of models for the limit order book dynamics. A detailed description of the models, the derivation and the financial and mathematical background is given in the manuscript: 

[Cont, Mueller (2019): A stochastic partial differential equation model for limit order book dynamics](https://arxiv.org/abs/1904.03058 )


## Code Example



In general, lobpy can be run as

> lobpy -cp/-cd args


### Calibration to Lobster data

**Requires:**
order book file, name format: TICKER_DATE_STARTTIMEINMS_ENDTIMEINMS_oderbook_NUMBEROFLEVELS.csv
message file, name format: TICKER_DATE_STARTTIMEINMS_ENDTIMEINMS_message_NUMBEROFLEVELS.csv

Please see lobsterdata.com for further details of file formatation

The calibration of the model dynamics is started as follows:

> lobpy -cd ticker_str date_str time_start_data time_end_data time_start_calc time_end_calc num_levels_data num_levels_calc timegrid_size timesteps_recal [calibration_to_average_flag(0/1)]

The fit of the average profile: 

> lobpy -cp ticker_str date_str time_start_data time_end_data time_start_calc time_end_calc num_levels_data num_levels_calc num_intervals


Test:
> lobpy -t


E. g. 

> python3 -m lobpy -cd "TICKER" "01-06-66" 34200000 36000000 34200000 36000000 10 10 10000 1000 1 1



## Example

We suppose we have the files

TICKER_yyyy-mm-dd_34200000_57600000_oderbook_50.csv

and

TICKER_yyyy-mm-dd_34200000_57600000_message_50.csv

in the specified format.

We want to calibrate the model by estimating the stationary profile from first 20 levels of the 30min-average profiles, and estimate the remaining model parameters from the processes of order volume in first 20 levels, separately on bid and ask side. 

Each 30min we calibrate to past 30min data. We skip the first and last 30min of the trading day. 
For the order volume processes we use a time grid of mesh size 10ms. That means the time grid has a size of 1980001 time points and for each calibration of the dynamics we have 180001 time points.  

The calibration then works in 2 steps:

1) Parameter estimation from expected profile: 
> lobpy -cp "TICKER" "yyyy-mm-dd" 34200000 57600000 36000000 55800000 50 20 1800000

2) Parameter estimation from dynamics of the bid and ask order volume processes:

> lobpy -cd "TICKER" "yyyy-mm-dd" 34200000 57600000 36000000 55800000 50 20 1980001 180001 180000 0

Recall that the last argument (0) specifies that we calibrate to the processes of total order book volume in the first 20 levels, and not of the average of the volume in the first 20 levels. 


## Motivation

The key motivation is to improve understanding of the dynamics and price formation of modern electronic financial markets. 


## Installation

Download the folder lobpy and place it where python can find it. 

## API Reference

See autogenerated documentation in [doc](doc) or the online version on an external homepage [here](https://people.math.ethz.ch/~marvmuel/lobpydoc/doc/lobpy.html)


## Authors

* **Rama Cont** 
* **Marvin S. Mueller** 

For the theoretical framework as well as results from the running code see [Cont,Mueller (2018)](in preparation)


## License

BSD 3-Clause License

Copyright (c) 2018, University of Oxford, Rama Cont and ETH Zurich, Marvin S. Mueller
All rights reserved.

See the [LICENSE](LICENSE) file for details.

