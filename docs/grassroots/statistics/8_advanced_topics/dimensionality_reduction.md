# Dimensionality Reduction

## Principal Component Analysis (PCA)

### Mathematical Foundation

1. **Objective**
* Find orthogonal directions maximizing variance
* Linear transformation of data
* Minimize reconstruction error

2. **Formulation**
```
X = UΣV'
```
where:
- X is centered data matrix (n × p)
- U is left singular vectors (n × p)
- Σ is diagonal matrix of singular values
- V is right singular vectors (p × p)

### Properties

1. **Principal Components**
* First PC: w₁ = argmax ||w||=1 Var(Xw)
* Subsequent PCs: orthogonal to previous
* Loading vector: eigenvectors of X'X
* Scores: Xw

2. **Variance Explained**
```
λᵢ/Σλᵢ
```
where λᵢ are eigenvalues of covariance matrix

### Implementation Steps

1. **Data Preprocessing**
* Center: X̃ = X - μ
* (Optional) Scale: X̃ = (X - μ)/σ

2. **Computation**
* Covariance matrix: S = X̃'X̃/n
* Eigendecomposition: S = VΛV'
* PC scores: Z = X̃V

## Factor Analysis

### Model Specification

1. **Basic Model**
```
X = ΛF + ε
```
where:
- X is p-dimensional observed variables
- Λ is p × k loading matrix
- F is k-dimensional factors
- ε is unique factors

2. **Assumptions**
```
F ~ N(0, I)
ε ~ N(0, Ψ)
```
where Ψ is diagonal

### Estimation Methods

1. **Maximum Likelihood**
* Iterative procedure
* Likelihood:
```
L(Λ,Ψ|X) = -½[log|ΛΛ'+Ψ| + tr((ΛΛ'+Ψ)⁻¹S)]
```

2. **Principal Factor**
* Initial estimate: Ψ = diag(1 - h²)
* h² are communalities
* Iterate until convergence

### Factor Rotation

1. **Orthogonal Rotation**
* Varimax: maximize variance of squared loadings
* Quartimax: simplify variables
* Equamax: compromise between varimax and quartimax

2. **Oblique Rotation**
* Promax: start with varimax, allow correlation
* Direct oblimin: minimize cross-products of loadings

## t-SNE

### Algorithm

1. **Similarity Computation**
* High-dimensional similarities:
```
pⱼ|ᵢ = exp(-||xᵢ-xⱼ||²/2σᵢ²)/Σₖexp(-||xᵢ-xₖ||²/2σᵢ²)
pᵢⱼ = (pⱼ|ᵢ + pᵢ|ⱼ)/2n
```

2. **Low-dimensional Similarities**
```
qᵢⱼ = (1 + ||yᵢ-yⱼ||²)⁻¹/Σₖ,ₗ(1 + ||yₖ-yₗ||²)⁻¹
```

3. **Objective Function**
```
KL(P||Q) = ΣᵢΣⱼpᵢⱼlog(pᵢⱼ/qᵢⱼ)
```

### Implementation Details

1. **Perplexity**
* Controls effective number of neighbors
* Typically between 5 and 50
* Adaptive σᵢ selection

2. **Optimization**
* Gradient descent with momentum
* Early exaggeration
* Learning rate annealing

### Advantages/Limitations

1. **Advantages**
* Preserves local structure
* Handles non-linear relationships
* Reveals clusters

2. **Limitations**
* Non-parametric (no out-of-sample extension)
* Computationally intensive
* Non-convex optimization

## Comparison and Selection

### Method Selection Criteria

1. **Data Characteristics**
* Sample size
* Dimensionality
* Linear vs non-linear relationships
* Sparsity

2. **Objectives**
* Visualization
* Feature extraction
* Data compression
* Structure discovery

### Performance Metrics

1. **Reconstruction Error**
```
||X - X̂||²
```

2. **Explained Variance**
```
R² = 1 - SS_res/SS_tot
```

3. **Structure Preservation**
* Trustworthiness
* Continuity
* Local structure preservation

## Best Practices

### Implementation Guidelines

1. **Data Preprocessing**
* Scaling/standardization
* Missing value handling
* Outlier detection

2. **Dimensionality Selection**
* Scree plot (PCA)
* Parallel analysis (FA)
* Perplexity tuning (t-SNE)

3. **Validation**
* Cross-validation
* Stability analysis
* Visual inspection

Remember:
1. Choose method based on objectives
2. Consider computational resources
3. Validate results
4. Understand assumptions
5. Document decisions