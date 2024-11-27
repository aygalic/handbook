# Simple Linear Regression: Mathematical Foundations

## Model Specification

### Basic Form
The simple linear regression model is expressed as:
```
Y = β₀ + β₁X + ε
```
where:
- Y is the dependent variable
- X is the independent variable
- β₀ is the y-intercept (value of Y when X = 0)
- β₁ is the slope (change in Y for one unit change in X)
- ε is the error term (random disturbance)

## Model Assumptions

### 1. Linearity
* **Mathematical Form**: E[Y|X] = β₀ + β₁X
* **Interpretation**: The conditional expectation of Y given X is a linear function
* **Violation Impact**: Biased estimates, poor predictions

### 2. Independence
* **Mathematical Form**: cov(εᵢ, εⱼ) = 0 for all i ≠ j
* **Interpretation**: No relationship between error terms
* **Detection**: Through autocorrelation function: ρₖ = cov(εₜ, εₜ₊ₖ)/var(ε)

### 3. Homoscedasticity
* **Mathematical Form**: var(ε|X) = σ²
* **Interpretation**: Constant variance of errors across all X values
* **Violation Impact**: Inefficient estimates, invalid standard errors

### 4. Normality
* **Mathematical Form**: ε ~ N(0, σ²)
* **Interpretation**: Errors follow normal distribution with mean 0 and constant variance
* **Importance**: Required for valid inference (t-tests, F-tests)

## Least Squares Estimation

### Optimization Problem
Find β₀ and β₁ that minimize the sum of squared residuals:
```
min Σ(Yᵢ - β₀ - β₁Xᵢ)²
```

### Solutions
The optimal estimates are given by:
```
β̂₁ = Σ((Xᵢ - X̄)(Yᵢ - Ȳ)) / Σ(Xᵢ - X̄)²
β̂₀ = Ȳ - β̂₁X̄
```
where X̄ and Ȳ are sample means

### Properties of OLS Estimators
1. **Unbiasedness**: E[β̂] = β
2. **Consistency**: β̂ → β as n → ∞
3. **Efficiency**: Minimum variance among linear unbiased estimators (BLUE)
4. **Sampling Distributions**:
   ```
   β̂₁ ~ N(β₁, σ²/Σ(Xᵢ - X̄)²)
   β̂₀ ~ N(β₀, σ²(1/n + X̄²/Σ(Xᵢ - X̄)²))
   ```

## Measures of Fit

### R-squared
* **Formula**: R² = 1 - SSE/SST
  where:
  - SSE = Σ(Yᵢ - Ŷᵢ)² (Sum of Squared Errors)
  - SST = Σ(Yᵢ - Ȳ)² (Total Sum of Squares)

* **Interpretation**: Proportion of variance in Y explained by X
* **Range**: [0,1], with 1 indicating perfect fit

### Adjusted R-squared
* **Formula**: R̄² = 1 - (1-R²)(n-1)/(n-p-1)
  where:
  - n = sample size
  - p = number of predictors (1 for simple regression)

* **Interpretation**: R² adjusted for model complexity
* **Advantage**: Penalizes unnecessary predictors

## Residual Analysis

### Residual Definition
```
eᵢ = Yᵢ - Ŷᵢ
```

### Standardized Residuals
```
rᵢ = eᵢ/(s√(1-hᵢᵢ))
```
where:
- s = standard error of regression
- hᵢᵢ = leverage of observation i

### Key Properties
1. **Sum**: Σeᵢ = 0
2. **Correlation**: Σ(Xᵢeᵢ) = 0
3. **Distribution**: Under assumptions, eᵢ/σ ~ N(0,1-hᵢᵢ)

## Statistical Inference

### Standard Errors
```
SE(β̂₁) = σ/√(Σ(Xᵢ - X̄)²)
SE(β̂₀) = σ√(1/n + X̄²/Σ(Xᵢ - X̄)²)
```

### Hypothesis Testing
For slope:
```
H₀: β₁ = 0
H₁: β₁ ≠ 0
t = β̂₁/SE(β̂₁) ~ t(n-2)
```

### Confidence Intervals
```
β̂₁ ± t(α/2,n-2)SE(β̂₁)
β̂₀ ± t(α/2,n-2)SE(β̂₀)
```

## Prediction

### Point Prediction
For new observation X₀:
```
Ŷ₀ = β̂₀ + β̂₁X₀
```

### Prediction Interval
```
Ŷ₀ ± t(α/2,n-2)s√(1 + 1/n + (X₀-X̄)²/Σ(Xᵢ-X̄)²)
```

### Confidence Interval for Mean Response
```
Ŷ₀ ± t(α/2,n-2)s√(1/n + (X₀-X̄)²/Σ(Xᵢ-X̄)²)
```

Remember:
1. Assumptions are crucial for valid inference
2. R² alone is insufficient for model assessment
3. Residual analysis provides insight into model adequacy
4. Prediction intervals are wider than confidence intervals
5. Model simplicity aids interpretation