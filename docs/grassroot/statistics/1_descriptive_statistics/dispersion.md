## Measures of Dispersion

### 1. Variance and Standard Deviation

#### Population Variance (σ²)
$\sigma^2 = \frac{1}{N}\sum_{i=1}^{N} (x_i - \mu)^2$

#### Sample Variance (s²)
$s^2 = \frac{1}{n-1}\sum_{i=1}^{n} (x_i - \bar{x})^2$

#### Properties
##### Computational Formula:
   $s^2 = \frac{1}{n-1}(\sum_{i=1}^{n} x_i^2 - \frac{1}{n}(\sum_{i=1}^{n} x_i)^2)$

##### Algebraic Properties:
   $$ Var(aX + b) = a^2Var(X) $$
   $$ Var(X + Y) = Var(X) + Var(Y) + 2Cov(X,Y) $$
   For independent X,Y: $$ Var(X + Y) = Var(X) + Var(Y) $$

##### Standard Deviation:
   $\sigma = \sqrt{\sigma^2}$ or $s = \sqrt{s^2}$

#### Use Cases
- Most common measure of variability
- Optimal for normal distributions
- Input for many statistical procedures
- Basis for least squares estimation

### 2. Range and Interquartile Range (IQR)

#### Range
$R = x_{max} - x_{min}$

#### Interquartile Range
$IQR = Q_3 - Q_1$

where:
- Q₁ is the 25th percentile
- Q₃ is the 75th percentile

#### Properties
##### 1. Outlier Detection:
   - Lower fence: $Q_1 - 1.5 × IQR$
   - Upper fence: $Q_3 + 1.5 × IQR$
   - Points beyond fences are potential outliers

##### 2. Relationship to Standard Deviation:
   For normal distribution:
   - $IQR ≈ 1.349\sigma$
   - Range ≈ 4σ (for moderate sample sizes)

#### Use Cases
- Non-parametric analyses
- Robust statistics
- Box plots
- Quick dispersion estimates

### 3. Coefficient of Variation (CV)

#### Definition
$CV = \frac{s}{\bar{x}} \times 100\%$

#### Properties
##### 1. Scale Independence:
   - Unitless measure
   - Allows comparison across different scales

##### 2. Limitations:
   - Only meaningful for ratio scales
   - Not suitable when mean ≈ 0
   - Not defined for negative values

#### Use Cases
- Comparing variability across different units
- Quality control
- Investment risk assessment
- Biological variation studies

### 4. Mean Absolute Deviation (MAD)

#### Population MAD
$MAD = \frac{1}{N}\sum_{i=1}^{N} |x_i - \mu|$

#### Sample MAD
$MAD = \frac{1}{n}\sum_{i=1}^{n} |x_i - \bar{x}|$

#### Properties
##### 1. Relationship to Standard Deviation:
   For normal distribution:
   $MAD ≈ 0.8\sigma$

##### 2. Robustness:
   - Less sensitive to outliers than variance
   - L1 norm vs L2 norm for variance

#### Use Cases
- Robust statistics
- Financial risk measures
- Time series analysis
- Bio statistics

### 5. Covariance Matrix (Σ)

#### Definition
For random vectors X = [X₁, ..., Xₚ]:
$\Sigma_{ij} = Cov(X_i, X_j) = E[(X_i - \mu_i)(X_j - \mu_j)]$

#### Sample Covariance Matrix (S)
$S_{ij} = \frac{1}{n-1}\sum_{k=1}^{n} (x_{ki} - \bar{x}_i)(x_{kj} - \bar{x}_j)$

#### Properties
##### 1. Matrix Properties:
   - Symmetric: $\Sigma_{ij} = \Sigma_{ji}$
   - Positive semi-definite
   - Diagonal elements are variances

##### 2. Eigendecomposition:
   $\Sigma = PDP^T$
   where:
   - D: diagonal matrix of eigenvalues
   - P: matrix of eigenvectors

#### Use Cases
- Principal Component Analysis
- Multivariate analysis
- Portfolio optimization
- Machine learning algorithms

### 6. Relationships and Comparisons

#### 1. Robustness Hierarchy (most to least robust):
1. IQR
2. MAD
3. Standard Deviation
4. Range

#### 2. Efficiency under Normality (most to least efficient):
1. Standard Deviation
2. MAD
3. IQR
4. Range

#### 3. Computational Complexity:
- Range: O(n)
- IQR: O(n log n)
- MAD: O(n)
- Variance: O(n)
- Covariance Matrix: O(n²)

### 7. Selection Guidelines

**Use Standard Deviation when**:
   - Data is approximately normal
   - Need mathematical tractability
   - Working with inferential statistics

**Use IQR when**:
   - Data has outliers
   - Distribution is skewed
   - Need robust measure

**Use CV when**:
   - Comparing different scales
   - Working with strictly positive data
   - Need relative variation

**Use MAD when**:
   - Need robust measure
   - Working with time series
   - Dealing with non-normal data

