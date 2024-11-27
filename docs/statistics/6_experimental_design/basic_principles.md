# Basic Principles of Experimental Design

## Randomization

### Principle
Randomization is the random assignment of experimental units to treatments, ensuring:
- Each unit has equal probability of receiving any treatment
- Statistical independence of observations
- Validity of statistical inference

### Mathematical Foundation
For n experimental units and k treatments:
* Probability of assignment = 1/k for each treatment
* Number of possible assignments = n!/(n₁!n₂!...nₖ!)
where nᵢ is the number of units assigned to treatment i

### Key Benefits
1. Controls for unknown confounding variables
2. Reduces selection bias
3. Allows valid statistical inference
4. Balances uncontrolled variables

## Replication

### Definition
True replication involves independent observations under the same treatment conditions.

### Types of Replication
1. **True Replication**
   * Independent experimental units
   * Same treatment conditions
   * Independent measurements

2. **Pseudo-replication**
   * Multiple measurements on same unit
   * Not true independent observations
   * Limited statistical validity

### Statistical Importance
* Enables estimation of experimental error
* Improves precision of estimates
* Sample size determination:
```
n = 2(zα/2 + zβ)²σ²/δ²
```
where:
- σ² is variance
- δ is minimum detectable difference
- α is Type I error rate
- β is Type II error rate

## Blocking

### Principle
Grouping experimental units into homogeneous blocks to:
* Reduce known sources of variation
* Increase precision of treatment comparisons
* Control for nuisance factors

### Mathematical Model
For randomized complete block design:
```
Yᵢⱼ = μ + τᵢ + βⱼ + εᵢⱼ
```
where:
- Yᵢⱼ is response for treatment i in block j
- μ is overall mean
- τᵢ is treatment effect
- βⱼ is block effect
- εᵢⱼ is random error

### Efficiency
* Relative efficiency vs completely randomized design:
```
RE = (EMS₁/EMS₂)(df₂/df₁)
```
where EMS is error mean square

### Types of Blocks
1. **Physical Blocks**
   * Spatial grouping
   * Environmental conditions
   * Time periods

2. **Statistical Blocks**
   * Covariates
   * Matched pairs
   * Repeated measures

## Factorial Designs

### Structure
* Multiple factors studied simultaneously
* All possible combinations of factor levels
* Enables study of interactions

### Mathematical Model
For two-factor factorial:
```
Yᵢⱼₖ = μ + αᵢ + βⱼ + (αβ)ᵢⱼ + εᵢⱼₖ
```
where:
- αᵢ is effect of factor A
- βⱼ is effect of factor B
- (αβ)ᵢⱼ is interaction effect
- εᵢⱼₖ is random error

### Properties
1. **Main Effects**
   * Average effect of factor across levels of other factors
   * Marginal means comparison

2. **Interactions**
   * Non-additive effects
   * Factor effects depend on levels of other factors

### Design Efficiency
* Number of runs = product of factor levels
* Degrees of freedom partition:
```
Total df = (a×b×r) - 1
```
where:
- a is levels of factor A
- b is levels of factor B
- r is replications

## Design Considerations

### 1. Treatment Structure
* Number of treatments
* Factor levels
* Interactions of interest

### 2. Experimental Unit
* Definition and size
* Independence
* Homogeneity

### 3. Design Structure
* Randomization restrictions
* Block size and number
* Resource constraints

### 4. Sample Size
* Power considerations
* Resource limitations
* Practical constraints

## Analysis Principles

### 1. ANOVA Decomposition
```
SS_Total = SS_Treatment + SS_Block + SS_Error
```

### 2. Error Structure
* Independent errors
* Constant variance
* Normal distribution

### 3. Testing Hierarchy
1. Interaction tests
2. Main effects (if interactions non-significant)
3. Simple effects (if interactions significant)

Remember:
1. Randomization provides validity
2. Replication provides precision
3. Blocking increases efficiency
4. Factorial designs study interactions
5. Design choice affects analysis options