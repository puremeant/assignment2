# Assignment 2: Empirical Asset Pricing via Machine Learning
This repository contains a reduced-sample replication project based on Gu, Kelly, and Xiu (2020), *Empirical Asset Pricing via Machine Learning*. The project uses firm characteristics, industry information, and macroeconomic predictors to forecast monthly stock excess returns.

The project is inspired by the original authors' code and related replication resources:

- Original authors' code: https://github.com/xiubooth/ML_Codes/tree/master/Simu_Matlab
- Tidy Finance replication guide: https://www.tidy-finance.org/blog/gu-kelly-xiu-replication/

## Overview
The project evaluates several machine-learning models for stock return prediction over a 2017--2021 out-of-sample period. The main goals are to compare statistical prediction accuracy, examine variable importance, and evaluate prediction-sorted portfolio performance.

Main outputs:
- Table 1: out-of-sample stock-level predictive \(R^2\)
- Figure 4: variable-importance comparison
- Figure 9: cumulative returns of long and short prediction portfolios

## Data Sources and Sample Construction
Stock return data are obtained from the CRSP monthly stock file, Stock Version 2 (CIZ), through WRDS:
https://wrds-www.wharton.upenn.edu/pages/get-data/center-research-security-prices-crsp/annual-update/stock-version-2/monthly-stock-file/
I use monthly U.S. common stock return data for NYSE-listed stocks. The selected CRSP variables are `permno`, `ticker`, `mthcaldt`, `mthret`, and `primaryexch`. Following the assignment requirement, I restrict the sample to NYSE stocks only by keeping observations with `primaryexch = "N"`. The stock dataset is automatically downloaded from Google Drive when running the code.

Firm-level characteristics are obtained from the Gu, Kelly, and Xiu / Dacheng Xiu firm characteristics dataset:
https://dachxiu.chicagobooth.edu/download/datashare.zip
The dataset contains `DATE`, `permno`, 94 lagged firm characteristics, and `sic2`, where `sic2` is the first two digits of the SIC code. The `sic2` variable is used to construct industry dummy variables.

Macroeconomic predictors are obtained from Amit Goyal's updated Welch--Goyal predictor dataset:
https://sites.google.com/view/agoyal145/home?authuser=0
https://docs.google.com/spreadsheets/d/1qwpl2R_DNujpU5YUkk8lacP1tTeMb9iJ/edit?usp=sharing&ouid=113571510202500088860&rtpof=true&sd=true
I use the monthly sheet from `PredictorData2025.xlsx`. Following Welch and Goyal (2008) and Gu, Kelly, and Xiu (2020), I construct eight macroeconomic predictors: dividend-price ratio (`dp`), earnings-price ratio (`ep`), book-to-market ratio (`bm`), net equity expansion (`ntis`), Treasury-bill rate (`tbl`), term spread (`tms`), default spread (`dfy`), and stock variance (`svar`). The macroeconomic predictors are lagged by one month before merging with stock-level returns and characteristics.

The main modeling dataset is: gkx_model_data_1971_2021.parquet

## Models
The project estimates the following models:
* OLS+H
* OLS-3+H
* PCR
* ENet+H
* Random Forest
* NN2
* NN4
Here, `+H` indicates Huber loss. The NN2 and NN4 results use 10-network ensemble predictions.

## Evaluation
Table 1 reports stock-level OOS (R^2), which measures return-level forecast accuracy.
Figure 9 evaluates portfolio performance. Each month, stocks are sorted by predicted excess returns. The long portfolio buys the top predicted-return decile, while the short portfolio shorts the bottom predicted-return decile. Monthly returns are compounded over time.
Table 1 and Figure 9 measure different aspects of performance: Table 1 evaluates forecast accuracy, while Figure 9 evaluates cross-sectional ranking ability.

## Running the Project

Install required packages:
pip install numpy pandas scikit-learn matplotlib pyarrow openpyxl torch

Run the main scripts:
python assignment2_build_data.py
python assignment2_table1_fig4_reduced.py
python run_nn2_nn4_ensemble10.py
python assignment2_figure9_reduced.py

Make sure the neural-network script uses the full dataset, not the sample file:
DATA_PATH = r"C:\Users\Assignment2\output\gkx_model_data_1971_2021.parquet"

## Notes
This is not a full-scale replication of Gu, Kelly, and Xiu (2020). It is a reduced-sample assignment project using a shorter 2017--2021 OOS period. The results should therefore be interpreted as a compact replication exercise rather than a direct reproduction of the original paper.

## References
* Gu, S., Kelly, B., and Xiu, D. (2020). Empirical Asset Pricing via Machine Learning. *The Review of Financial Studies*, 33(5), 2223--2273.
* Welch, I., and Goyal, A. (2008). A Comprehensive Look at the Empirical Performance of Equity Premium Prediction. *The Review of Financial Studies*, 21(4), 1455--1508.
