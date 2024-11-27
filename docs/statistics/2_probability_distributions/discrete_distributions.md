# Discrete Probability Distributions

## Bernoulli and Binomial Distributions

### Bernoulli Distribution
Models a single binary outcome (success/failure).

* **Probability Mass Function (PMF)**:
    ```
    P(X = x) = p^x * (1-p)^(1-x), x ∈ {0,1}
    ```
    where:
    - p = probability of success
    - x = outcome (0 or 1)

* **Properties**:
    - Mean (μ) = p
    - Variance (σ²) = p(1-p)
    - Only two possible outcomes

* **Applications**:
    - Coin flips
    - Yes/no questions
    - Pass/fail outcomes

### Binomial Distribution
Models the number of successes in n independent Bernoulli trials.

* **PMF**:
    ```
    P(X = k) = C(n,k) * p^k * (1-p)^(n-k)
    ```
    where:
    - n = number of trials
    - k = number of successes
    - p = probability of success
    - C(n,k) = binomial coefficient

* **Properties**:
    - Mean = np
    - Variance = np(1-p)
    - Sum of Bernoulli trials

```python
from scipy import stats
import numpy as np

# Generate binomial data
n_trials = 100
p_success = 0.3
binomial_data = stats.binom.rvs(n=n_trials, p=p_success, size=1000)

# Calculate probability of exactly k successes
k = 25
prob_k = stats.binom.pmf(k, n_trials, p_success)
```

## Poisson Distribution
Models the number of rare events in a fixed interval.

### Properties
* **PMF**:
    ```
    P(X = k) = (λ^k * e^(-λ)) / k!
    ```
    where:
    - λ = average rate of events
    - k = number of events

* **Characteristics**:
    - Mean = λ
    - Variance = λ
    - Events occur independently
    - Rate remains constant

### Applications
* Customer arrivals
* Website traffic
* Defects in manufacturing
* Rare disease occurrences

```python
# Generate Poisson data
lambda_rate = 3
poisson_data = stats.poisson.rvs(mu=lambda_rate, size=1000)

# Calculate probability of exactly k events
k = 2
prob_k = stats.poisson.pmf(k, lambda_rate)
```

## Geometric and Negative Binomial Distributions

### Geometric Distribution
Models the number of trials until first success.

* **PMF**:
    ```
    P(X = k) = p * (1-p)^(k-1)
    ```
    where:
    - p = success probability
    - k = number of trials until success

* **Properties**:
    - Mean = 1/p
    - Variance = (1-p)/p²
    - Memoryless property

### Negative Binomial Distribution
Models the number of trials until r successes.

* **PMF**:
    ```
    P(X = k) = C(k-1,r-1) * p^r * (1-p)^(k-r)
    ```
    where:
    - r = number of successes needed
    - k = number of trials
    - p = success probability

* **Properties**:
    - Mean = r/p
    - Variance = r(1-p)/p²
    - Generalizes geometric distribution

```python
# Geometric distribution
p_success = 0.2
geometric_data = stats.geom.rvs(p=p_success, size=1000)

# Negative binomial
r_successes = 3
neg_binom_data = stats.nbinom.rvs(n=r_successes, p=p_success, size=1000)
```

## Hypergeometric Distribution
Models sampling without replacement from a finite population.

### Properties
* **PMF**:
    ```
    P(X = k) = [C(K,k) * C(N-K,n-k)] / C(N,n)
    ```
    where:
    - N = population size
    - K = number of successes in population
    - n = sample size
    - k = number of observed successes

* **Characteristics**:
    - Mean = n(K/N)
    - Variance = n(K/N)(1-K/N)((N-n)/(N-1))
    - Differs from binomial due to dependency

### Applications
* Quality control sampling
* Election auditing
* Card drawing problems
* Population sampling

```python
# Hypergeometric distribution
N_population = 100  # Total population
K_successes = 20   # Success states in population
n_sample = 10      # Sample size
hypergeom_data = stats.hypergeom.rvs(M=N_population, n=K_successes, 
                                    N=n_sample, size=1000)
```

## Relationships and Approximations

### Poisson Approximation to Binomial
When n is large and p is small:
```python
def poisson_approx_binomial(n, p, k):
    lambda_param = n * p
    return stats.poisson.pmf(k, lambda_param)
```

### Normal Approximation to Binomial
When np(1-p) > 10:
```python
def normal_approx_binomial(n, p, k):
    mu = n * p
    sigma = np.sqrt(n * p * (1-p))
    return stats.norm.cdf(k + 0.5, mu, sigma) - stats.norm.cdf(k - 0.5, mu, sigma)
```

## Implementation and Testing

### Distribution Fitting
```python
def fit_discrete_distribution(data, distribution='poisson'):
    """Fit discrete distribution to data"""
    if distribution == 'poisson':
        lambda_mle = np.mean(data)
        return {'lambda': lambda_mle}
    elif distribution == 'geometric':
        p_mle = 1 / np.mean(data)
        return {'p': p_mle}
    # Add more distributions as needed
```

### Goodness of Fit Tests
```python
def test_discrete_fit(data, distribution, params):
    """Chi-square goodness of fit test"""
    observed = np.bincount(data)
    if distribution == 'poisson':
        expected = stats.poisson.pmf(np.arange(len(observed)), 
                                   params['lambda']) * len(data)
    # Add more distributions as needed
    chi2_stat, p_value = stats.chisquare(observed, expected)
    return chi2_stat, p_value
```

## Practical Guidelines

### Distribution Selection
1. **Consider the Data Generation Process**:
   * Binary outcomes → Bernoulli/Binomial
   * Rare events → Poisson
   * Time until success → Geometric
   * Sampling without replacement → Hypergeometric

2. **Sample Size Considerations**:
   * Large samples may allow normal approximations
   * Small samples require exact distributions

3. **Assumptions Checking**:
   * Independence
   * Constant probability/rate
   * Population size (finite/infinite)

Remember:
1. Verify distribution assumptions
2. Consider approximations for computational efficiency
3. Use appropriate goodness-of-fit tests
4. Account for real-world constraints
5. Document distribution choice rationale