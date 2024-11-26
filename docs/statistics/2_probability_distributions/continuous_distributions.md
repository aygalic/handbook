# Continuous Probability Distributions

## Normal/Gaussian Distribution

The normal distribution is the most widely used continuous probability distribution in statistics.

### Properties
* Probability Density Function (PDF):
    ```
    f(x) = (1/√(2πσ²)) * e^(-(x-μ)²/(2σ²))
    ```
    where:
    - μ = mean (location parameter)
    - σ = standard deviation (scale parameter)

* Characteristics:
    - Symmetric bell-shaped curve
    - Mean = Median = Mode
    - 68-95-99.7 rule:
        - 68% of data within ±1σ
        - 95% within ±2σ
        - 99.7% within ±3σ

### Applications
* Natural phenomena
* Measurement errors
* Statistical inference
* Central Limit Theorem applications

## Student's t-Distribution

Developed by William Gosset under the pseudonym "Student", this distribution is crucial for small sample inference.

### Properties
* PDF:
    ```
    f(t) = [Γ((v+1)/2)/(√(vπ)Γ(v/2))] * (1 + t²/v)^(-(v+1)/2)
    ```
    where:
    - v = degrees of freedom
    - Γ = gamma function

* Characteristics:
    - Similar to normal but heavier tails
    - Approaches normal as v → ∞
    - Symmetric around 0

### Applications
* Small sample hypothesis testing
* Confidence intervals for means
* Regression analysis

## Chi-Square Distribution

A distribution of the sum of squared standard normal variables.

### Properties
* PDF:
    ```
    f(x) = [x^(k/2-1) * e^(-x/2)] / [2^(k/2) * Γ(k/2)]
    ```
    where:
    - k = degrees of freedom
    - x ≥ 0

* Characteristics:
    - Right-skewed
    - Non-negative
    - Mean = k
    - Variance = 2k

### Applications
* Goodness-of-fit tests
* Independence tests
* Variance analysis

## F-Distribution

The ratio of two chi-square distributions divided by their respective degrees of freedom.

### Properties
* PDF:
    ```
    f(x) = [Γ((d1+d2)/2)/(Γ(d1/2)Γ(d2/2))] * (d1/d2)^(d1/2) * x^(d1/2-1) * (1 + (d1/d2)x)^(-(d1+d2)/2)
    ```
    where:
    - d1, d2 = degrees of freedom
    - x ≥ 0

* Characteristics:
    - Right-skewed
    - Non-negative
    - Shape depends on both degrees of freedom

### Applications
* ANOVA
* Regression analysis
* Variance comparisons

## Exponential and Gamma Distributions

### Exponential Distribution
* PDF:
    ```
    f(x) = λe^(-λx)
    ```
    where:
    - λ = rate parameter
    - x ≥ 0

* Characteristics:
    - Memoryless property
    - Mean = 1/λ
    - Variance = 1/λ²

#### Applications
* Time between events
* Lifetime modeling
* Reliability analysis

### Gamma Distribution
* PDF:
    ```
    f(x) = (λ^k * x^(k-1) * e^(-λx)) / Γ(k)
    ```
    where:
    - k = shape parameter
    - λ = rate parameter
    - x ≥ 0

* Characteristics:
    - Generalizes exponential distribution
    - Mean = k/λ
    - Variance = k/λ²

#### Applications
* Waiting time analysis
* Rainfall modeling
* Risk analysis

## Beta Distribution

A flexible distribution defined on the interval [0,1].

### Properties
* PDF:
    ```
    f(x) = [x^(α-1) * (1-x)^(β-1)] / B(α,β)
    ```
    where:
    - α, β = shape parameters
    - B(α,β) = beta function
    - 0 ≤ x ≤ 1

* Characteristics:
    - Highly flexible shape
    - Mean = α/(α+β)
    - Variance = αβ/((α+β)²(α+β+1))

### Applications
* Probability modeling
* Bayesian inference
* Quality control
* Success rate estimation

## Relationships and Transformations

### Key Relationships
1. Normal → Chi-square:
   - Sum of squared standard normal variables
2. Chi-square → F-distribution:
   - Ratio of independent chi-squares
3. Normal → Student's t:
   - Ratio of normal to sqrt of chi-square/df

### Common Transformations
```python
import numpy as np
from scipy import stats

# Generate normal data
normal_data = np.random.normal(loc=0, scale=1, size=1000)

# Transform to chi-square (1 df)
chi_square_data = normal_data**2

# Create F-distribution from two chi-squares
chi1 = np.random.chisquare(df=5, size=1000)
chi2 = np.random.chisquare(df=10, size=1000)
f_data = (chi1/5)/(chi2/10)
```

## Practical Considerations

### Distribution Selection
* Data characteristics
* Theoretical justification
* Sample size
* Analysis objectives

### Implementation Tips
```python
from scipy import stats
import numpy as np

# Normal distribution
normal_data = stats.norm.rvs(loc=0, scale=1, size=1000)

# Student's t
t_data = stats.t.rvs(df=10, size=1000)

# Chi-square
chi2_data = stats.chi2.rvs(df=5, size=1000)

# F-distribution
f_data = stats.f.rvs(dfn=5, dfd=10, size=1000)

# Exponential
exp_data = stats.expon.rvs(scale=1/2, size=1000)  # λ=2

# Beta
beta_data = stats.beta.rvs(a=2, b=5, size=1000)
```

### Diagnostic Tools
```python
def check_distribution(data, dist_name):
    """Basic distribution checking"""
    # QQ plot
    stats.probplot(data, dist=dist_name, plot=plt)
    plt.title(f'Q-Q Plot for {dist_name}')
    
    # Kolmogorov-Smirnov test
    ks_stat, p_value = stats.kstest(data, dist_name)
    print(f'KS test p-value: {p_value}')
```

Remember:
1. Distribution assumptions need verification
2. Sample size affects distribution choice
3. Transformations can help achieve desired properties
4. Multiple tools for distribution checking
5. Consider practical significance and context