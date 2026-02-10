# Results Directory

This directory stores the outputs of the PCA analysis on volatility surfaces.

## Generated Files

When you run the analysis notebook (`4520_final.ipynb`), the following files may be generated:

### NumPy Arrays

- **eigenmodes.npy**: Principal components (eigenvectors) representing orthogonal surface deformation patterns
- **factor_scores.npy**: Time series of factor loadings for each principal component
- **eigenvalues.npy**: Variance explained by each eigenmode
- **explained_variance_ratio.npy**: Percentage of total variance captured by each mode

### Visualization Outputs

- **surface_*.html**: Interactive 3D plots of volatility surfaces (Plotly HTML files)
- **eigenmode_*.html**: Interactive 3D visualizations of each eigenmode
- **factor_scores_*.png**: Time series plots of principal component scores

### Intermediate Data

- **daily_surfaces.npy**: Smoothed volatility surface data for each trading day
- **surface_changes.npy**: Day-to-day changes in log-implied volatility

## Data Format

### Eigenmodes Array Shape
- Dimensions: (n_components, n_moneyness × n_maturity)
- Example: (10, 420) for 10 components on a 21×20 grid
- Each row represents one eigenmode (principal component)

### Factor Scores Array Shape
- Dimensions: (n_days, n_components)
- Example: (2500, 10) for ~10 years of daily data with 10 components
- Each column is the time series of one factor's contribution

## Usage Example

```python
import numpy as np

# Load results
eigenmodes = np.load('results/eigenmodes.npy')
factor_scores = np.load('results/factor_scores.npy')
explained_variance = np.load('results/explained_variance_ratio.npy')

# Print variance explained by first 3 modes
print(f"First 3 modes explain: {explained_variance[:3].sum():.2%} of variance")

# Access first eigenmode
first_mode = eigenmodes[0].reshape(21, 20)  # Reshape to 2D surface
```

## Storage Notes

⚠️ **This directory is excluded from version control** (via `.gitignore`) to:
- Avoid large file storage in Git
- Prevent repository bloat
- Allow users to generate their own results

If you want to share results, consider:
1. Using Git LFS (Large File Storage) for binary files
2. Compressing results into a single archive
3. Uploading to external storage (Google Drive, Dropbox) and providing links
4. Including only summary statistics and visualizations in the repository

## File Organization

Recommended structure for multiple analysis runs:

```
results/
├── run_2015_2024/
│   ├── eigenmodes.npy
│   ├── factor_scores.npy
│   └── visualizations/
├── run_2020_2024/
│   ├── eigenmodes.npy
│   └── factor_scores.npy
└── comparison/
    └── variance_comparison.csv
```

This allows you to compare results from different time periods or parameter settings.
