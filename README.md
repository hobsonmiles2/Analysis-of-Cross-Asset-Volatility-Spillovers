# Cross-Asset Volatility Spillover Analysis

![Python](https://img.shields.io/badge/python-3.8+-blue.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)
![Status](https://img.shields.io/badge/status-complete-brightgreen.svg)

MSc Dissertation in Responsible Finance and Alternative Assets
University College London

## Overview
Analysis of volatility spillovers across equities, bonds, commodities, and ESG assets (2005-2025) using:
- GARCH-DCC models
- Diebold-Yilmaz spillover indices
- Regime-switching analysis
- Machine learning forecasting (XGBoost, LightGBM)

## Notebooks
1. [01_DataCollect.ipynb](Notebooks/01_DataCollect.ipynb) - Data download and preprocessing
2. [02_GARCH_DCC_GARCH.ipynb](Notebooks/02_GARCH_DCC_GARCH.ipynb) - Univariate GARCH and DCC estimation
3. [03_Diss_ML.ipynb](Notebooks/03_Diss_ML.ipynb) - Machine learning forecasting with SHAP
4. [04_Regime_Identification.ipynb](Notebooks/04_Regime_Identification.ipynb) - HMM and PELT regime detection
5. [05_Spillover_Analysis.ipynb](Notebooks/05_Spillover_Analysis.ipynb) - Diebold-Yilmaz spillover computation

## Assets Analyzed
- **Equities**: S&P 500 (^GSPC)
- **Fixed Income**: iShares 7-10 Year Treasury Bond ETF (IEF)
- **Commodities**: Gold (GC=F), Crude Oil (CL=F), Natural Gas (NG=F), Wheat (ZW=F)
- **ESG/Clean Energy**: Invesco WilderHill Clean Energy ETF (PBW)

**Sample Period**: 2005-2025 (daily data)

## Key Findings
- Equities and ESG equities act as volatility transmitters
- Treasuries and gold serve as receivers/safe havens, but display weaker safe-haven tendencies within crises, often transmitting volatility.
- Spillovers intensify during crises (GFC, COVID-19, 2022 energy shock)
- ML models provide statistically significant but modest forecasting improvements

## Quick Start

### Prerequisites
- Python 3.8 or higher
- Jupyter Notebook or JupyterLab
- ~500 MB free disk space for data and outputs

### Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/yourusername/Analysis-of-Cross-Asset-Volatility-Spillovers.git
   cd Analysis-of-Cross-Asset-Volatility-Spillovers
   ```

2. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

3. **Run notebooks in sequence:**

   Navigate to the Notebooks directory and run in order:

   ```bash
   cd Notebooks
   jupyter notebook
   ```

   **Execution Order:**
   1. **01_DataCollect.ipynb** - Downloads asset price data from Yahoo Finance (2005-2025)
   2. **02_GARCH_DCC_GARCH.ipynb** - Estimates GARCH models and dynamic conditional correlations
   3. **03_Spillover_Analysis.ipynb** - Computes Diebold-Yilmaz spillover indices
   4. **04_Regime_Identification.ipynb** - Identifies volatility regimes using HMM and PELT
   5. **05_Diss_ML.ipynb** - Machine learning forecasting with XGBoost/LightGBM and SHAP analysis

   **Note:** Each notebook takes 5-30 minutes to run depending on your system.

### Troubleshooting

**Path Errors:**
- Ensure you're running notebooks from the `Notebooks/` directory
- All paths are relative to the Notebooks folder

**Memory Errors:**
- Reduce `MAX_ROWS_PER_SEG` in notebook 05 (default: 5000 → try 2000)
- Close other applications to free RAM

**Missing Data:**
- Run notebook 01 first to download data
- Check internet connection for Yahoo Finance access

**Package Conflicts:**
- Use a virtual environment: `python -m venv venv`
- Activate: `source venv/bin/activate` (Linux/Mac) or `venv\Scripts\activate` (Windows)

## Project Structure
```
.
├── Notebooks/              # Analysis notebooks (run in numerical order)
│   ├── 01_DataCollect.ipynb
│   ├── 02_GARCH_DCC_GARCH.ipynb
│   ├── 03_Diss_ML.ipynb
│   ├── 04_Regime_Identification.ipynb
│   └── 05_Spillover_Analysis.ipynb
├── data/                   # Data directory (created by notebooks)
│   ├── raw/               # Downloaded asset price data
│   ├── processed/         # Cleaned and merged datasets
│   └── metadata/          # Asset classifications and metadata
├── outputs/               # Analysis results and visualizations
│   ├── causal_link/       # Causality analysis results
│   ├── ml/                # Machine learning predictions and SHAP values
│   ├── regime_switching/  # Regime identification outputs
│   ├── rolling/           # Rolling window spillover analysis
│   └── spillovers/        # Static spillover indices
├── README.md
├── requirements.txt
├── LICENSE
└── CITATION.cff
```

## Methodology

### 1. Volatility Modeling
- **GARCH(1,1)** models for univariate conditional volatility
- **DCC-GARCH** for dynamic conditional correlations
- Standardized residuals extracted for spillover analysis

### 2. Spillover Analysis
- **Diebold-Yilmaz (2012)** spillover indices based on forecast error variance decomposition
- Rolling window analysis (250-day windows) to capture time-varying spillovers
- Network analysis to identify key transmitters and receivers

### 3. Regime Identification
- **Hidden Markov Models (HMM)** for probabilistic regime classification
- **PELT** algorithm for structural break detection
- Multiple regime tracks (HMM-based and changepoint-based)

### 4. Machine Learning
- **XGBoost** and **LightGBM** for volatility forecasting
- Walk-forward validation to avoid look-ahead bias
- **SHAP** values for feature importance and model interpretation
- Diebold-Mariano tests for statistical significance of forecast improvements

## Outputs

All results are saved in the `outputs/` directory:
- **CSV files**: Spillover indices, regime labels, predictions, and summary statistics
- **Visualizations**: Heatmaps, time series plots, network graphs, and SHAP plots
- **Model artifacts**: Trained models (.json, .pkl), feature importances, and metrics

## Citation

If you use this code or methodology in your research, please cite:

```
Hobson, M. (2025). Cross-Asset Volatility Spillover Analysis.
MSc Dissertation, University College London.
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Author

**Miles Hobson**
MSc Responsible Finance and Alternative Assets
University College London
2025
