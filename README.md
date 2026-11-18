# Can past prices forecast the price of Wrapped Bitcoin?

ARIMA and GARCH models at daily, weekly and monthly frequency, in R.

This repository contains my final project for the Quantitative Finance course of the Master in Mathematics, Economics and Statistics (MMES) at the African School of Economics.

**Read the report:** [PDF version](wbtc_arima_garch.pdf) or [page readable on GitHub](wbtc_arima_garch.md). The code is in [wbtc_arima_garch.Rmd](wbtc_arima_garch.Rmd).

## Question

If prices already reflect the available information, the price of an asset should behave like a random walk, and its past should be of little use to forecast its future. The project studies this for Wrapped Bitcoin (WBTC), a token backed one-to-one by Bitcoin, with five years of daily prices (31 January 2019 to 31 January 2024):

1. Which ARIMA model describes the closing price at the daily, weekly and monthly frequencies?
2. How do the properties of the returns change with the frequency?
3. Does the selected daily model forecast better than a random walk on data that were not used to estimate it?

## Methods

- Stationarity tests (ADF and KPSS) on the log price and on the returns, and a check for a seasonal pattern.
- For each frequency: autocorrelations of the returns, comparison of 16 ARIMA models by AIC and BIC, and a check with `auto.arima()`.
- Tests on the residuals: Ljung-Box, Jarque-Bera and a test for a changing variance.
- A GARCH(1,1) model for the volatility of the daily returns.
- A forecast evaluation on the last three months of the sample, compared with a random walk (RMSE, MAE and Diebold-Mariano test).

## Main results

- The log price is not stationary and its returns are stationary, at the three frequencies. I find no seasonal pattern.
- The selected models are ARIMA(1,1,0) for the daily price and the random walk, ARIMA(0,1,0), for the weekly and monthly prices.
- The daily returns have a small negative autocorrelation, very frequent extreme values (kurtosis of 22.5) and a variance that changes over time. These features fade at lower frequencies: the monthly returns are close to a normal white noise.
- The GARCH(1,1) model captures the changing variance of the daily returns. The volatility is persistent (alpha + beta = 0.94).
- On the last three months, the daily ARIMA model does not forecast significantly better than the random walk (RMSE of 1,006 USD against 1,016 USD).

## How to run

Install the required R packages:

```r
install.packages(c("xts", "forecast", "tseries", "fGarch", "rmarkdown"))
```

Open `wbtc_arima_garch.Rmd` in RStudio and click **Knit**. The file `wbtc_usd_daily.csv` must be in the same folder. The PDF version also requires a LaTeX installation, for example with `tinytex::install_tinytex()`.

## Files

| File | Content |
|---|---|
| `wbtc_arima_garch.pdf` | Report, PDF version |
| `wbtc_arima_garch.md` | Report, version readable on GitHub |
| `wbtc_arima_garch.Rmd` | R Markdown file containing all the code and the text of the report |
| `wbtc_arima_garch_files/` | Plots of the GitHub version |
| `wbtc_usd_daily.csv` | Daily prices of WBTC in US dollars (ticker `WBTC-USD`), downloaded from Yahoo Finance with the R package `quantmod` |

## Note

I submitted this project in February 2024 and reworked it in November 2026 before publishing it. The questions of the assignment are the same, but I corrected the way the series are differenced, the choice of the models and the reading of the residual tests. The volatility model and the forecast evaluation were not in the assignment: I added them in 2026.
