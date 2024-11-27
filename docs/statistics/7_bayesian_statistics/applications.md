# Bayesian Statistical Applications

## Bayesian A/B Testing

### Model Formulation

1. **Binomial Model**
For conversion rates:
```
Group A: yₐ ~ Binomial(nₐ, θₐ)
Group B: yᵦ ~ Binomial(nᵦ, θᵦ)
Priors: θₐ, θᵦ ~ Beta(α, β)
```

2. **Normal Model**
For continuous metrics:
```
Group A: yₐ ~ N(μₐ, σ²)
Group B: yᵦ ~ N(μᵦ, σ²)
Priors: μₐ, μᵦ ~ N(μ₀, σ₀²)
        σ² ~ InvGamma(α, β)
```

### Inference

1. **Probability of Improvement**
```
P(θᵦ > θₐ|data) = ∫∫I(θᵦ > θₐ)p(θₐ,θᵦ|data)dθₐdθᵦ
```

2. **Expected Lift**
```
E[θᵦ - θₐ|data] = E[θᵦ|data] - E[θₐ|data]
```

3. **Risk Assessment**
```
P(θᵦ - θₐ > δ|data)  # Probability of meaningful difference
```

## Bayesian Regression

### Linear Regression Model
```
y = Xβ + ε
ε ~ N(0, σ²I)
```

### Prior Specifications

1. **Coefficients**
```
β ~ N(μ₀, Σ₀)  # Multivariate normal prior
```

2. **Variance**
```
σ² ~ InvGamma(α, β)  # Inverse gamma prior
```

3. **Joint Posterior**
```
p(β,σ²|y) ∝ p(y|β,σ²)p(β)p(σ²)
```

### Posterior Inference

1. **Parameter Estimation**
```
E[β|y] = (X'X + σ²Σ₀⁻¹)⁻¹(X'y + σ²Σ₀⁻¹μ₀)
Var(β|y) = σ²(X'X + σ²Σ₀⁻¹)⁻¹
```

2. **Prediction**
```
p(ỹ|x̃,y) = ∫∫p(ỹ|x̃,β,σ²)p(β,σ²|y)dβdσ²
```

## Hierarchical Models

### General Structure
```
Level 1 (Data): y_i ~ p(y|θᵢ)
Level 2 (Parameters): θᵢ ~ p(θ|η)
Level 3 (Hyperparameters): η ~ p(η)
```

### Hierarchical Linear Model

1. **Model Specification**
```
yᵢⱼ = β₀ⱼ + β₁ⱼxᵢⱼ + εᵢⱼ
β₀ⱼ = γ₀₀ + γ₀₁wⱼ + u₀ⱼ
β₁ⱼ = γ₁₀ + γ₁₁wⱼ + u₁ⱼ
```

2. **Distribution Assumptions**
```
εᵢⱼ ~ N(0, σ²)
[u₀ⱼ, u₁ⱼ]' ~ N(0, Σ)
```

3. **Prior Specifications**
```
γ ~ N(μγ, Σγ)
σ² ~ InvGamma(α₁, β₁)
Σ ~ InvWishart(ν, S)
```

### Hierarchical Logistic Regression

1. **Model Structure**
```
yᵢⱼ ~ Bernoulli(pᵢⱼ)
logit(pᵢⱼ) = β₀ⱼ + β₁ⱼxᵢⱼ
β₀ⱼ ~ N(μ₀, τ₀²)
β₁ⱼ ~ N(μ₁, τ₁²)
```

2. **Hyperpriors**
```
μ₀,μ₁ ~ N(0, σ²)
τ₀²,τ₁² ~ InvGamma(α, β)
```

### Implementation Considerations

1. **Model Building**
* Start simple
* Add complexity gradually
* Check convergence
* Assess model fit

2. **Prior Selection**
* Weakly informative
* Domain knowledge
* Sensitivity analysis

3. **Computational Methods**
* MCMC (Gibbs, Metropolis-Hastings)
* Hamiltonian Monte Carlo
* Variational inference

### Diagnostics and Model Checking

1. **MCMC Diagnostics**
```
R̂ (Gelman-Rubin statistic)
Effective sample size
Trace plots
Autocorrelation
```

2. **Posterior Predictive Checks**
```
yrep ~ p(y|θ)  # Simulated data
T(yrep) vs T(y)  # Test statistics comparison
```

3. **Model Comparison**
```
WAIC = -2(lppd - pWAIC)
DIC = D̄ + pD
LOO-CV = Σᵢlog p(yᵢ|y₋ᵢ)
```

Remember:
1. Model complexity matches data structure
2. Check convergence and mixing
3. Validate assumptions
4. Consider computational efficiency
5. Use appropriate diagnostics