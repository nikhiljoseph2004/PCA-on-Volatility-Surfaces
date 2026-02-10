# Methodology Documentation

## Detailed Methodology for Volatility Surface PCA Analysis

This document provides technical details on the methodology used in this project.

---

## 1. Data Preprocessing

### 1.1 Data Collection
- **Asset**: S&P 500 Index (SPX) options
- **Time Period**: January 2015 - December 2024
- **Frequency**: Daily observations

### 1.2 Filtering Criteria

**Option Selection:**
- Include only **out-of-the-money (OTM)** options:
  - **Call options**: Moneyness *m* > 1 (strike > spot)
  - **Put options**: Moneyness *m* < 1 (strike < spot)
- **Rationale**: OTM options have purer volatility information without intrinsic value distortion

**Moneyness Range:**
- 0.5 ≤ *m* ≤ 1.2
- Where *m = K/S* (strike price / spot price)

**Time to Maturity:**
- Minimum: 28 days (avoid short-dated erratic behavior)
- Maximum: 365 days (1 year)

**Liquidity:**
- Non-zero volume or open interest preferred (if available)
- Valid implied volatility values (non-null, positive)

---

## 2. Volatility Surface Construction

### 2.1 Parametrization

The implied volatility is expressed as a function:

**I(m, τ) = σ_impl(K, T)**

where:
- *m = K/S*: Moneyness (strike/spot ratio)
- *τ = T - t*: Time to maturity (in days or years)

### 2.2 Smoothing: Nadaraya-Watson Kernel Regression

**Purpose**: Create smooth surface from discrete option quotes

**Method**: 2D Gaussian kernel regression

**Formula**:

```
Î(m, τ) = Σᵢ wᵢ(m, τ) × σᵢ
```

where the weights are:

```
wᵢ(m, τ) = K((m - mᵢ)/h_m, (τ - τᵢ)/h_τ) / Σⱼ K((m - mⱼ)/h_m, (τ - τⱼ)/h_τ)
```

**Kernel Function**: Gaussian (normal) kernel
```
K(u, v) = exp(-0.5 × (u² + v²))
```

**Bandwidth Selection**:
- *h_m*: Bandwidth for moneyness dimension (typical: 0.05-0.10)
- *h_τ*: Bandwidth for maturity dimension (typical: 10-30 days)

**Grid**:
- Moneyness: 21 points uniformly spaced between 0.5 and 1.2
- Maturity: 20 points uniformly spaced between 28 and 365 days
- Total: 21 × 20 = **420 surface points per day**

---

## 3. Surface Dynamics: Daily Changes

### 3.1 Log-Volatility Transformation

Compute daily changes in **log-implied volatility**:

```
ΔI_t(m, τ) = ln(I_t(m, τ)) - ln(I_{t-1}(m, τ))
```

**Rationale**:
- Log transformation ensures multiplicative changes are additive
- Better statistical properties (closer to normality)
- Dimensionless measure

### 3.2 Matrix Construction

Stack daily changes into a matrix:

```
X = [ΔI_1, ΔI_2, ..., ΔI_T]ᵀ
```

where:
- Each row represents one trading day
- Each column represents one (moneyness, maturity) point on the grid
- Dimensions: (T days, 420 features)

---

## 4. Principal Component Analysis (PCA)

### 4.1 Karhunen-Loève Decomposition

**Goal**: Decompose surface changes into orthogonal eigenmodes

**Mathematical Framework**:

```
ΔI_t(m, τ) = Σₖ αₖ(t) × φₖ(m, τ) + ε_t(m, τ)
```

where:
- *φₖ(m, τ)*: k-th eigenmode (principal component)
- *αₖ(t)*: k-th factor score at time *t*
- *ε_t*: Residual noise

### 4.2 Implementation

**Step 1: Standardization** (optional)
- Center the data: Subtract mean across time
- Standard scaling may be applied

**Step 2: Covariance Matrix**
```
C = (1/T) × XᵀX
```

**Step 3: Eigendecomposition**
```
C × φₖ = λₖ × φₖ
```

Solve for:
- Eigenvalues *λₖ*: Variance explained by mode *k*
- Eigenvectors *φₖ*: Shape of mode *k*

**Step 4: Factor Scores**
```
α(t) = X(t) · φₖ
```

### 4.3 Variance Explained

```
Variance Ratio_k = λₖ / Σⱼ λⱼ
```

**Typical Results**:
- First 3 modes: 70-85% of total variance
- First 5 modes: 85-95% of total variance

---

## 5. Interpretation of Eigenmodes

### Mode 1: Level Shift
- **Shape**: Relatively flat across moneyness and maturity
- **Effect**: Parallel shift of entire surface up or down
- **Variance**: ~40-50% of total variance

### Mode 2: Slope/Skew
- **Shape**: Linear in moneyness, varying with maturity
- **Effect**: Rotation of volatility smile (steepening/flattening skew)
- **Variance**: ~20-30%

### Mode 3: Curvature/Smile
- **Shape**: Quadratic in moneyness
- **Effect**: Changes in smile curvature (convexity)
- **Variance**: ~10-15%

---

## 6. Statistical Tests

### 6.1 Sticky Moneyness Hypothesis

**Null Hypothesis**: Volatility surface remains fixed in (*m*, *τ*) coordinates

**Test**:
- If true: ΔI_t(m, τ) ≈ 0 after accounting for spot price changes
- Compare variance before/after transformation to (*m*, *τ*) parametrization

**Typical Finding**: Hypothesis is **rejected** - surface is not sticky in moneyness

---

## 7. Validation and Robustness

### 7.1 Cross-Validation
- Split data into training and test periods
- Verify that eigenmode structure is stable across subperiods

### 7.2 Sensitivity Analysis
- Test different bandwidth choices for kernel regression
- Vary moneyness/maturity ranges
- Check robustness to outlier removal

### 7.3 Comparison with Literature
- Compare to Cont & da Fonseca (2002) findings
- Assess differences between 1990s and 2015-2024 markets

---

## 8. Computational Complexity

**Surface Construction**: O(N × M) per day
- N: Number of option quotes per day (~500-2000)
- M: Number of grid points (420)

**PCA**: O(min(T², M²))
- T: Number of days (~2500)
- M: Number of features (420)

**Total Runtime**: Minutes to hours depending on data size

---

## References

1. Cont, R., & da Fonseca, J. (2002). Dynamics of implied volatility surfaces. *Quantitative Finance*, 2(1), 45-60.
2. Nadaraya, E. A. (1964). On estimating regression. *Theory of Probability & Its Applications*, 9(1), 141-142.
3. Watson, G. S. (1964). Smooth regression analysis. *Sankhyā: The Indian Journal of Statistics*, Series A, 359-372.
4. Pearson, K. (1901). On lines and planes of closest fit to systems of points in space. *Philosophical Magazine*, 2(11), 559-572.

---

## Implementation Notes

- **Libraries**: scikit-learn for PCA, numpy for numerical operations
- **Memory**: Store intermediate results to avoid recomputation
- **Visualization**: Plotly for interactive 3D surface plots
- **Reproducibility**: Fix random seeds if applicable, document all parameters
