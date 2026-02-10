# Data Directory

This directory is intended for storing S&P 500 options data used in the volatility surface analysis.

## Expected Data Format

The analysis expects CSV files with the following columns:

- **date**: Trading date (YYYY-MM-DD format)
- **cp_flag**: Call/Put indicator ('C' for calls, 'P' for puts)
- **strike_price**: Option strike price (float)
- **impl_volatility**: Implied volatility (decimal, e.g., 0.20 for 20%)
- **index_price**: S&P 500 index level at observation time (float)
- **days_to_expiration**: Days until option expiration (integer)

### Example Data Structure

```csv
date,cp_flag,strike_price,impl_volatility,index_price,days_to_expiration
2015-01-02,C,2100,0.185,2058.20,30
2015-01-02,P,2050,0.178,2058.20,30
2015-01-02,C,2150,0.192,2058.20,60
...
```

## Data Sources

Due to licensing restrictions, the data is **not included** in this repository. You can obtain S&P 500 options data from:

- **OptionMetrics**: Comprehensive historical options database (academic access available)
- **Bloomberg Terminal**: Bloomberg Options (OMON) function
- **CBOE DataShop**: Historical options data marketplace
- **Quandl/Nasdaq Data Link**: Various options datasets
- **Yahoo Finance**: Limited free historical options data (via `yfinance` Python package)

## Data Filtering Recommendations

For best results, filter the data to include:

1. **Liquidity**: Options with non-zero volume and open interest
2. **Moneyness range**: 0.5 ≤ *m* ≤ 1.2 (where *m* = strike/spot)
3. **Time to maturity**: 28 days ≤ TTM ≤ 365 days
4. **Option type**: Out-of-the-money (OTM) options preferred:
   - Calls: *m* > 1
   - Puts: *m* < 1
5. **Data quality**: Remove entries with missing or invalid implied volatility values

## File Naming Convention

Suggested naming format: `options_SPX_YYYYMMDD_YYYYMMDD.csv`

Example: `options_SPX_20150101_20241231.csv`

## Privacy and Security

⚠️ **Important**: This directory is excluded from version control (via `.gitignore`) to:
- Comply with data licensing agreements
- Prevent accidental sharing of proprietary data
- Keep repository size manageable

Ensure you have the necessary licenses and permissions before using any financial data.
