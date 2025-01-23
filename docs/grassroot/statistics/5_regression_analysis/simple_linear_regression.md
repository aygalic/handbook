# Simple Linear Regression

## Model Fundamentals
Linear regression models the relationship between a dependent variable y and a single independent variable x:
```
y = β₀ + β₁x + ε
```
where:
- β₀ is the intercept
- β₁ is the slope
- ε is the error term

## Assumptions

### 1. Linearity
* Relationship between x and y is linear
* Can be checked through scatter plots

```python
def check_linearity(X, y):
    """
    Plot data and check linearity assumption
    """
    plt.scatter(X, y)
    plt.xlabel('Independent Variable')
    plt.ylabel('Dependent Variable')
    
    # Add lowess smoother for comparison
    from statsmodels.nonparametric.smoothers_lowess import lowess
    smooth = lowess(y, X.ravel(), frac=2/3)
    plt.plot(smooth[:, 0], smooth[:, 1], 'r-', label='LOWESS')
```

### 2. Independence
* Observations are independent of each other
* No autocorrelation in residuals

```python
def check_independence(residuals):
    """
    Check independence using Durbin-Watson test
    """
    from statsmodels.stats.stattools import durbin_watson
    dw_stat = durbin_watson(residuals)
    
    return {
        'dw_statistic': dw_stat,
        'is_independent': 1.5 < dw_stat < 2.5
    }
```

### 3. Homoscedasticity
* Constant variance of residuals
* Can be checked through residual plots

```python
def check_homoscedasticity(model, X, residuals):
    """
    Plot residuals vs fitted values
    """
    fitted_values = model.predict(X)
    plt.scatter(fitted_values, residuals)
    plt.axhline(y=0, color='r', linestyle='--')
    plt.xlabel('Fitted Values')
    plt.ylabel('Residuals')
```

### 4. Normality of Residuals
* Residuals should be normally distributed
* Can be checked through QQ plots and tests

```python
def check_normality(residuals):
    """
    Check normality of residuals
    """
    from scipy import stats
    
    # QQ plot
    stats.probplot(residuals, dist="norm", plot=plt)
    
    # Shapiro-Wilk test
    _, p_value = stats.shapiro(residuals)
    
    return {
        'p_value': p_value,
        'is_normal': p_value > 0.05
    }
```

## Least Squares Estimation

### Ordinary Least Squares (OLS)
```python
def fit_ols(X, y):
    """
    Fit simple linear regression using OLS
    """
    from sklearn.linear_model import LinearRegression
    model = LinearRegression()
    model.fit(X.reshape(-1, 1), y)
    
    return {
        'intercept': model.intercept_,
        'slope': model.coef_[0],
        'model': model
    }
```

### Manual Implementation
```python
def manual_ols(X, y):
    """
    Manual implementation of OLS
    """
    X_mean = np.mean(X)
    y_mean = np.mean(y)
    
    # Calculate slope
    numerator = np.sum((X - X_mean) * (y - y_mean))
    denominator = np.sum((X - X_mean)**2)
    slope = numerator / denominator
    
    # Calculate intercept
    intercept = y_mean - slope * X_mean
    
    return {
        'intercept': intercept,
        'slope': slope
    }
```

## R-squared and Adjusted R-squared

### R-squared
```python
def calculate_r_squared(y_true, y_pred):
    """
    Calculate R-squared
    """
    ss_total = np.sum((y_true - np.mean(y_true))**2)
    ss_residual = np.sum((y_true - y_pred)**2)
    
    r_squared = 1 - (ss_residual / ss_total)
    return r_squared
```

### Adjusted R-squared
```python
def calculate_adjusted_r_squared(r_squared, n, p):
    """
    Calculate adjusted R-squared
    n: number of observations
    p: number of predictors (1 for simple linear regression)
    """
    adjusted_r_squared = 1 - (1 - r_squared) * (n - 1) / (n - p - 1)
    return adjusted_r_squared
```

## Residual Analysis

### Comprehensive Residual Analysis
```python
class ResidualAnalyzer:
    def __init__(self, model, X, y):
        self.model = model
        self.X = X
        self.y = y
        self.residuals = self.calculate_residuals()
        
    def calculate_residuals(self):
        """Calculate residuals"""
        y_pred = self.model.predict(self.X.reshape(-1, 1))
        return self.y - y_pred
    
    def standardized_residuals(self):
        """Calculate standardized residuals"""
        return stats.zscore(self.residuals)
    
    def run_all_checks(self):
        """Run all residual diagnostics"""
        results = {
            'normality': check_normality(self.residuals),
            'independence': check_independence(self.residuals),
            'homoscedasticity': self.check_homoscedasticity(),
            'outliers': self.check_outliers()
        }
        return results
    
    def check_outliers(self, threshold=3):
        """Check for outliers using standardized residuals"""
        std_resid = self.standardized_residuals()
        outliers = np.abs(std_resid) > threshold
        return {
            'n_outliers': sum(outliers),
            'outlier_indices': np.where(outliers)[0]
        }
    
    def check_homoscedasticity(self):
        """
        Breusch-Pagan test for homoscedasticity
        """
        from statsmodels.stats.diagnostic import het_breuschpagan
        
        y_pred = self.model.predict(self.X.reshape(-1, 1))
        _, p_value, _ = het_breuschpagan(self.residuals, self.X)
        
        return {
            'p_value': p_value,
            'is_homoscedastic': p_value > 0.05
        }
```

### Complete Regression Analysis
```python
def complete_regression_analysis(X, y):
    """
    Perform complete regression analysis
    """
    # Fit model
    model = fit_ols(X, y)
    
    # Get predictions
    y_pred = model['model'].predict(X.reshape(-1, 1))
    
    # Calculate metrics
    r_squared = calculate_r_squared(y, y_pred)
    adj_r_squared = calculate_adjusted_r_squared(r_squared, len(y), 1)
    
    # Analyze residuals
    analyzer = ResidualAnalyzer(model['model'], X, y)
    residual_analysis = analyzer.run_all_checks()
    
    return {
        'model': model,
        'r_squared': r_squared,
        'adj_r_squared': adj_r_squared,
        'residual_analysis': residual_analysis
    }
```

## Best Practices

### Model Building Process
1. **Data Preparation**
    - Check for missing values
    - Handle outliers
    - Scale if necessary

2. **Model Fitting**
    - Fit model using OLS
    - Calculate confidence intervals
    - Assess significance

3. **Model Validation**
    - Check assumptions
    - Analyze residuals
    - Assess fit metrics

4. **Reporting**
    - Coefficient estimates
    - Standard errors
    - R-squared values
    - Assumption validations

Remember:
1. Always check assumptions
2. Look for influential points
3. Consider transformations if needed
4. Report comprehensive results
5. Interpret findings in context