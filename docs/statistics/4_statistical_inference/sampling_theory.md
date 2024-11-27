# Sampling Theory

## Population vs Sample

### Fundamental Concepts
* **Population**
    - Complete set of all possible observations
    - Parameters denoted with Greek letters (μ, σ, π)
    - Often unknown or impractical to measure completely

* **Sample**
    - Subset of population
    - Statistics denoted with Latin letters (x̄, s, p)
    - Used to make inferences about population

```python
import numpy as np
from scipy import stats

def compare_population_sample(population, sample_size):
    """
    Compare population and sample statistics
    """
    # Draw random sample
    sample = np.random.choice(population, size=sample_size, replace=False)
    
    return {
        'population_mean': np.mean(population),
        'population_std': np.std(population),
        'sample_mean': np.mean(sample),
        'sample_std': np.std(sample),
        'sample_size': sample_size
    }
```

## Sampling Distributions

### Properties
* Distribution of a statistic across all possible samples
* Key characteristics:
    - Shape
    - Center
    - Spread (standard error)

### Common Sampling Distributions

#### Sample Mean
```python
def sampling_distribution_mean(population, sample_size, n_samples=1000):
    """
    Generate sampling distribution of mean
    """
    sample_means = []
    for _ in range(n_samples):
        sample = np.random.choice(population, size=sample_size)
        sample_means.append(np.mean(sample))
        
    return {
        'means': np.array(sample_means),
        'distribution_mean': np.mean(sample_means),
        'distribution_std': np.std(sample_means),
        'theoretical_se': np.std(population) / np.sqrt(sample_size)
    }
```

#### Sample Proportion
```python
def sampling_distribution_proportion(p, sample_size, n_samples=1000):
    """
    Generate sampling distribution of proportion
    """
    sample_props = []
    for _ in range(n_samples):
        sample = np.random.binomial(1, p, size=sample_size)
        sample_props.append(np.mean(sample))
        
    return {
        'proportions': np.array(sample_props),
        'distribution_mean': np.mean(sample_props),
        'distribution_std': np.std(sample_props),
        'theoretical_se': np.sqrt(p * (1-p) / sample_size)
    }
```

## Central Limit Theorem (CLT)

### Statement
* Sample mean distribution approaches normal as n increases
* Regardless of population distribution (with finite variance)
* Mean = population mean
* Standard deviation = population standard deviation / √n

### Implementation
```python
def demonstrate_clt(population, sample_sizes, n_samples=1000):
    """
    Demonstrate CLT for different sample sizes
    """
    results = {}
    for n in sample_sizes:
        sample_means = []
        for _ in range(n_samples):
            sample = np.random.choice(population, size=n)
            sample_means.append(np.mean(sample))
            
        results[n] = {
            'means': np.array(sample_means),
            'normality_test': stats.normaltest(sample_means)
        }
    return results
```

## Law of Large Numbers (LLN)

### Weak Law (WLLN)
* Sample mean converges in probability to population mean
* As sample size increases, probability of large deviation decreases

### Strong Law (SLLN)
* Sample mean converges almost surely to population mean
* Holds for every sequence (with probability 1)

```python
def demonstrate_lln(population, max_sample_size, step_size=100):
    """
    Demonstrate Law of Large Numbers
    """
    sample_sizes = range(step_size, max_sample_size + step_size, step_size)
    means = []
    
    for n in sample_sizes:
        sample = np.random.choice(population, size=n)
        means.append(np.mean(sample))
    
    return {
        'sample_sizes': np.array(sample_sizes),
        'sample_means': np.array(means),
        'population_mean': np.mean(population)
    }
```

## Standard Error

### Concept
* Measures variability of sample statistic
* Decreases with square root of sample size
* Used in confidence intervals and hypothesis tests

### Common Standard Errors

#### Mean
```python
def standard_error_mean(data):
    """
    Calculate standard error of mean
    """
    return np.std(data, ddof=1) / np.sqrt(len(data))
```

#### Proportion
```python
def standard_error_proportion(p_hat, n):
    """
    Calculate standard error of proportion
    """
    return np.sqrt(p_hat * (1 - p_hat) / n)
```

### Bootstrap Standard Error
```python
def bootstrap_se(data, statistic, n_boots=1000):
    """
    Calculate bootstrap standard error
    """
    boot_stats = []
    for _ in range(n_boots):
        boot_sample = np.random.choice(data, size=len(data), replace=True)
        boot_stats.append(statistic(boot_sample))
    
    return np.std(boot_stats)
```

## Practical Applications

### Sample Size Determination
```python
def calculate_sample_size(confidence_level, margin_error, std_dev=None, proportion=None):
    """
    Calculate required sample size
    """
    z = stats.norm.ppf((1 + confidence_level) / 2)
    
    if std_dev is not None:  # For means
        return (z * std_dev / margin_error)**2
    elif proportion is not None:  # For proportions
        p = proportion
        return (z**2 * p * (1-p)) / margin_error**2
```

### Sampling Methods
```python
class Sampler:
    def simple_random(self, population, size):
        """Simple random sampling"""
        return np.random.choice(population, size=size, replace=False)
    
    def systematic(self, population, size):
        """Systematic sampling"""
        step = len(population) // size
        start = np.random.randint(step)
        return population[start::step][:size]
    
    def stratified(self, population, strata, sizes):
        """Stratified sampling"""
        samples = []
        for stratum, size in zip(strata, sizes):
            samples.extend(self.simple_random(stratum, size))
        return np.array(samples)
```

## Best Practices

### Sampling Considerations
1. **Representativeness**
    - Random selection
    - Adequate sample size
    - Population coverage

2. **Practical Constraints**
    - Cost and resources
    - Time limitations
    - Accessibility

3. **Analysis Requirements**
    - Desired precision
    - Type of inference
    - Assumption validity

Remember:
1. Ensure random sampling when possible
2. Consider sample size requirements
3. Check assumptions of theoretical results
4. Use appropriate sampling methods
5. Document sampling procedure