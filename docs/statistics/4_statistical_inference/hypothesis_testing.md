# Statistical Hypothesis Testing

## Fundamentals of Hypothesis Testing

### Null and Alternative Hypotheses

#### Core Concepts
* **Null Hypothesis (H₀)**
    - Default position of "no effect" or "no difference"
    - Usually contains an equality (=, ≤, ≥)
    - What we assume true until evidence suggests otherwise

* **Alternative Hypothesis (H₁ or Hₐ)**
    - Research claim we want to support
    - Usually contains an inequality (≠, >, <)
    - What we need evidence to support

#### Types of Alternative Hypotheses
1. **Two-tailed**:
    ```
    H₀: μ = μ₀
    H₁: μ ≠ μ₀
    ```

2. **One-tailed (Upper)**:
    ```
    H₀: μ ≤ μ₀
    H₁: μ > μ₀
    ```

3. **One-tailed (Lower)**:
    ```
    H₀: μ ≥ μ₀
    H₁: μ < μ₀
    ```

### Type I and Type II Errors

#### Error Types Matrix
```python
import pandas as pd

error_matrix = pd.DataFrame({
    'H₀ True': ['Correct Decision (1-α)', 'Type I Error (α)'],
    'H₀ False': ['Type II Error (β)', 'Correct Decision (1-β)']
}, index=['Fail to Reject H₀', 'Reject H₀'])
```

#### Characteristics
* **Type I Error (α)**
    - Rejecting H₀ when it's true
    - "False Positive"
    - Controlled by significance level
    - Usually set at 0.05 or 0.01

* **Type II Error (β)**
    - Failing to reject H₀ when it's false
    - "False Negative"
    - Related to sample size and effect size
    - Usually aimed to be < 0.2

## P-values and Significance Levels

### P-value Definition
* Probability of observing test statistic as extreme or more extreme than observed, assuming H₀ is true
* Smaller p-values indicate stronger evidence against H₀

### Implementation
```python
def calculate_p_value(test_statistic, dist_type='normal', **params):
    """
    Calculate p-value for different distributions
    """
    if dist_type == 'normal':
        return 2 * (1 - stats.norm.cdf(abs(test_statistic)))
    elif dist_type == 't':
        df = params.get('df', 1)
        return 2 * (1 - stats.t.cdf(abs(test_statistic), df))
    # Add more distributions as needed
```

### Significance Level (α)
* Pre-determined threshold for rejection
* Common values: 0.05, 0.01, 0.001
* Decision rule: Reject H₀ if p-value < α

## Statistical Power

### Definition
* Probability of correctly rejecting false H₀
* Power = 1 - β
* Usually aim for power ≥ 0.8

### Factors Affecting Power
1. **Sample Size**
2. **Effect Size**
3. **Significance Level**
4. **Variability**

### Power Analysis
```python
from statsmodels.stats.power import TTestPower, FTestPower

def calculate_sample_size(effect_size, power=0.8, alpha=0.05):
    """
    Calculate required sample size for t-test
    """
    analysis = TTestPower()
    sample_size = analysis.solve_power(
        effect_size=effect_size,
        power=power,
        alpha=alpha
    )
    return np.ceil(sample_size)

def calculate_power(sample_size, effect_size, alpha=0.05):
    """
    Calculate power for given parameters
    """
    analysis = TTestPower()
    power = analysis.solve_power(
        nobs=sample_size,
        effect_size=effect_size,
        alpha=alpha
    )
    return power
```

## Multiple Testing Problem

### Issue Description
* Increased probability of Type I errors when conducting multiple tests
* Family-wise error rate (FWER) = 1 - (1-α)ᵏ
* Need for correction methods

### Correction Methods

#### Bonferroni Correction
```python
def bonferroni_correction(p_values, alpha=0.05):
    """
    Apply Bonferroni correction to p-values
    """
    n_tests = len(p_values)
    adjusted_alpha = alpha / n_tests
    significant = p_values < adjusted_alpha
    return significant, adjusted_alpha
```

#### Benjamini-Hochberg (FDR)
```python
def benjamini_hochberg(p_values, alpha=0.05):
    """
    Apply Benjamini-Hochberg correction
    """
    n_tests = len(p_values)
    ranked = stats.rankdata(p_values)
    critical_values = ranked * alpha / n_tests
    significant = p_values <= critical_values
    return significant
```

### Practical Implementation

#### Complete Testing Framework
```python
class HypothesisTest:
    def __init__(self, alpha=0.05, power=0.8):
        self.alpha = alpha
        self.power = power
        
    def run_single_test(self, data, test_type='t_test'):
        """
        Run single hypothesis test
        """
        if test_type == 't_test':
            stat, p_value = stats.ttest_1samp(data, 0)
        # Add more test types
        
        return {
            'statistic': stat,
            'p_value': p_value,
            'significant': p_value < self.alpha
        }
    
    def run_multiple_tests(self, data_dict, correction='bonferroni'):
        """
        Run multiple tests with correction
        """
        p_values = []
        for key, data in data_dict.items():
            result = self.run_single_test(data)
            p_values.append(result['p_value'])
            
        if correction == 'bonferroni':
            significant, adj_alpha = bonferroni_correction(
                np.array(p_values), self.alpha
            )
        elif correction == 'fdr':
            significant = benjamini_hochberg(
                np.array(p_values), self.alpha
            )
            
        return {
            'p_values': p_values,
            'significant': significant
        }
```

## Best Practices

### Study Design
1. **A Priori Power Analysis**
    - Determine required sample size
    - Consider practical significance
    - Account for multiple testing

2. **Hypothesis Formulation**
    - Clear, specific hypotheses
    - Appropriate test selection
    - Consider directionality

### Analysis Execution
1. **Assumptions Checking**
    - Normality
    - Independence
    - Equal variances (when applicable)

2. **Documentation**
    - Pre-registered hypotheses
    - Chosen significance level
    - Power calculations
    - Multiple testing corrections

### Reporting Results
1. **Essential Elements**
    - Test statistic
    - Degrees of freedom
    - Exact p-value
    - Effect size
    - Confidence intervals

2. **Context**
    - Practical significance
    - Limitations
    - Power considerations

Remember:
1. Always state hypotheses clearly
2. Consider practical significance
3. Report effect sizes
4. Address multiple testing when applicable
5. Document all decisions and assumptions