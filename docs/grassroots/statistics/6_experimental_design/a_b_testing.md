# A/B Testing

## Sample Size Determination

### Basic Formula
For comparing two proportions:
```
n = 2(zα/2 + zβ)²[p₁(1-p₁) + p₂(1-p₂)] / (p₁-p₂)²
```
where:
- n is sample size per group
- α is significance level
- β is Type II error rate (1-power)
- p₁, p₂ are expected proportions
- zα/2, zβ are standard normal quantiles

### For Continuous Outcomes
```
n = 2(zα/2 + zβ)²σ² / δ²
```
where:
- σ² is pooled variance
- δ is minimum detectable effect

### Practical Considerations
1. **Effect Size Specification**
   * Minimum Detectable Effect (MDE)
   * Business meaningful difference
   * Historical effect sizes

2. **Power Analysis Components**
   * Baseline metrics
   * Expected variance
   * Desired power (typically 0.8 or 0.9)

## Statistical Significance

### Hypothesis Framework
```
H₀: p₁ = p₂ (or μ₁ = μ₂)
H₁: p₁ ≠ p₂ (or μ₁ ≠ μ₂)
```

### Test Statistics

1. **For Proportions (z-test)**:
```
z = (p̂₁ - p̂₂) / √[p̂(1-p̂)(1/n₁ + 1/n₂)]
```
where p̂ is pooled proportion

2. **For Means (t-test)**:
```
t = (x̄₁ - x̄₂) / √(s²₁/n₁ + s²₂/n₂)
```
with Welch's correction for unequal variances

### Sequential Analysis
For continuous monitoring:
* **O'Brien-Fleming Boundaries**:
```
z_k = C/√(k/K)
```
where:
- k is current look
- K is total looks
- C is critical value

## Effect Size

### Standardized Measures

1. **Cohen's d**:
```
d = (μ₁ - μ₂) / σₚ
```
where σₚ is pooled standard deviation

2. **Relative Difference**:
```
Δ% = (μ₁ - μ₂) / μ₂ × 100
```

3. **Risk Ratio**:
```
RR = p₁/p₂
```

### Confidence Intervals

1. **For Difference in Proportions**:
```
(p̂₁ - p̂₂) ± zα/2√[p̂₁(1-p̂₁)/n₁ + p̂₂(1-p̂₂)/n₂]
```

2. **For Difference in Means**:
```
(x̄₁ - x̄₂) ± tα/2√(s²₁/n₁ + s²₂/n₂)
```

## Multiple Testing Corrections

### Family-Wise Error Rate (FWER)

1. **Bonferroni Correction**:
```
α' = α/m
```
where m is number of tests

2. **Šidák Correction**:
```
α' = 1 - (1-α)^(1/m)
```

### False Discovery Rate (FDR)

1. **Benjamini-Hochberg Procedure**:
* Order p-values: p₁ ≤ p₂ ≤ ... ≤ pₘ
* Find largest k where:
```
pₖ ≤ (k/m)α
```

2. **Critical Value Function**:
```
α(i) = (i/m)α
```
where i is rank of p-value

### Sequential Testing
1. **Alpha Spending Function**:
```
α*(t) = α × t^ψ
```
where:
- t is information fraction
- ψ is spending parameter

2. **Error Spending Boundaries**:
```
b(t) = √[2log(log(1/t))]
```

## Practical Implementation

### Design Considerations
1. **Pre-experiment**
   * Define success metrics
   * Specify MDE
   * Calculate duration
   * Set stopping rules

2. **During Experiment**
   * Monitor implementation
   * Check randomization
   * Track sample sizes
   * Watch for validity threats

3. **Post-experiment**
   * Check assumptions
   * Apply corrections
   * Document findings
   * Make recommendations

### Common Pitfalls
1. **Statistical**
   * Peeking at results
   * Insufficient power
   * Multiple testing
   * Simpson's paradox

2. **Practical**
   * Selection bias
   * Network effects
   * Seasonality
   * Novelty effects

Remember:
1. Power analysis before testing
2. Clear success criteria
3. Appropriate corrections for multiple tests
4. Context-appropriate effect size measures
5. Document all decisions and assumptions