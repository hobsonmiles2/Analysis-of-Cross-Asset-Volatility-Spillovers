# Cross-Asset Volatility Spillover Analysis

MSc Dissertation in Responsible Finance and Alternative Assets  
University College London

## Overview
Analysis of volatility spillovers across equities, bonds, commodities, and ESG assets (2005-2025) using:
- GARCH-DCC models
- Diebold-Yilmaz spillover indices
- Regime-switching analysis
- Machine learning forecasting (XGBoost, LightGBM)

## Notebooks
1. `Diss_DataCollect.ipynb` - Data download and preprocessing
2. `Diss_Garch_Garch_Fitting.ipynb` - Univariate GARCH estimation
3. `Regime_Identification.ipynb` - HMM and PELT regime detection
4. `Spillover_analysis.ipynb` - Diebold-Yilmaz spillover computation
5. `Diss_ML.ipynb` - Machine learning forecasting with SHAP

## Assets
- S&P 500, IEF (Treasuries), Gold, Crude Oil, Natural Gas, Wheat, PBW (Clean Energy)

## Key Findings
- Equities and ESG equities act as volatility transmitters
- Treasuries and gold serve as receivers/safe havens, but display weaker safe-haven tendencies within crises, often transmitting volatility.
- Spillovers intensify during crises (GFC, COVID-19, 2022 energy shock)
- ML models provide statistically significant but modest forecasting improvements

## Requirements
python>=3.8
pandas
numpy
yfinance
arch
statsmodels
sklearn
xgboost
lightgbm
shap
matplotlib
seaborn

##Author
Miles Hobson  
UCL MSc Responsible Finance and Alternative Assets
