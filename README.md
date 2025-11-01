# Demand Forecasting — Experiments Summary

This README summarises the insights, analysis steps, and key findings from `experiments.ipynb`. The notebook performs time-series analysis and forecasting on the provided demand dataset, along with exploratory preprocessing and plotting. The goal is to build a robust ARIMA-based forecasting pipeline and document the reasoning behind model choices.

---

## Files in the repository / provided
- `experiments.ipynb` — Jupyter notebook containing data loading, EDA, preprocessing, model selection, fitting, and forecasting steps.
- `demand_inventory.csv` — Raw dataset used for analysis (uploaded alongside the notebook).


---

## Problem statement
Given a historical time series of demand, build a forecasting model to predict short-term future demand. The notebook explores stationarity, identifies ARIMA hyperparameters using ACF/PACF, fits candidate ARIMA models, and produces forecasts that can be used for planning inventory and operations.

---

## Data & preprocessing
- The dataset used in the notebook contains daily demand records (a `Date` column and a `Demand` value column). The notebook converts the `Date` column to `datetime` and sets it as the index for time-series processing.
- Missing values were inspected and handled (see notebook). Where appropriate, rows with missing target values were filtered out before modeling.
- A first difference (period = 1) of the series was computed and visually inspected for stationarity. The notebook notes that *after first differencing the pattern becomes stationary*, so `d = 1` is selected for ARIMA modeling.

---

## Exploratory analysis (high-level)
- Time-series plots: original series plotted to inspect trend and seasonality. Visual inspection suggested non-stationarity that was removed after first differencing.
- ACF and PACF plots of the differenced series were examined to identify AR (`p`) and MA (`q`) orders:
  - The notebook states: "AT lag=1,2 the value is out of the range marked in the chart. Hence p = 1, 2".
  - The notebook also states: "AT lag=1 the value is out of the range marked in the chart. Hence q = 1".
  - Taken together, candidate ARIMA orders explored include combinations with `d = 1`, `p` in `{1, 2}`, and `q = 1` (i.e., (1,1,1) and (2,1,1) among others).

> Note: ACF and PACF interpretation rules were used visually in the notebook to select candidate (p, d, q) — see the corresponding ACF/PACF plots in the notebook for exact lags and confidence intervals.

---

## Modeling approach
- The notebook fits ARIMA-style models (using `statsmodels` or similar) on the differenced/time-indexed series with candidate hyperparameters identified above.
- Forecasts were produced for a horizon of interest and presented as a DataFrame (the notebook contains the forecasted values and associated series shown in printed tables). Plots of fitted vs. actual and forecasted values help visually assess model fit.

---

## Results (summary)
- The notebook concludes that first-differencing achieved stationarity, and candidate models were evaluated based on visual fit and the modeling procedures in the notebook.
- Forecasted demand values (sample rows) are produced in the notebook; for example, some predicted demand values in the notebook's output are around ~115–130 for a subsequent date range (see the notebook's forecast table for exact dates/values).
- The notebook contains plotted diagnostics (residual plots, ACF of residuals, etc.) to check for remaining autocorrelation and to validate model assumptions.
