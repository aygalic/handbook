# Statistical Estimation

## Point Estimates

### Fundamentals
* **Definition**: Single value estimate of a population parameter
* **Properties of Good Estimators**:
    1. Unbiasedness: E[θ̂] = θ
    2. Consistency: θ̂ → θ as n → ∞
    3. Efficiency: Minimum variance among unbiased estimators
    4. Sufficiency: Contains all information about θ

### Common Point Estimators
```python
import numpy as np
from scipy import stats

def sample_estimators(data):
    """
    Calculate common point estimates
    """
    return {
        'mean': np.mean(data),
        'median': np.median(data),
        'variance': np.var(data, ddof=1),
        'std_dev': np.std(data, ddof=1),
        'skewness': stats.skew(data),
        'kurtosis': stats.kurtosis(data)
    }
```

## Confidence Intervals

### Basic Concepts
* **Definition**: Range of values likely to contain the true parameter
* **Interpretation**: If we repeat sampling many times, (1-α)% of intervals contain true parameter

### Types of Intervals

#### Mean (Normal Distribution)
```python
def normal_ci(data, confidence=0.95):
    """
    Calculate confidence interval for the mean
    assuming normal distribution
    """
    n = len(data)
    mean = np.mean(data)
    sem = stats.sem(data)
    ci = stats.t.interval(confidence, 
                         df=n-1,
                         loc=mean,
                         scale=sem)
    return {
        'mean': mean,
        'ci_lower': ci[0],
        'ci_upper': ci[1]
    }
```

#### Proportion
```python
def proportion_ci(successes, n, confidence=0.95):
    """
    Calculate confidence interval for a proportion
    """
    p_hat = successes / n
    z = stats.norm.ppf((1 + confidence) / 2)
    margin = z * np.sqrt(p_hat * (1 - p_hat) / n)
    
    return {
        'proportion': p_hat,
        'ci_lower': max(0, p_hat - margin),
        'ci_upper': min(1, p_hat + margin)
    }
```

#### Bootstrap Confidence Intervals
```python
def bootstrap_ci(data, statistic, confidence=0.95, n_boots=10000):
    """
    Calculate bootstrap confidence interval
    """
    boot_stats = []
    for _ in range(n_boots):
        boot_sample = np.random.choice(data, size=len(data), replace=True)
        boot_stats.append(statistic(boot_sample))
    
    lower = np.percentile(boot_stats, (1 - confidence) * 100 / 2)
    upper = np.percentile(boot_stats, (1 + confidence) * 100 / 2)
    
    return {
        'statistic': statistic(data),
        'ci_lower': lower,
        'ci_upper': upper
    }
```

## Maximum Likelihood Estimation (MLE)

### Theory
* **Principle**: Find parameter values that maximize probability of observed data
* **Log-Likelihood**: Usually maximize log(L) for computational convenience
* **Properties**:
    - Consistent
    - Asymptotically unbiased
    - Asymptotically efficient

### Implementation

#### General MLE Framework
```python
from scipy.optimize import minimize

class MLEstimator:
    def __init__(self, distribution):
        self.distribution = distribution
    
    def neg_log_likelihood(self, params, data):
        """
        Negative log-likelihood function
        """
        return -np.sum(np.log(self.distribution.pdf(data, *params)))
    
    def fit(self, data, initial_guess):
        """
        Find MLE estimates
        """
        result = minimize(self.neg_log_likelihood,
                        initial_guess,
                        args=(data,),
                        method='BFGS')
        return result.x
```

#### Example: Normal Distribution MLE
```python
def normal_mle(data):
    """
    MLE for normal distribution parameters
    """
    mu = np.mean(data)
    sigma = np.sqrt(np.mean((data - mu)**2))
    return {
        'mu': mu,
        'sigma': sigma
    }
```

## Bias-Variance Tradeoff

### Components

#### Bias
* Difference between expected prediction and true value
* Underfitting indicator
* High bias → model too simple

#### Variance
* Variability of prediction for given input
* Overfitting indicator
* High variance → model too complex

### Decomposition
```python
def bias_variance_decomp(model, X_train, X_test, y_train, y_test, n_boots=100):
    """
    Estimate bias and variance through bootstrap
    """
    predictions = np.zeros((n_boots, len(X_test)))
    
    for i in range(n_boots):
        # Bootstrap sample
        idx = np.random.choice(len(X_train), len(X_train), replace=True)
        X_boot, y_boot = X_train[idx], y_train[idx]
        
        # Train model and predict
        model.fit(X_boot, y_boot)
        predictions[i] = model.predict(X_test)
    
    # Calculate components
    expected_pred = np.mean(predictions, axis=0)
    bias = np.mean((expected_pred - y_test)**2)
    variance = np.mean(np.var(predictions, axis=0))
    
    return {
        'bias': bias,
        'variance': variance,
        'total_error': bias + variance
    }
```

### Practical Considerations

#### Model Complexity Control
```python
def complexity_analysis(model, param_range, X_train, X_test, y_train, y_test):
    """
    Analyze bias-variance tradeoff across model complexities
    """
    results = []
    for param in param_range:
        model.set_params(**{'complexity_param': param})
        decomp = bias_variance_decomp(model, X_train, X_test, 
                                    y_train, y_test)
        results.append({
            'complexity': param,
            **decomp
        })
    return pd.DataFrame(results)
```

## Best Practices

### Estimation Strategy
1. **Data Quality**
    - Check assumptions
    - Handle missing values
    - Address outliers

2. **Method Selection**
    - Consider sample size
    - Assess distribution
    - Evaluate computational resources

3. **Validation**
    - Cross-validation
    - Bootstrap when appropriate
    - Sensitivity analysis

### Reporting Guidelines
1. **Point Estimates**
    - Always include measure of uncertainty
    - Report relevant assumptions
    - Include sample size

2. **Confidence Intervals**
    - State confidence level
    - Report method used
    - Include interpretation

3. **MLE**
    - Report convergence status
    - Include standard errors
    - State optimization method

Remember:
1. Choose appropriate estimation method
2. Consider sample size implications
3. Report uncertainty measures
4. Validate assumptions
5. Document methodology clearly