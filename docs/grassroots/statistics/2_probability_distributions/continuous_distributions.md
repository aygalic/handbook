# Continuous Probability Distributions

## Normal/Gaussian Distribution

The normal distribution is fundamental to statistics, arising naturally in many phenomena due to the Central Limit Theorem. Its mathematical elegance and theoretical properties make it the cornerstone of statistical inference.

### Mathematical Foundation
The probability density function is given by:

$f(x) = \frac{1}{\sigma\sqrt{2\pi}} e^{-\frac{(x-\mu)^2}{2\sigma^2}}$

where:
- μ determines the center (location)
- σ controls the spread (scale)

The standard normal distribution (μ = 0, σ = 1) simplifies this to:

$f(x) = \frac{1}{\sqrt{2\pi}} e^{-\frac{x^2}{2}}$

### Key Properties
1. **Symmetry**: The distribution is perfectly symmetric around μ
2. **Empirical Rule**:
   - μ ± σ contains ≈ 68% of data
   - μ ± 2σ contains ≈ 95% of data
   - μ ± 3σ contains ≈ 99.7% of data

### Implementation
For practical applications, we can use Python:

```python
import scipy.stats as stats

# Calculate probability between -1 and 1 standard deviations
prob = stats.norm.cdf(1) - stats.norm.cdf(-1)
print(f"Probability within 1σ: {prob:.4f}")  # ≈ 0.6827
```

## Student's t-Distribution

The t-distribution emerges when estimating the mean of a normally distributed population when the sample size is small and the population standard deviation is unknown.

### Mathematical Foundation
The probability density function is:

$f(t) = \frac{\Gamma(\frac{\nu+1}{2})}{\sqrt{\nu\pi}\,\Gamma(\frac{\nu}{2})} \left(1+\frac{t^2}{\nu}\right)^{-\frac{\nu+1}{2}}$

where ν represents the degrees of freedom.

### Intuition
Think of the t-distribution as a "more uncertain" normal distribution. As sample size increases (ν increases), we become more certain about our estimates, and the t-distribution approaches the normal distribution.

### Critical Values
For hypothesis testing, we often need critical values. The relationship between confidence levels and t-values depends on ν:

```python
def get_t_critical(confidence_level, df):
    """Calculate two-tailed critical t-value"""
    alpha = 1 - confidence_level
    return stats.t.ppf(1 - alpha/2, df)
```

## Chi-Square Distribution

The chi-square distribution represents the sum of squared standard normal variables. It's crucial for variance-related inference and categorical data analysis.

### Mathematical Foundation
For k degrees of freedom:

$f(x) = \frac{1}{2^{k/2}\Gamma(k/2)} x^{k/2-1}e^{-x/2}$

### Properties
Expected value: E(X) = k
Variance: Var(X) = 2k

The distribution becomes more symmetric as k increases, approaching normality.

### Application Example: Testing Variance
To test if a sample comes from a population with a specified variance σ²₀:

$\chi^2 = \frac{(n-1)s^2}{\sigma_0^2}$

where s² is the sample variance.

## F-Distribution

The F-distribution represents the ratio of two chi-square distributions divided by their respective degrees of freedom. It's fundamental for comparing variances and in ANOVA.

### Mathematical Formulation
For a ratio of chi-square variables with d₁ and d₂ degrees of freedom:

$f(x) = \frac{\sqrt{\frac{(d_1x)^{d_1}d_2^{d_2}}{(d_1x+d_2)^{d_1+d_2}}}}{xB(d_1/2,d_2/2)}$

where B is the beta function.

### ANOVA Application
In one-way ANOVA, the F-statistic is:

$F = \frac{\text{Between-group variability}}{\text{Within-group variability}} = \frac{MS_{\text{between}}}{MS_{\text{within}}}$

```python
def calculate_f_statistic(groups):
    """Calculate F-statistic for one-way ANOVA"""
    f_stat, p_val = stats.f_oneway(*groups)
    return f_stat, p_val
```

## Exponential and Gamma Distributions

### Exponential Distribution
Models the time between events in a Poisson process. Its memoryless property makes it unique.

Mathematical form:
$f(x) = \lambda e^{-\lambda x}$, x ≥ 0

The mean is 1/λ and variance is 1/λ².

### Gamma Distribution
Generalizes the exponential distribution. If X₁, ..., Xₖ are independent exponential(λ), their sum follows Gamma(k,λ).

PDF:
$f(x) = \frac{\lambda^k x^{k-1} e^{-\lambda x}}{\Gamma(k)}$

## Beta Distribution

The beta distribution is defined on [0,1], making it perfect for modeling probabilities and proportions.

### Mathematical Form
$f(x) = \frac{x^{\alpha-1}(1-x)^{\beta-1}}{B(\alpha,\beta)}$

### Shape Parameters
α and β control the distribution shape:
- α, β > 1: unimodal
- α = β = 1: uniform
- α < 1: J-shaped
- β < 1: reverse J-shaped

### Bayesian Application
In Bayesian inference, beta serves as a conjugate prior for binomial probability:
- Prior: Beta(α,β)
- Data: Binomial(n,p)
- Posterior: Beta(α+successes, β+failures)

```python
def update_beta_parameters(prior_alpha, prior_beta, successes, failures):
    """Update beta parameters with new data"""
    post_alpha = prior_alpha + successes
    post_beta = prior_beta + failures
    return post_alpha, post_beta
```

Remember:
1. Distribution choice should be guided by data properties and theoretical considerations
2. Many distributions are interconnected through transformations
3. Computational tools help, but understanding the mathematical foundations is crucial
4. Visual inspection and formal tests should complement each other in distribution analysis