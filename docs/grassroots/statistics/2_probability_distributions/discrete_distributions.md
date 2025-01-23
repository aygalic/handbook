# Discrete Probability Distributions

## Bernoulli and Binomial Distributions

### Bernoulli Distribution

The Bernoulli distribution is the fundamental building block for many discrete distributions, modeling a single binary outcome. Think of it as a single coin flip or any yes/no experiment.

**Mathematical Formulation:**
Let X be a Bernoulli random variable with parameter p. Then:

P(X = x) = p^x * (1-p)^(1-x), x ∈ {0,1}

The elegance of this formulation lies in how it captures both outcomes in a single expression:
- When x = 1: P(X = 1) = p
- When x = 0: P(X = 0) = 1-p

**Key Properties:**
- E[X] = p
- Var(X) = p(1-p)
- All higher moments can be derived from p
- Moment Generating Function: M(t) = 1-p + pe^t

For practical implementation, we can use Python's built-in random module for simple cases:
```python
import random
def bernoulli_trial(p):
    return 1 if random.random() < p else 0
```

### Binomial Distribution

The binomial distribution naturally extends the Bernoulli to model the sum of n independent trials. Imagine flipping a coin n times and counting the total number of heads.

**Mathematical Formulation:**
For n trials with success probability p:

P(X = k) = (n choose k) * p^k * (1-p)^(n-k)

where (n choose k) = n!/(k!(n-k)!)

The binomial coefficient (n choose k) represents the number of ways to choose k successes from n trials, making this a beautiful combination of combinatorics and probability.

**Probability Calculation Example:**
For small values, we can compute this directly:
```python
from math import comb

def binomial_probability(n, k, p):
    return comb(n, k) * (p**k) * ((1-p)**(n-k))
```

For larger values where numerical stability is important, we should use specialized libraries:
```python
from scipy import stats
stats.binom.pmf(k, n, p)
```

## Poisson Distribution

The Poisson distribution models rare events occurring in a fixed interval. What makes it special is that we only need to know the average rate λ to describe the entire distribution.

**Mathematical Foundation:**
P(X = k) = (λ^k * e^(-λ)) / k!

This elegant formula emerges as the limit of a binomial distribution when n → ∞ and p → 0 while np = λ remains constant. This limiting relationship provides deep insight into why the Poisson distribution appears so often in nature.

**Moment Properties:**
- E[X] = λ
- Var(X) = λ
- All cumulants = λ

This equality of mean and variance is a defining characteristic that can help identify Poisson processes in real data.

## Geometric and Negative Binomial

### Geometric Distribution

The geometric distribution models the waiting time until the first success in repeated trials. Its memoryless property makes it unique among discrete distributions.

**Mathematical Insight:**
P(X = k) = p(1-p)^(k-1)

The memoryless property means:
P(X > m + n | X > m) = P(X > n)

This counterintuitive property tells us that the distribution "forgets" its past attempts.

### Negative Binomial

Generalizing the geometric distribution, the negative binomial models the number of trials until r successes. 

**Mathematical Form:**
P(X = k) = ((k-1) choose (r-1)) * p^r * (1-p)^(k-r)

This can be understood as waiting for the rth success, with k-r failures along the way. The combinatorial term accounts for all possible arrangements of these failures.

## Hypergeometric Distribution

Unlike the binomial, the hypergeometric distribution models sampling without replacement, making each draw dependent on previous draws.

**Mathematical Foundation:**

P(X = k) = [C(K,k) * C(N-K,n-k)] / C(N,n)

Where:
- N = population size
- K = success states in population
- n = sample size
- k = observed successes

The denominator C(N,n) represents all possible samples, while the numerator counts favorable outcomes through a clever application of the multiplication principle.

**Expected Value:**
E[X] = n(K/N)

This intuitive result shows that the expected proportion of successes in the sample equals the proportion in the population.

## Distribution Relationships

The relationships between these distributions reveal deep mathematical connections:

1. **Binomial and Poisson:**
When n is large and p is small:
Binomial(n,p) ≈ Poisson(np)

2. **Geometric and Negative Binomial:**
Geometric(p) = NegativeBinomial(1,p)

3. **Binomial and Hypergeometric:**
As N → ∞, Hypergeometric(N,K,n) → Binomial(n,K/N)

For computational work, these relationships often suggest efficient approximations:
```python
def approximate_large_binomial(n, p, k):
    """Use Poisson approximation for large n, small p"""
    if n > 100 and p < 0.05:
        return stats.poisson.pmf(k, n*p)
    return stats.binom.pmf(k, n, p)
```

Remember:
- The choice between mathematical and computational approaches should be guided by both theoretical considerations and practical constraints
- Understanding the mathematical foundations helps in selecting appropriate approximations
- Modern computational tools make exact calculations feasible in many cases where approximations were historically necessary
- The elegance of these distributions lies in their ability to model complex phenomena with simple parameters