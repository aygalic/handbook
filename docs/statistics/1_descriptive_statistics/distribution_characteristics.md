# Distribution Characteristics

## 1. Descriptive Measures of Distributions

### Skewness
Skewness measures the asymmetry of a probability distribution:
- **Left-skewed (Negative)**: Tail extends to the left, mean < median
- **Right-skewed (Positive)**: Tail extends to the right, mean > median
- **Symmetric**: Mean = median, skewness = 0

### Kurtosis
Kurtosis measures the "tailedness" of a probability distribution:
- **Heavy-tailed (Leptokurtic)**: Higher kurtosis, more extreme values
- **Light-tailed (Platykurtic)**: Lower kurtosis, fewer extreme values
- **Normal (Mesokurtic)**: Reference kurtosis = 3 for normal distribution

### Moments of a Distribution
Moments are quantitative measures that describe the shape of a distribution:
- **First Moment**: Mean (Expected Value)
- **Second Moment**: Variance and Standard Deviation
- **Third Moment**: Related to Skewness
- **Fourth Moment**: Related to Kurtosis

## 2. Common Distribution Expected Values and Variances

### Normal Distribution N(μ, σ²)
- Expected Value: $E[X] = \mu$
- Variance: $Var(X) = \sigma^2$

### Exponential Distribution (rate λ)
- Expected Value: $E[X] = \frac{1}{\lambda}$
- Variance: $Var(X) = \frac{1}{\lambda^2}$

### Poisson Distribution (rate λ)
- Expected Value: $E[X] = \lambda$
- Variance: $Var(X) = \lambda$

### Binomial Distribution (n trials, probability p)
- Expected Value: $E[X] = np$
- Variance: $Var(X) = np(1-p)$

### Geometric Distribution (probability p)
- Expected Value: $E[X] = \frac{1}{p}$
- Variance: $Var(X) = \frac{1-p}{p^2}$

## 3. Higher Moments

### kth Moment
The kth moment about zero (raw moment) is defined as:

$E[X^k] = \begin{cases}
\sum_x x^k P(X=x) & \text{discrete case} \\
\int_{-\infty}^{\infty} x^k f(x)dx & \text{continuous case}
\end{cases}$

### Central Moments
The kth central moment (moment about the mean) is defined as:

$E[(X-\mu)^k] = \begin{cases}
\sum_x (x-\mu)^k P(X=x) & \text{discrete case} \\
\int_{-\infty}^{\infty} (x-\mu)^k f(x)dx & \text{continuous case}
\end{cases}$

### Important Relations
- First Central Moment: $E[X-\mu] = 0$
- Second Central Moment: $E[(X-\mu)^2] = Var(X)$
- Third Standardized Moment: $E[(X-\mu)^3]/\sigma^3$ (Skewness)
- Fourth Standardized Moment: $E[(X-\mu)^4]/\sigma^4$ (Kurtosis)