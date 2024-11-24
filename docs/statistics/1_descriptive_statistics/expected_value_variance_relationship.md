#  Expected Value - Variance Relationship

### 1. Basic Definitions

##### Discrete Random Variables
For a discrete random variable X:
$E[X] = \sum_{x} x \cdot P(X = x)$

##### Continuous Random Variables
For a continuous random variable X:
$E[X] = \int_{-\infty}^{\infty} x \cdot f(x) dx$

where f(x) is the probability density function.

##### Alternative Notation
- $E[X]$
- $\mu$
- $\mu_X$
- $\langle X \rangle$

### 2. Fundamental Properties

##### Linearity of Expectation
1. $E[aX + b] = aE[X] + b$
2. $E[X + Y] = E[X] + E[Y]$

###### Proof for Discrete Case:
```
E[aX + b] = ∑(ax + b)P(X = x)
          = a∑xP(X = x) + b∑P(X = x)
          = aE[X] + b    (since ∑P(X = x) = 1)
```

##### Law of Total Expectation
$E[X] = E[E[X|Y]]$

###### Proof:
For discrete case:
```
E[E[X|Y]] = ∑_y E[X|Y=y]P(Y=y)
          = ∑_y ∑_x xP(X=x|Y=y)P(Y=y)
          = ∑_x x∑_y P(X=x|Y=y)P(Y=y)
          = ∑_x xP(X=x)
          = E[X]
```

### 3. Special Cases and Important Results

##### Functions of Random Variables
For any function g(X):
$E[g(X)] = \begin{cases} 
\sum_x g(x)P(X=x) & \text{discrete case} \\
\int_{-\infty}^{\infty} g(x)f(x)dx & \text{continuous case}
\end{cases}$

##### Expected Value of Product
For any two random variables X and Y:
$E[XY] = E[X]E[Y] + Cov(X,Y)$

For independent variables:
$E[XY] = E[X]E[Y]$

### 4. Sample Mean Properties

##### Sample Mean
For observations {X₁, ..., Xₙ}:
$\bar{X} = \frac{1}{n}\sum_{i=1}^{n} X_i$

##### Properties of Sample Mean
1. $E[\bar{X}] = \mu$ (unbiased)
2. $Var(\bar{X}) = \frac{\sigma^2}{n}$
3. $SE(\bar{X}) = \frac{\sigma}{\sqrt{n}}$


### 5. Important Inequalities

##### Jensen's Inequality
For convex function g:
$g(E[X]) \leq E[g(X)]$

##### Markov's Inequality
For non-negative X and a > 0:
$P(X \geq a) \leq \frac{E[X]}{a}$

##### Chebyshev's Inequality
For any k > 0:
$P(|X-\mu| \geq k\sigma) \leq \frac{1}{k^2}$

### 6. Relationship with Characteristic Function

##### Definition
$\phi_X(t) = E[e^{itX}]$

##### Moments from Characteristic Function
$E[X^n] = i^{-n}\frac{d^n}{dt^n}\phi_X(t)|_{t=0}$

### 7. Computational Considerations

##### Monte Carlo Estimation
$E[X] \approx \frac{1}{n}\sum_{i=1}^{n} X_i$

##### Error in Estimation
Standard Error = $\frac{\sigma}{\sqrt{n}}$

95% Confidence Interval ≈ $\bar{X} \pm 1.96\frac{\sigma}{\sqrt{n}}$