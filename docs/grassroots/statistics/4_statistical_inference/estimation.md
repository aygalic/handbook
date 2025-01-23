# Statistical Estimation

## Point Estimates

A point estimate is a single value that serves as our "best guess" for an unknown population parameter. The theory behind point estimation helps us understand what makes a good estimator.

### Properties of Estimators

#### Unbiasedness
An estimator θ̂ is unbiased if its expected value equals the true parameter:

E[θ̂] = θ

For example, the sample mean X̄ is an unbiased estimator of the population mean μ:

E[X̄] = E[∑Xᵢ/n] = ∑E[Xᵢ]/n = μ

#### Consistency
An estimator is consistent if it converges to the true parameter as sample size increases:

lim(n→∞) P(|θ̂ₙ - θ| > ε) = 0 for any ε > 0

#### Efficiency
Among unbiased estimators, the most efficient one has the smallest variance. The Cramér-Rao bound gives us the theoretical minimum variance:

Var(θ̂) ≥ 1/I(θ)

where I(θ) is the Fisher Information.

### Implementation of Basic Estimators
```python
def calculate_estimators(data):
    """Calculate common point estimates with their standard errors"""
    n = len(data)
    mean = np.mean(data)
    variance = np.var(data, ddof=1)  # Using n-1 for unbiased estimation
    
    return {
        'mean': mean,
        'se_mean': np.sqrt(variance/n),  # Standard error of mean
        'variance': variance,
        'se_variance': variance * np.sqrt(2/(n-1))  # SE of variance
    }
```

## Confidence Intervals

A confidence interval provides a range of plausible values for a parameter, along with a measure of uncertainty. The mathematical foundation comes from the sampling distribution of the estimator.

### For Population Mean
Under normality assumption, the pivotal quantity:

(X̄ - μ)/(s/√n) ~ t(n-1)

leads to the confidence interval:

X̄ ± t(α/2,n-1) * s/√n

where:
- t(α/2,n-1) is the critical value from t-distribution
- s is the sample standard deviation
- n is the sample size

```python
def mean_confidence_interval(data, confidence=0.95):
    """Calculate CI for mean using t-distribution"""
    n = len(data)
    mean = np.mean(data)
    se = stats.sem(data)
    ci = stats.t.interval(confidence, df=n-1, loc=mean, scale=se)
    return mean, ci
```

## Maximum Likelihood Estimation (MLE)

MLE finds parameter values that maximize the probability of observing the data. For independent observations, the likelihood function is:

L(θ; x₁, ..., xₙ) = ∏ᵢ f(xᵢ; θ)

We typically maximize the log-likelihood:

ℓ(θ) = ∑ᵢ log f(xᵢ; θ)

### Example: Normal Distribution
For normal distribution, the log-likelihood is:

ℓ(μ,σ²) = -n/2 * log(2πσ²) - ∑(xᵢ - μ)²/(2σ²)

The MLEs are:
μ̂ = X̄
σ̂² = ∑(xᵢ - X̄)²/n

```python
def normal_mle_with_uncertainty(data):
    """MLE for normal distribution with standard errors"""
    n = len(data)
    mu = np.mean(data)
    sigma2 = np.sum((data - mu)**2)/n  # MLE of variance
    
    # Fisher Information Matrix derivatives
    se_mu = np.sqrt(sigma2/n)
    se_sigma = np.sqrt(sigma2/(2*n))
    
    return {
        'mu': mu, 
        'sigma': np.sqrt(sigma2),
        'se_mu': se_mu,
        'se_sigma': se_sigma
    }
```

## Bias-Variance Tradeoff

The expected prediction error can be decomposed into:

E[(y - f̂(x))²] = (Bias[f̂(x)])² + Var[f̂(x)] + σ²

where:
- Bias[f̂(x)] = E[f̂(x)] - f(x)
- Var[f̂(x)] = E[(f̂(x) - E[f̂(x)])²]
- σ² is irreducible error

This decomposition helps understand the fundamental tradeoff in model complexity:
- Simple models: High bias, low variance
- Complex models: Low bias, high variance

### Visual Demonstration
```python
def plot_bias_variance_tradeoff(complexity_range, bias_values, variance_values):
    """Plot bias-variance tradeoff across model complexities"""
    plt.figure(figsize=(10, 6))
    plt.plot(complexity_range, bias_values, label='Bias²')
    plt.plot(complexity_range, variance_values, label='Variance')
    plt.plot(complexity_range, 
            np.array(bias_values) + np.array(variance_values), 
            label='Total Error')
    plt.xlabel('Model Complexity')
    plt.ylabel('Error')
    plt.legend()
    plt.title('Bias-Variance Tradeoff')
    return plt
```

## Key Takeaways

1. **Point Estimation**
   - Balance between different estimator properties
   - Consider both theoretical properties and practical constraints

2. **Interval Estimation**
   - Provides measure of uncertainty
   - Based on sampling distribution theory
   - Requires careful interpretation

3. **Maximum Likelihood**
   - Powerful, general-purpose method
   - Asymptotically optimal properties
   - Can be computationally intensive

4. **Bias-Variance**
   - Fundamental tradeoff in statistical learning
   - Guides model complexity selection
   - Helps understand overfitting/underfitting

Remember:
- Theory guides the choice of methods
- Practical considerations often require compromises
- Understanding uncertainty is crucial
- Multiple approaches often provide better insight