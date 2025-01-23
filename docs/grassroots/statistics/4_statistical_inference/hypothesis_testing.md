# Statistical Hypothesis Testing

## Fundamental Concepts

### Null and Alternative Hypotheses

The foundation of hypothesis testing lies in formulating two competing claims about a population parameter:

1. **Null Hypothesis (H₀)**: The default position, typically expressing "no effect" or "no difference"
2. **Alternative Hypothesis (H₁ or Hₐ)**: The research claim we want to support with evidence

For a population parameter θ, these are typically expressed in one of three ways:

**Two-sided test:**
```
H₀: θ = θ₀
H₁: θ ≠ θ₀
```

**One-sided tests:**
```
Upper-tailed:          Lower-tailed:
H₀: θ ≤ θ₀            H₀: θ ≥ θ₀
H₁: θ > θ₀            H₁: θ < θ₀
```

### Test Statistics and Sampling Distributions

Most test statistics follow the general form:

```
test statistic = (sample estimate - null value) / (standard error)
```

For example, the z-test statistic for a population mean:

```
z = (x̄ - μ₀) / (σ/√n)
```

where:
- x̄ is the sample mean
- μ₀ is the hypothesized population mean
- σ is the population standard deviation
- n is the sample size

When σ is unknown and estimated by s, we use the t-statistic:

```
t = (x̄ - μ₀) / (s/√n)
```

## Type I and Type II Errors

The decision process in hypothesis testing can lead to two types of errors:

|                    | H₀ True            | H₀ False           |
|--------------------|-------------------|-------------------|
| Reject H₀         | Type I Error (α)   | Correct Decision  |
| Fail to Reject H₀ | Correct Decision   | Type II Error (β) |

The probability relationships:
```
P(Type I Error) = α = P(Reject H₀ | H₀ true)
P(Type II Error) = β = P(Fail to Reject H₀ | H₁ true)
Power = 1 - β = P(Reject H₀ | H₁ true)
```

## P-values and Statistical Power

### P-value
The p-value is defined mathematically as:

For a test statistic T and observed value t*:
```
Two-sided: p = 2 * P(T ≥ |t*| | H₀)
Upper-tailed: p = P(T ≥ t* | H₀)
Lower-tailed: p = P(T ≤ t* | H₀)
```

For practical computation in Python:
```python
def calculate_p_value(test_statistic, distribution='normal', two_sided=True):
    if distribution == 'normal':
        if two_sided:
            return 2 * (1 - stats.norm.cdf(abs(test_statistic)))
        return 1 - stats.norm.cdf(test_statistic)
```

### Statistical Power
Power depends on four interrelated quantities:
1. Effect size (δ)
2. Sample size (n)
3. Significance level (α)
4. Power (1-β)

For a two-sided z-test, the power function is:

```
Power = 1 - β = Φ(zα/2 + δ√n/σ) + Φ(-zα/2 + δ√n/σ)
```

where:
- Φ is the standard normal CDF
- zα/2 is the critical value
- δ is the true difference from null
- σ is the population standard deviation

## Multiple Testing Problem

When conducting m independent tests at significance level α, the probability of at least one Type I error (Family-Wise Error Rate, FWER) is:

```
FWER = 1 - (1-α)ᵐ
```

### Bonferroni Correction
Controls FWER by adjusting the significance level:
```
α_adjusted = α/m
```

### Benjamini-Hochberg Procedure
Controls False Discovery Rate (FDR). For ordered p-values p₁ ≤ p₂ ≤ ... ≤ pₘ:
1. Find largest k where p_k ≤ (k/m)α
2. Reject all hypotheses H₍ᵢ₎ for i = 1,...,k

Implementation combining mathematical insight with computation:
```python
def benjamini_hochberg(p_values, alpha=0.05):
    """
    Implements Benjamini-Hochberg procedure
    Returns: boolean array of rejected null hypotheses
    """
    m = len(p_values)
    ranked = stats.rankdata(p_values, method='min')
    critical_values = (ranked / m) * alpha
    sorted_p_values = np.sort(p_values)
    
    # Find largest k where p_k ≤ (k/m)α
    significant = p_values <= critical_values
    return significant
```

## Power Analysis Example
Let's combine mathematical formulation with computation for a t-test power analysis:

```python
def power_analysis(effect_size, n, alpha=0.05, two_sided=True):
    """
    Calculate power for two-sample t-test
    
    Parameters:
    effect_size (d) = (μ₁ - μ₂)/σ
    n = sample size per group
    """
    # Critical value
    df = 2*n - 2  # degrees of freedom
    if two_sided:
        t_crit = stats.t.ppf(1 - alpha/2, df)
    else:
        t_crit = stats.t.ppf(1 - alpha, df)
    
    # Non-centrality parameter
    ncp = effect_size * np.sqrt(n/2)
    
    # Power calculation
    if two_sided:
        power = (1 - stats.nct.cdf(t_crit, df, ncp) + 
                stats.nct.cdf(-t_crit, df, ncp))
    else:
        power = 1 - stats.nct.cdf(t_crit, df, ncp)
    
    return power
```

This balanced approach shows both the mathematical foundation and its practical implementation, helping users understand both the theory and application of hypothesis testing.

Remember:
1. Start with clear mathematical formulation
2. Provide intuitive explanations
3. Implement solutions efficiently
4. Consider computational aspects
5. Document assumptions and limitations