# Documentation

Welcome to the documentation for the **PCA on Volatility Surfaces** project.

## Quick Links

- [Getting Started](#getting-started)
- [Data Requirements](#data-requirements)
- [Running the Analysis](#running-the-analysis)
- [Understanding Results](#understanding-results)
- [Detailed Methodology](methodology.md)
- [LaTeX Report](Final_Report.tex)

---

## Getting Started

### What is this project?

This project applies **Principal Component Analysis (PCA)** to the implied volatility surface of S&P 500 options, identifying the key factors that drive its evolution over time. It replicates the seminal work by Cont and da Fonseca (2002) using modern data from 2015-2024.

### Why is this important?

Understanding volatility surface dynamics is crucial for:
- **Option traders**: Identifying opportunities and risks
- **Risk managers**: Hedging Vega exposure across strikes and maturities
- **Quantitative researchers**: Building better pricing and forecasting models
- **Volatility arbitrageurs**: Detecting mispricings

### Key Concepts

**Implied Volatility Surface**: A 3D representation showing how implied volatility varies across:
- **Moneyness** (*m = K/S*): How far the strike is from the current spot price
- **Time to Maturity** (*τ*): Days until option expiration

**Principal Component Analysis**: A dimensionality reduction technique that identifies orthogonal "factors" or "modes" that explain the most variance in the data.

**Karhunen-Loève Decomposition**: A functional PCA approach for continuous fields (like our volatility surface).

---

## Data Requirements

### Required Format

Your data should be a CSV file with these columns:

| Column | Type | Description | Example |
|--------|------|-------------|---------|
| `date` | Date | Trading date | 2015-01-02 |
| `cp_flag` | String | 'C' for call, 'P' for put | C |
| `strike_price` | Float | Option strike | 2100.0 |
| `impl_volatility` | Float | Implied vol (decimal) | 0.185 |
| `index_price` | Float | S&P 500 level | 2058.20 |
| `days_to_expiration` | Integer | Days to maturity | 30 |

### Data Quality Checklist

- ✅ No missing values in critical columns
- ✅ Positive implied volatilities
- ✅ Reasonable moneyness range (0.5 to 1.5)
- ✅ Multiple expiration dates per day
- ✅ Sufficient liquidity (non-zero volume if available)

### Where to Get Data

See [data/README.md](../data/README.md) for detailed guidance on obtaining S&P 500 options data.

---

## Running the Analysis

### Step-by-Step Guide

#### 1. Install Dependencies

```bash
pip install -r requirements.txt
```

#### 2. Prepare Your Data

Place your CSV file in the `data/` directory or note its path.

#### 3. Open the Notebook

```bash
jupyter notebook 4520_final.ipynb
```

#### 4. Configure Data Path

In the second code cell, update the file path:

```python
file_path = '/path/to/your/data/options_SPX.csv'
```

If using Google Colab (as the notebook is configured):
```python
from google.colab import drive
drive.mount('/content/drive')
file_path = '/content/drive/MyDrive/your_folder/options_SPX.csv'
```

#### 5. Run All Cells

Execute cells sequentially (or "Run All"):
1. Import libraries
2. Load and filter data
3. Construct daily volatility surfaces
4. Compute surface changes
5. Perform PCA
6. Visualize results

#### 6. Examine Outputs

Results will be displayed inline and can be saved to `results/` directory.

---

## Understanding Results

### Eigenvalues and Variance Explained

The analysis produces eigenvalues *λₖ* representing the variance captured by each principal component.

**Example Output:**
```
PC1: 45.2% variance
PC2: 28.7% variance
PC3: 12.1% variance
---
Top 3 PCs: 86.0% total variance
```

**Interpretation**: The first three factors explain 86% of all variation in the volatility surface. This confirms a **low-dimensional structure**.

### Eigenmodes (Principal Components)

Each eigenmode *φₖ(m, τ)* is a 2D function showing how the surface deforms for that factor.

**Typical Patterns:**

#### Mode 1: Level Shift
![Level](https://via.placeholder.com/400x300?text=PC1:+Parallel+Shift)
- Mostly flat across moneyness and maturity
- Represents overall volatility regime changes
- Captures market-wide volatility increases/decreases

#### Mode 2: Skew Rotation
![Skew](https://via.placeholder.com/400x300?text=PC2:+Skew+Change)
- Linear in moneyness, varying with maturity
- Represents changes in put-call skew
- Related to risk sentiment and hedging demand

#### Mode 3: Smile Curvature
![Curvature](https://via.placeholder.com/400x300?text=PC3:+Smile+Adjustment)
- Quadratic in moneyness
- Represents changes in smile convexity
- Related to jump risk and fat-tail expectations

### Factor Scores (Time Series)

Factor scores *αₖ(t)* show how much each mode contributes on each day.

**Usage**:
- **Trading signals**: Large factor scores may indicate opportunities
- **Risk monitoring**: Track exposure to each factor
- **Regime detection**: Identify structural changes in market behavior

**Example Analysis**:
```python
import numpy as np
import matplotlib.pyplot as plt

# Load factor scores
factor_scores = np.load('results/factor_scores.npy')

# Plot first factor over time
plt.figure(figsize=(12, 4))
plt.plot(factor_scores[:, 0])
plt.title('Factor 1 Time Series (Level Shifts)')
plt.xlabel('Trading Days')
plt.ylabel('Factor Loading')
plt.show()

# Find days with extreme factor loadings
threshold = 2.0  # 2 standard deviations
extreme_days = np.where(np.abs(factor_scores[:, 0]) > threshold)[0]
print(f"Extreme events on days: {extreme_days}")
```

### Visualization: 3D Surface Plots

The notebook generates interactive Plotly visualizations:

1. **Original surfaces**: Daily volatility surfaces
2. **Eigenmodes**: 3D plots of each principal component
3. **Reconstructions**: Surface approximations using top *k* factors

**Interactivity**:
- Rotate: Click and drag
- Zoom: Scroll or pinch
- Pan: Right-click and drag
- Hover: See exact values

---

## Advanced Usage

### Customizing the Analysis

#### Change Grid Resolution

In the surface construction cell:

```python
# Original: 21 x 20 grid
moneyness_grid = np.linspace(0.5, 1.2, 21)
maturity_grid = np.linspace(28, 365, 20)

# Higher resolution: 41 x 40 grid
moneyness_grid = np.linspace(0.5, 1.2, 41)
maturity_grid = np.linspace(28, 365, 40)
```

⚠️ **Note**: Higher resolution increases computational cost quadratically.

#### Adjust Kernel Bandwidth

For Nadaraya-Watson smoothing:

```python
# Increase for more smoothing (less noise, more bias)
bandwidth_moneyness = 0.10
bandwidth_maturity = 30

# Decrease for less smoothing (more noise, less bias)
bandwidth_moneyness = 0.05
bandwidth_maturity = 15
```

#### Number of Principal Components

```python
# Extract top 10 components instead of all
pca = PCA(n_components=10)
factor_scores = pca.fit_transform(surface_changes)
```

### Saving Results

Add cells to save outputs:

```python
import numpy as np

# Save eigenmodes
np.save('results/eigenmodes.npy', pca.components_)

# Save factor scores
np.save('results/factor_scores.npy', factor_scores)

# Save explained variance
np.save('results/explained_variance.npy', pca.explained_variance_ratio_)

print("Results saved to results/ directory")
```

### Exporting Visualizations

For Plotly figures:

```python
# Save as HTML (interactive)
fig.write_html('results/eigenmode_1.html')

# Save as static image (requires kaleido)
fig.write_image('results/eigenmode_1.png', width=1200, height=800)
```

---

## Troubleshooting

### Common Issues

#### "File not found" Error

**Problem**: Data file path is incorrect

**Solution**: 
- Check the file path is absolute or relative to notebook location
- Ensure file exists: `ls data/options_SPX.csv`
- Use tab completion in Jupyter to verify path

#### "Empty DataFrame" After Filtering

**Problem**: Data doesn't match expected format or all filtered out

**Solution**:
- Print column names: `df.columns`
- Check data types: `df.dtypes`
- Verify date parsing: `df['date'].dtype` should be datetime
- Relax filter criteria temporarily to debug

#### "Memory Error" During PCA

**Problem**: Dataset too large for available RAM

**Solution**:
- Reduce date range
- Use lower grid resolution
- Use incremental PCA: `from sklearn.decomposition import IncrementalPCA`

#### Kernel Regression Returns NaN

**Problem**: Insufficient data points for smoothing

**Solution**:
- Reduce bandwidth (increase locality)
- Check data density in moneyness-maturity space
- Remove grid points with no nearby data

#### Visualizations Don't Display

**Problem**: Plotly rendering issue

**Solution**:
- Restart kernel and run all cells
- Install/update: `pip install --upgrade plotly`
- For Jupyter Lab: `pip install jupyterlab plotly`

---

## Performance Tips

### Speed Optimizations

1. **Vectorize operations**: Use NumPy array operations instead of loops
2. **Reduce data size**: Filter to essential date range before analysis
3. **Cache surfaces**: Save constructed surfaces to avoid recomputation
4. **Parallel processing**: Use joblib or multiprocessing for independent days

### Memory Optimization

1. **Delete intermediate variables**: Use `del` for large arrays no longer needed
2. **Generator patterns**: Process days iteratively instead of loading all at once
3. **Sparse storage**: If surfaces are sparse, use `scipy.sparse`

---

## Further Reading

### Academic References

1. **Cont & da Fonseca (2002)**: Original paper on volatility surface dynamics
2. **Gatheral (2006)**: *The Volatility Surface* - Comprehensive textbook
3. **Heston (1993)**: Stochastic volatility model
4. **Dupire (1994)**: Local volatility model

### Related Topics

- **Volatility modeling**: GARCH, stochastic volatility
- **Option pricing**: Black-Scholes, local volatility, stochastic volatility
- **Risk management**: Greeks, Vega hedging
- **Factor models**: Arbitrage Pricing Theory, Fama-French

---

## Contact and Support

For questions, issues, or contributions:

1. **Open an issue** on GitHub
2. **Refer to** [CONTRIBUTING.md](../CONTRIBUTING.md)
3. **Email authors** (see README)

---

**Last Updated**: February 2025
