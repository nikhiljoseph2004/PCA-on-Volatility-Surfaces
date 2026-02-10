# Principal Component Analysis on Volatility Surfaces

![Python](https://img.shields.io/badge/python-3.8+-blue.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)

## Overview

This project performs **Principal Component Analysis (PCA)** on implied volatility surfaces of S&P 500 index options, replicating and extending the seminal work by Cont and da Fonseca (2002). Using daily options data from 2015 to 2024, we identify the dominant factors driving volatility surface dynamics and test the "sticky moneyness" hypothesis.

### Key Features

- 📊 **Non-parametric surface construction** using Nadaraya-Watson kernel regression
- 🔍 **Karhunen-Loève decomposition** to extract orthogonal eigenmodes
- 📈 **Factor analysis** revealing low-dimensional structure of volatility changes
- 🎯 **Empirical testing** of the sticky moneyness rule
- 🌐 **Interactive 3D visualizations** of volatility surfaces and eigenmodes

## Background

The implied volatility surface is a critical tool in options pricing and risk management, representing the market's expectations of future volatility across different strikes and maturities. Unlike the Black-Scholes model which assumes constant volatility, empirical observations reveal persistent features such as volatility smiles, skews, and term structures.

This project models the volatility surface as a randomly fluctuating field driven by a small number of orthogonal factors. By applying functional PCA to daily surface changes, we uncover the fundamental modes that explain the majority of variance in surface evolution.

## Methodology

### Data Processing

1. **Data Source**: S&P 500 index options from 2015-2024
2. **Filtering**: 
   - Out-of-the-money (OTM) options only
   - Calls with moneyness *m = K/S > 1*
   - Puts with moneyness *m = K/S < 1*
   - Time-to-maturity: 28 days to 1 year
3. **Parametrization**: Surface expressed in terms of:
   - Moneyness: *m = K/S* (strike/spot ratio)
   - Time-to-maturity: *τ = T - t*

### Surface Construction

- **Method**: 2D Nadaraya-Watson Gaussian kernel regression
- **Grid**: 21 moneyness points × 20 maturity points
- **Output**: Smooth daily implied volatility surfaces

### PCA Analysis

1. Compute daily changes in log-implied volatility: Δ ln *I_t*
2. Flatten each surface into a 420-dimensional vector
3. Apply PCA to the matrix of daily changes
4. Extract:
   - Principal components (eigenmodes)
   - Factor scores (time series)
   - Eigenvalues and explained variance ratios

## Installation

### Prerequisites

- Python 3.8 or higher
- pip package manager

### Setup

1. Clone the repository:
```bash
git clone https://github.com/nikhiljoseph2004/PCA-on-Volatility-Surfaces.git
cd PCA-on-Volatility-Surfaces
```

2. Install required packages:
```bash
pip install -r requirements.txt
```

## Usage

### Running the Analysis

1. **Prepare your data**: Place S&P 500 options data in CSV format with columns:
   - `date`: Trading date
   - `cp_flag`: Call/Put indicator ('C' or 'P')
   - `strike_price`: Option strike price
   - `impl_volatility`: Implied volatility
   - `index_price`: S&P 500 index level
   - `days_to_expiration`: Time to maturity in days

2. **Open the Jupyter notebook**:
```bash
jupyter notebook 4520_final.ipynb
```

3. **Update the data path** in the notebook to point to your CSV file

4. **Run all cells** to:
   - Load and filter the data
   - Construct volatility surfaces
   - Perform PCA analysis
   - Generate visualizations

### Expected Output

- **Eigenmodes**: The principal components representing orthogonal surface deformation patterns
- **Factor scores**: Time series of each component's contribution
- **Variance explained**: Percentage of total variance captured by each mode
- **3D visualizations**: Interactive plots of surfaces and eigenmodes

## Key Findings

Our analysis of 2015-2024 S&P 500 options data reveals:

1. **Low-dimensional structure**: 3 dominant factors explain the majority of variance in volatility surface changes
2. **Interpretable modes**: 
   - First mode: Overall level shifts
   - Second mode: Slope/skew changes
   - Third mode: Curvature/smile adjustments
3. **Sticky moneyness violation**: The surface does not remain fixed in (*m*, *τ*) coordinates, contradicting a common modeling assumption
4. **Market evolution**: Compared to 1990s data, modern markets show smaller level effects and more pronounced skew dynamics

## Project Structure

```
PCA-on-Volatility-Surfaces/
├── 4520_final.ipynb          # Main analysis notebook
├── README.md                  # This file
├── requirements.txt           # Python dependencies
├── LICENSE                    # MIT License
├── .gitignore                # Git ignore rules
├── data/                      # Data directory (not tracked)
│   └── README.md             # Data documentation
├── results/                   # Analysis outputs (not tracked)
│   └── README.md             # Results documentation
└── docs/                      # Additional documentation
    ├── Final_Report.tex      # LaTeX source for final report
    └── methodology.md        # Detailed methodology notes
```

## Dependencies

- **numpy**: Numerical computations and array operations
- **pandas**: Data manipulation and analysis
- **scikit-learn**: PCA implementation
- **plotly**: Interactive 3D visualizations
- **jupyter**: Notebook environment

See `requirements.txt` for specific versions.

## References

- Cont, R., & da Fonseca, J. (2002). **Dynamics of implied volatility surfaces**. *Quantitative Finance*, 2(1), 45-60.
- Black, F., & Scholes, M. (1973). **The pricing of options and corporate liabilities**. *Journal of Political Economy*, 81(3), 637-654.

## Authors

- **Oliwer Aleksander Wirkus**
- **Nikhil Kurian Joseph** - [GitHub](https://github.com/nikhiljoseph2004)
- **Ananya Singhvi**

*IEDA 4520 - December 2025*

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- Course: IEDA 4520
- Institution: Hong Kong University of Science and Technology (HKUST)
- Inspired by the work of Cont and da Fonseca (2002)

## Future Work

- Extend analysis to other asset classes (individual stocks, FX, commodities)
- Investigate time-varying factor loadings
- Develop forecasting models based on identified factors
- Compare with alternative dimensionality reduction techniques (e.g., Independent Component Analysis)

## Contact

For questions or collaboration opportunities, please open an issue on GitHub or contact the authors directly.

---

**Note**: The data used in this analysis is not included in this repository due to licensing restrictions. Please obtain S&P 500 options data from a licensed data provider such as Bloomberg, OptionMetrics, or CBOE DataShop.
