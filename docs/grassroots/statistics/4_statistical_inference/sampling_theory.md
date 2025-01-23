# Sampling Theory 

## Population vs Sample

The foundation of statistical inference lies in the relationship between populations and samples:

* **Population**: The complete set of all elements we want to study
* **Sample**: A subset of the population used to make inferences

### Mathematical Notation
* Population parameters: θ, μ, σ, π
* Sample statistics: θ̂, x̄, s, p̂

The relationship between population parameters and sample statistics can be expressed through expected values:
* E[x̄] = μ (unbiased estimator of mean)
* E[s²] = σ² (unbiased estimator of variance)

## Sampling Distributions

The sampling distribution describes the probability distribution of a statistic across all possible samples of size n.

### Sample Mean
For a random sample X₁, X₂, ..., Xₙ:
* Sample mean: x̄ = (1/n)∑Xᵢ
* Expected value: E[x̄] = μ
* Variance: Var(x̄) = σ²/n

```python
# Implementation for empirical sampling distribution
def sample_mean_distribution(population, n_samples, sample_size):
    means = [np.mean(np.random.choice(population, sample_size)) 
             for _ in range(n_samples)]
    return np.array(means)
```

## Central Limit Theorem (CLT)

### Mathematical Formulation
For independent, identically distributed random variables X₁, X₂, ..., Xₙ with mean μ and variance σ², the standardized sample mean:

Z = (x̄ - μ)/(σ/√n) → N(0,1) as n → ∞

This means that for large n:
x̄ ∼ N(μ, σ²/n)

### Intuition
The CLT tells us that regardless of the underlying distribution:
1. The sampling distribution of the mean becomes approximately normal
2. The spread of this distribution shrinks with √n
3. The center remains at the population mean

## Law of Large Numbers (LLN)

### Weak Law (WLLN)
For any ε > 0:
P(|x̄ₙ - μ| > ε) → 0 as n → ∞

### Strong Law (SLLN)
P(limₙ→∞ x̄ₙ = μ) = 1

### Intuition
* WLLN: The probability of a large deviation from μ becomes small
* SLLN: The sample mean will converge to μ with probability 1

## Standard Error

### Mathematical Definition
For any statistic θ̂:
SE(θ̂) = √Var(θ̂)

### Common Forms
1. **Mean**: SE(x̄) = σ/√n
2. **Proportion**: SE(p̂) = √(p(1-p)/n)
3. **Difference of Means**: SE(x̄₁ - x̄₂) = √(σ₁²/n₁ + σ₂²/n₂)

When population parameters are unknown, we use sample estimates:
s/√n, √(p̂(1-p̂)/n), etc.

## Practical Sampling Methods

### Simple Random Sampling
Each subset of size n has equal probability of selection:
P(selecting specific sample) = 1/₍ₙᴺ₎

```python
def simple_random_sample(population, size):
    return np.random.choice(population, size=size, replace=False)
```

### Stratified Sampling
For L strata with Nₕ units in stratum h:
* Stratum weight: Wₕ = Nₕ/N
* Stratified mean: x̄ₛₜ = ∑Wₕx̄ₕ
* Variance: Var(x̄ₛₜ) = ∑Wₕ²σₕ²/nₕ

### Sample Size Determination

For desired margin of error E and confidence level α:
* For means: n = (zα/2 × σ/E)²
* For proportions: n = (zα/2)² × p(1-p)/E²

## Relationship to Statistical Inference

The theoretical foundations of sampling connect directly to inference through:

1. **Confidence Intervals**
θ̂ ± zα/2 × SE(θ̂)

2. **Hypothesis Tests**
Test statistic = (θ̂ - θ₀)/SE(θ̂)

These applications rely on the sampling distribution theory developed above.

### Example: One-Sample t-test
Under H₀: μ = μ₀
t = (x̄ - μ₀)/(s/√n) ∼ t(n-1)

```python
def t_test_statistic(sample, null_mean):
    return (np.mean(sample) - null_mean)/(np.std(sample, ddof=1)/np.sqrt(len(sample)))
```

Remember:
1. The mathematical theory provides the foundation
2. Computational methods help verify and visualize
3. Understanding both perspectives enhances statistical practice
4. Real-world applications often require both theoretical and practical tools