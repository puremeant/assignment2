특성 만들기: https://feng-cityuhk.github.io/EquityCharacteristics/
저자 코드: https://github.com/xiubooth/ML_Codes/tree/master/Simu_Matlab
Tidy: https://www.tidy-finance.org/blog/gu-kelly-xiu-replication/

Project Overview:
This project replicates the tables and figures of Gu, Kelly, and Xiu (2020), using U.S. stock data from 2017–2025. 

The main goal is to:
Requirements
Install dependencies using: pip install -r requirements.txt
To follow Gu, Kelly, and Xiu (2020)
Table 1 "Monthly out-of-sample stock-level prediction performance"
Figure 4 "Variable importance by model"
Figure 9 "Cumulative return of machine learning portfolios"
– Set 1: using sample period 1971 to 2016– Set 2: using sample period 1971 to 2025
– Set 2: using sample period 1971 to 2025

How to Run:
Install dependencies: pip install -r requirements.txt
Run the main script: python main.py

< Data Sources and Sample Construction >
1. Stock return data are obtained from the CRSP monthly stock file, Stock Version 2 (CIZ), through WRDS:
- https://wrds-www.wharton.upenn.edu/pages/get-data/center-research-security-prices-crsp/annual-update/stock-version-2/monthly-stock-file/
- I use monthly U.S. common stock return data for NYSE-listed stocks. The selected CRSP variables are permno, ticker, mthcaldt, mthret, and primaryexch. Following the assignment requirement, I restrict the sample to NYSE stocks only by keeping observations with primaryexch = "N".
- The stock dataset is automatically downloaded from Google Drive when running the code.

2. Firm-level characteristics are obtained from the Gu, Kelly, and Xiu / Dacheng Xiu firm characteristics dataset from:
- https://dachxiu.chicagobooth.edu/download/datashare.zip
- The dataset contains DATE, permno, 94 lagged firm characteristics, and sic2, where sic2 is the first two digits of the SIC code. The sic2 variable is used to construct industry dummy variables.

3. Macroeconomic predictors are obtained from Amit Goyal’s updated Welch–Goyal predictor dataset from:
- https://sites.google.com/view/agoyal145/home?authuser=0
- https://docs.google.com/spreadsheets/d/1qwpl2R_DNujpU5YUkk8lacP1tTeMb9iJ/edit?usp=sharing&ouid=113571510202500088860&rtpof=true&sd=true
- I use the monthly sheet from PredictorData2025.xlsx. Following Welch and Goyal (2008) and Gu, Kelly, and Xiu (2020), I construct eight macroeconomic predictors: dividend-price ratio (dp), earnings-price ratio (ep), book-to-market ratio (bm), net equity expansion (ntis), Treasury-bill rate (tbl), term spread (tms), default spread (dfy), and stock variance (svar).
- The macroeconomic predictors are lagged by one month before merging with stock-level returns and characteristics.

---------------------------------------
< Model, Objective, Algorithm >
1. Model: linear prediction using many predictors
- Objective: MSE plus Elastic Net penalty, with Huber robustification
- Algorithm: regularized regression path / cross-validation to choose penalty
2. Model: average of many regression trees
- Objective: reduce prediction error through tree splits
- Algorithm: bootstrap aggregation and random predictor selection
3. Model: nonlinear neural network with 2 or 4 hidden layers
- Objective: minimize prediction loss, usually MSE or robust loss
- Algorithm: backpropagation and stochastic gradient descent
