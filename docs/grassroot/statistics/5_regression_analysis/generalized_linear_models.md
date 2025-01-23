# Generalized Linear Models

## General Framework

### Components of GLMs
1. **Random Component**: Response distribution from exponential family
2. **Systematic Component**: Linear predictor η = Xβ
3. **Link Function**: g(μ) = η, connecting mean to linear predictor

### Exponential Family Form
```
f(y;θ,ϕ) = exp{[yθ - b(θ)]/a(ϕ) + c(y,ϕ)}
```
where:
- θ is the natural parameter
- ϕ is the dispersion parameter
- b(θ) is the cumulant function
- a(ϕ) is typically ϕ/w (w is a known weight)

## Logistic Regression

### Model Specification
For binary response Y ∈ {0,1}:
```
logit(π) = log(π/(1-π)) = β₀ + β₁x₁ + ... + βₖxₖ
```
where π = P(Y=1|X)

### Properties
* **Response Distribution**: Bernoulli/Binomial
* **Link Function**: logit(π) = log(π/(1-π))
* **Mean Function**: E(Y|X) = π = exp(Xβ)/(1 + exp(Xβ))
* **Variance Function**: var(Y|X) = π(1-π)

### Interpretation
* **Odds**: π/(1-π)
* **Odds Ratio**: exp(βᵢ) for unit change in xᵢ
* **Probability**: π = exp(Xβ)/(1 + exp(Xβ))

### Maximum Likelihood Estimation
Log-likelihood:
```
l(β) = Σ[yᵢlog(πᵢ) + (1-yᵢ)log(1-πᵢ)]
```
Solved iteratively using Newton-Raphson or Fisher scoring

## Poisson Regression

### Model Specification
For count response Y:
```
log(μ) = β₀ + β₁x₁ + ... + βₖxₖ
```
where μ = E(Y|X)

### Properties
* **Response Distribution**: Poisson
* **Link Function**: log(μ)
* **Mean Function**: E(Y|X) = μ = exp(Xβ)
* **Variance Function**: var(Y|X) = μ

### Interpretation
* **Rate Ratio**: exp(βᵢ) for unit change in xᵢ
* **Expected Count**: exp(Xβ)
* **Percent Change**: 100(exp(βᵢ)-1)% for unit change in xᵢ

### Maximum Likelihood Estimation
Log-likelihood:
```
l(β) = Σ[yᵢlog(μᵢ) - μᵢ - log(yᵢ!)]
```

## Link Functions

### Common Link Functions
1. **Identity**: g(μ) = μ
   - Used in linear regression
   - Range: (-∞, ∞)

2. **Logit**: g(μ) = log(μ/(1-μ))
   - Used in logistic regression
   - Range: [0,1]

3. **Log**: g(μ) = log(μ)
   - Used in Poisson regression
   - Range: (0, ∞)

4. **Probit**: g(μ) = Φ⁻¹(μ)
   - Alternative for binary data
   - Range: [0,1]

### Canonical Links
For exponential family distributions:
* **Binomial**: logit
* **Poisson**: log
* **Normal**: identity
* **Gamma**: inverse

## Model Assessment

### Deviance
```
D = 2[l(y;y) - l(β̂;y)]
```
where l(y;y) is log-likelihood of saturated model

### Residuals

1. **Pearson Residuals**:
```
rₚ = (y - μ̂)/√(V(μ̂))
```

2. **Deviance Residuals**:
```
rᵈ = sign(y - μ̂)√(d)
```
where d is contribution to deviance

3. **Working Residuals**:
```
rʷ = (y - μ̂)g'(μ̂)
```

### Goodness of Fit

1. **Deviance Test**:
   - Compare deviance to χ² distribution
   - Degrees of freedom = n - p

2. **Pearson χ² Test**:
```
X² = Σ(y - μ̂)²/V(μ̂)
```

3. **AIC**:
```
AIC = -2l(β̂) + 2p
```

## Inference

### Parameter Estimation
* **Standard Errors**: √(diagonal of (X'WX)⁻¹)
* **Confidence Intervals**: β̂ᵢ ± z₁₋α/₂SE(β̂ᵢ)

### Hypothesis Testing
* **Wald Test**:
```
W = β̂/SE(β̂) ~ N(0,1)
```

* **Likelihood Ratio Test**:
```
LR = 2[l(β̂₁) - l(β̂₀)] ~ χ²(df)
```

## Model Selection

### Criteria
1. **AIC**: -2l(β̂) + 2p
2. **BIC**: -2l(β̂) + p log(n)
3. **Deviance**: Measure of model fit

### Stepwise Procedures
1. **Forward Selection**: Add terms sequentially
2. **Backward Elimination**: Remove terms sequentially
3. **Stepwise**: Combination of forward and backward

Remember:
1. Choice of link function affects interpretation
2. Check for overdispersion in count data
3. Assess model fit through residuals
4. Consider theoretical justification for model choice
5. Validate assumptions appropriate to chosen model