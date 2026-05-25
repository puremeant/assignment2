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

Data:
Stock data: CRSP monthly returns (2017–2025), NYSE stocks only
From https://wrds-www.wharton.upenn.edu/pages/get-data/center-research-security-prices-crsp/annual-update/stock-version-2/monthly-stock-file/
Choose 'permno, ticker, mthcaldt, and mthret, primaryexch' as variables after reading variable description in Stock - Version 2 (CIZ)
And extract data where 'primaryexch = N' (NYSE ONLY)
Factor data: Fama-French 5 factors + Momentum (from Ken French Data Library)
The stock dataset is automatically downloaded from Google Drive when running the code.
