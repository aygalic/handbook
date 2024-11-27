# Fundamentals of Bayesian Statistics

## Bayes' Theorem

### Basic Form
```
P(θ|X) = P(X|θ)P(θ) / P(X)
```
where:
- P(θ|X) is posterior probability
- P(X|θ) is likelihood
- P(θ) is prior probability
- P(X) is marginal likelihood (normalizing constant)

### Expanded Form
```
P(X) = ∫P(X|θ)P(θ)dθ
```
or for discrete case:
```
P(X) = ΣP(X|θ)P(θ)
```

### Alternative Expression
```
Posterior ∝ Likelihood × Prior
```
avoiding computation of normalizing constant

## Prior and Posterior Distributions

### Prior Distributions

1. **Informative Priors**
   * Based on previous knowledge
   * Expert opinion
   * Historical data
   * Example: N(μ₀, σ₀²) for known mean

2. **Non-informative Priors**
   * Uniform distribution
   * Jeffreys prior: √|I(θ)|
   * Reference priors
   * Example: P(θ) ∝ 1 for location parameter

3. **Hierarchical Priors**
   * Parameters have their own priors
   * Hyperparameters
   ```
   P(θ) = ∫P(θ|η)P(η)dη
   ```

### Posterior Distributions

1. **Point Estimates**
   * Posterior mean: E[θ|X]
   * Posterior median
   * Maximum a posteriori (MAP)

2. **Interval Estimates**
   * Credible intervals
   * Highest posterior density (HPD)
   ```
   P(a ≤ θ ≤ b|X) = 1-α
   ```

3. **Posterior Predictive**
   ```
   P(X̃|X) = ∫P(X̃|θ)P(θ|X)dθ
   ```

## Conjugate Priors

### Definition
Prior and posterior from same family of distributions

### Common Conjugate Pairs

1. **Binomial-Beta**
   * Likelihood: Binomial(n,θ)
   * Prior: Beta(α,β)
   * Posterior: Beta(α+x, β+n-x)
   where x is number of successes

2. **Normal-Normal**
   * Likelihood: N(θ,σ²)
   * Prior: N(μ₀,σ₀²)
   * Posterior: N(μₙ,σₙ²)
   where:
   ```
   μₙ = (σ⁻²X̄n + σ₀⁻²μ₀)/(σ⁻²n + σ₀⁻²)
   σₙ² = 1/(σ⁻²n + σ₀⁻²)
   ```

3. **Poisson-Gamma**
   * Likelihood: Poisson(θ)
   * Prior: Gamma(α,β)
   * Posterior: Gamma(α+Σx, β+n)

## Bayesian Inference

### Parameter Estimation

1. **Point Estimation**
   ```
   θ̂ = E[θ|X] = ∫θP(θ|X)dθ
   ```

2. **Interval Estimation**
   * Equal-tailed interval:
   ```
   [θₗ,θᵤ]: P(θ < θₗ|X) = P(θ > θᵤ|X) = α/2
   ```
   * HPD interval:
   ```
   P(θ∈[θₗ,θᵤ]|X) = 1-α
   ```
   minimizing θᵤ-θₗ

### Hypothesis Testing

1. **Bayes Factor**:
```
BF₁₀ = P(X|H₁)/P(X|H₀)
```
Interpretation:
* BF₁₀ > 1: Evidence for H₁
* BF₁₀ < 1: Evidence for H₀

2. **Posterior Probability**:
```
P(H₁|X) = P(X|H₁)P(H₁)/P(X)
```

3. **Decision Theory**:
```
d* = argmin_d ∫L(d,θ)P(θ|X)dθ
```
where L is loss function

### Model Comparison

1. **Bayesian Model Averaging**:
```
P(θ|X) = ΣP(θ|M_k,X)P(M_k|X)
```

2. **DIC (Deviance Information Criterion)**:
```
DIC = D̄ + pD
```
where:
- D̄ is expected deviance
- pD is effective number of parameters

## Practical Considerations

### Prior Selection
1. **Sensitivity Analysis**
   * Multiple priors
   * Impact on conclusions
   * Robustness checks

2. **Elicitation**
   * Expert knowledge
   * Historical data
   * Meta-analysis

### Computation
1. **Analytical Solutions**
   * Conjugate priors
   * Simple models

2. **Numerical Methods**
   * MCMC
   * Variational inference
   * Laplace approximation

Remember:
1. Prior specification is crucial
2. Conjugate priors simplify computation
3. Interpretation differs from frequentist
4. Model checking is important
5. Consider computational feasibility