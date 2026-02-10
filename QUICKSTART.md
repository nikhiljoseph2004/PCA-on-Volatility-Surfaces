# Quick Start Guide

Get up and running with PCA on Volatility Surfaces in 5 minutes!

## Prerequisites

- Python 3.8 or higher
- pip (Python package manager)
- Jupyter Notebook
- S&P 500 options data (CSV format)

## Step 1: Clone the Repository

```bash
git clone https://github.com/nikhiljoseph2004/PCA-on-Volatility-Surfaces.git
cd PCA-on-Volatility-Surfaces
```

## Step 2: Install Dependencies

```bash
pip install -r requirements.txt
```

This installs:
- numpy (numerical computing)
- pandas (data manipulation)
- scikit-learn (PCA implementation)
- plotly (interactive visualizations)
- jupyter (notebook environment)

## Step 3: Prepare Your Data

Place your S&P 500 options data CSV in the `data/` directory.

**Required columns:**
- `date`: Trading date
- `cp_flag`: 'C' for calls, 'P' for puts
- `strike_price`: Option strike
- `impl_volatility`: Implied volatility (decimal)
- `index_price`: S&P 500 level
- `days_to_expiration`: Days to maturity

**Example:** `data/options_SPX_2015_2024.csv`

See [data/README.md](data/README.md) for data sources and format details.

## Step 4: Launch Jupyter Notebook

```bash
jupyter notebook 4520_final.ipynb
```

Your browser will open with the notebook.

## Step 5: Configure and Run

### 5.1 Update Data Path

In the second code cell, change the file path:

```python
# For local file
file_path = 'data/options_SPX_2015_2024.csv'

# OR for Google Colab (if using Colab)
from google.colab import drive
drive.mount('/content/drive')
file_path = '/content/drive/MyDrive/your_folder/options_SPX.csv'
```

### 5.2 Run All Cells

- **Option 1**: Click "Cell" → "Run All"
- **Option 2**: Press `Shift + Enter` on each cell sequentially

### 5.3 Wait for Completion

The analysis takes approximately:
- Data loading: 10-30 seconds
- Surface construction: 2-5 minutes
- PCA computation: 10-30 seconds
- Visualizations: 30-60 seconds

**Total: ~5 minutes** (varies with data size)

## Step 6: Explore Results

### View Variance Explained

Look for output similar to:
```
PC1: 45.2% variance
PC2: 28.7% variance
PC3: 12.1% variance
---
Top 3 PCs explain 86.0% of total variance
```

### Interact with 3D Plots

- **Rotate**: Click and drag
- **Zoom**: Scroll wheel
- **Pan**: Right-click and drag
- **Hover**: View exact values

### Save Results (Optional)

Add a cell at the end:

```python
import numpy as np

# Save eigenmodes and factor scores
np.save('results/eigenmodes.npy', pca.components_)
np.save('results/factor_scores.npy', factor_scores)
np.save('results/explained_variance.npy', pca.explained_variance_ratio_)

print("✓ Results saved to results/ directory")
```

## What's Next?

### Customize the Analysis

1. **Adjust grid resolution**: Change moneyness/maturity grid size
2. **Modify kernel bandwidth**: Tune smoothing parameters
3. **Extract more components**: Set `n_components` in PCA
4. **Change date range**: Filter data to specific periods

See [docs/README.md](docs/README.md) for advanced usage.

### Understand the Results

- **Eigenmodes**: Surface deformation patterns (level, skew, curvature)
- **Factor scores**: Time series showing daily contributions
- **Variance explained**: Importance of each factor

### Dive Deeper

- Read [docs/methodology.md](docs/methodology.md) for technical details
- Review [docs/Final_Report.tex](docs/Final_Report.tex) for full analysis
- Check [CONTRIBUTING.md](CONTRIBUTING.md) to contribute

## Troubleshooting

### "Module not found" error

```bash
pip install --upgrade -r requirements.txt
```

### "File not found" error

Check your data path:
```python
import os
print(os.path.abspath('data/'))
print(os.listdir('data/'))
```

### "Empty DataFrame" after filtering

Your data format might differ. Check column names:
```python
print(df.columns.tolist())
print(df.head())
```

### Memory issues

Reduce the date range or grid resolution:
```python
# Filter to fewer days
df = df[df['date'].between('2020-01-01', '2024-12-31')]

# Use coarser grid
moneyness_grid = np.linspace(0.5, 1.2, 11)  # 11 instead of 21
maturity_grid = np.linspace(28, 365, 10)    # 10 instead of 20
```

## Getting Help

- **Documentation**: See [docs/README.md](docs/README.md)
- **Issues**: Open an issue on [GitHub](https://github.com/nikhiljoseph2004/PCA-on-Volatility-Surfaces/issues)
- **Discussions**: Check existing issues and pull requests

## Resources

- **Original paper**: Cont & da Fonseca (2002) - [DOI](https://doi.org/10.1088/1469-7688/2/1/304)
- **Options data**: See [data/README.md](data/README.md) for sources
- **Python docs**:
  - [NumPy](https://numpy.org/doc/)
  - [Pandas](https://pandas.pydata.org/docs/)
  - [Scikit-learn PCA](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html)
  - [Plotly](https://plotly.com/python/)

---

**Ready to start?** Run `jupyter notebook 4520_final.ipynb` and follow the notebook!

For detailed documentation, see the main [README.md](README.md).
