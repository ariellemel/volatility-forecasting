# Predicting Short-Term Stock Volatility

Forecasting the next 30 seconds of realised volatility from high-frequency order book data, comparing eight models in R.

**Result:** Ridge regression achieved the lowest forecast error (QLIKE 0.124), outperforming every time-series model.

## Approach
- **Data:** second-by-second order book snapshots for one stock, split into 10-minute intervals and 30-second buckets. Missing seconds are forward-filled to preserve real price jumps.
- **Features:** weighted average price, order book depth (top two levels) and bid-ask spread.
- **Models:** OLS, weighted least squares, ridge, lasso, EWMA, HAR-RV, GARCH(1,1) and EGARCH(1,1).
- **Evaluation:** rolling-window forecasts (train on the previous 8 minutes, predict the next 30 seconds), scored with QLIKE (Patton, 2011) and RMSE. The ridge penalty is tuned against QLIKE on the same rolling windows.

## Results

| Model | QLIKE | RMSE |
|---|---|---|
| Ridge | 0.1237 | 0.000499 |
| Lasso | 0.1244 | 0.000515 |
| EGARCH | 0.1457 | 0.000546 |
| OLS | 0.1651 | 0.000550 |
| EWMA | 0.1748 | 0.000567 |
| HAR-RV | 0.1984 | 0.000544 |
| WLS | 0.1991 | 0.000541 |
| GARCH | 0.2476 | 0.000524 |

## Running it
1. Install R packages: `install.packages(c("tidyverse", "rugarch", "glmnet"))`
2. Place the order book data at `data/stock_1.csv` (not included in this repository).
3. Render `volatility_forecasting.qmd` with Quarto, or run the chunks in RStudio.

## Author
Arielle Melamed, University of Sydney
