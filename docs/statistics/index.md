# Core Statistical Concepts for Data Science Interviews

1. [Descriptive Statistics](./1_descriptive_statistics/)
2. [Probability Distributions](./2_probability_distributions/)
3. [Statistical Inference]()
4. [Regression Analysis]()
5. [Experimental Design]()
6. [Time Series Analysis]()
7. [Bayesian Statistics]()
8. [Advanced Topics]()

## Common Interview Questions
- [When to use t-test vs z-test](./common-questions/t-test-vs-z-test.md)
- [Handling Missing Data](./common-questions/missing-data.md)
- [Correlation vs Causation](./common-questions/correlation-causation.md)

## [Interview Tips](./interview-tips.md)


## 1. Descriptive Statistics
- **Measures of Central Tendency**
  - Mean: Arithmetic, geometric, harmonic means and their use cases
  - Median: Robustness to outliers
  - Mode: Use in categorical data
  - Relationship between mean, median, mode in skewed distributions

- **Measures of Dispersion**
  - Variance and Standard Deviation
  - Range and Interquartile Range (IQR)
  - Coefficient of Variation
  - Mean Absolute Deviation

- **Distribution Characteristics**
  - Skewness: Left vs right-skewed distributions
  - Kurtosis: Heavy vs light tails
  - Moments of a distribution

## 2. Probability Distributions
- **Discrete Distributions**
  - Bernoulli and Binomial
  - Poisson (rare events)
  - Geometric and Negative Binomial
  - Hypergeometric

- **Continuous Distributions**
  - Normal/Gaussian
  - Student's t-distribution
  - Chi-square distribution
  - F-distribution
  - Exponential and Gamma
  - Beta distribution

## 3. Statistical Inference
- **Sampling Theory**
  - Population vs Sample
  - Sampling distributions
  - Central Limit Theorem
  - Law of Large Numbers
  - Standard Error

- **Estimation**
  - Point estimates
  - Confidence intervals
  - Maximum Likelihood Estimation (MLE)
  - Bias-Variance tradeoff

- **Hypothesis Testing**
  - Null and Alternative hypotheses
  - Type I and Type II errors
  - p-values and significance levels
  - Statistical power
  - Multiple testing problem

## 4. Regression Analysis
- **Simple Linear Regression**
  - Assumptions
  - Least Squares estimation
  - R-squared and adjusted R-squared
  - Residual analysis

- **Multiple Linear Regression**
  - Multicollinearity
  - Feature selection
  - Interaction terms
  - Polynomial regression

- **Generalized Linear Models**
  - Logistic regression
  - Poisson regression
  - Link functions

## 5. Experimental Design
- **Basic Principles**
  - Randomization
  - Replication
  - Blocking
  - Factorial designs

- **A/B Testing**
  - Sample size determination
  - Statistical significance
  - Effect size
  - Multiple testing corrections

## 6. Time Series Analysis
- **Components**
  - Trend
  - Seasonality
  - Cyclical patterns
  - Random variation

- **Methods**
  - Moving averages
  - Exponential smoothing
  - ARIMA models
  - Stationarity

## 7. Bayesian Statistics
- **Fundamentals**
  - Bayes' Theorem
  - Prior and posterior distributions
  - Conjugate priors
  - Bayesian inference

- **Applications**
  - Bayesian A/B testing
  - Bayesian regression
  - Hierarchical models

## 8. Advanced Topics
- **Dimensionality Reduction**
  - Principal Component Analysis (PCA)
  - Factor Analysis
  - t-SNE

- **Non-parametric Statistics**
  - Kernel Density Estimation
  - Spearman correlation
  - Mann-Whitney U test
  - Kruskal-Wallis test

- **Survival Analysis**
  - Censoring
  - Kaplan-Meier estimates
  - Cox proportional hazards

## Interview Tips
- Always start with the simplest explanation
- Be prepared to explain assumptions behind methods
- Know when to use which test/method
- Be able to explain limitations and alternatives
- Practice explaining technical concepts to non-technical audiences
- Be ready to discuss real-world applications



## Common Interview Questions
<details>
<summary>1. When would you use a t-test vs z-test?</summary>
<br>
Let me break down the key differences between t-tests and z-tests and explain when to use each one:

Key Distinctions:

1. Population Standard Deviation
- Z-test: Used when we KNOW the population standard deviation (σ)
- T-test: Used when we DON'T know the population standard deviation and must estimate it using sample standard deviation (s)

2. Sample Size
- Z-test: Generally used for large samples (n > 30)
- T-test: Better for small samples (n < 30) because it accounts for the extra uncertainty in estimating the standard deviation

3. Distribution
- Z-test: Assumes data follows a normal distribution
- T-test: Uses Student's t-distribution, which has heavier tails than normal distribution to account for additional uncertainty

Here's a practical example:

Scenario 1: Quality Control in Large Manufacturing Plant
- Testing widget weights
- Years of historical data available
- Known population standard deviation
- Large daily samples
→ Use Z-test because you know σ and have large samples

Scenario 2: Medical Research Study
- Testing new drug effectiveness
- Small patient group (n=20)
- No known population standard deviation
- Need to estimate variance from sample
→ Use T-test because of small sample size and unknown σ

Common Interview Follow-up Questions:
1. Why does the t-distribution have heavier tails than normal distribution?
   - Because it accounts for the additional uncertainty in estimating the standard deviation
   
### Heavier Tails of T-Distribution

The t-distribution is defined as:

$t = \frac{Z}{\sqrt{V/n}}$

where:
- Z follows N(0,1)
- V follows χ²(n) (chi-square with n degrees of freedom)
- Z and V are independent

The probability density function (PDF) of t-distribution with ν degrees of freedom is:

$f(t) = \frac{\Gamma(\frac{\nu + 1}{2})}{\sqrt{\nu\pi}\Gamma(\frac{\nu}{2})}(1 + \frac{t^2}{\nu})^{-\frac{\nu + 1}{2}}$

Compare this to the normal distribution PDF:

$f(x) = \frac{1}{\sqrt{2\pi}}e^{-\frac{x^2}{2}}$

The key difference is in the tails:
- Normal distribution: Decays as $e^{-x^2/2}$
- t-distribution: Decays as $x^{-(\nu+1)}$

For small ν, the polynomial decay of t-distribution is slower than the exponential decay of normal distribution, resulting in heavier tails.


2. What happens to the t-distribution as sample size increases?
   - It approaches the normal distribution (degrees of freedom increase)

### 2. Convergence to Normal Distribution

As n → ∞, we can prove convergence using:

1. The Central Limit Theorem for V/n:
   $\frac{V/n - 1}{\sqrt{2/n}} \xrightarrow{d} N(0,1)$

2. Therefore, as n → ∞:
   $\sqrt{V/n} \xrightarrow{p} 1$

3. Thus:
   $t = \frac{Z}{\sqrt{V/n}} \xrightarrow{d} Z \sim N(0,1)$

This convergence can be quantified:
- For ν = 1: Cauchy distribution (undefined moments)
- For ν = 2: No fourth moment
- For ν = 3: No third moment
- As ν increases: Moments exist up to order ν-1
- As ν → ∞: All moments exist and match normal distribution




3. Can you use a t-test when you know the population standard deviation?
   - Yes, but it's less powerful than a z-test in this case


### 3. Power Analysis of T-test vs Z-test

The power function for a z-test:
$\pi_Z(\mu) = 1 - \Phi(z_{α/2} - \frac{\mu - \mu_0}{\sigma/\sqrt{n}}) + \Phi(-z_{α/2} - \frac{\mu - \mu_0}{\sigma/\sqrt{n}})$

The power function for a t-test:
$\pi_T(\mu) = 1 - F_t(t_{α/2,n-1} - \frac{\mu - \mu_0}{s/\sqrt{n}}) + F_t(-t_{α/2,n-1} - \frac{\mu - \mu_0}{s/\sqrt{n}})$

where:
- Φ is the standard normal CDF
- F_t is the t-distribution CDF
- z_{α/2} is the normal critical value
- t_{α/2,n-1} is the t critical value

The z-test is more powerful because:
1. |z_{α/2}| < |t_{α/2,n-1}| for any α and n
2. Using known σ eliminates estimation uncertainty
3. Therefore: $\pi_Z(\mu) > \pi_T(\mu)$ for any μ ≠ μ₀

Quantitatively, for α = 0.05:
- n = 10: t-test needs ~10% larger sample size for same power
- n = 30: Difference reduces to ~3%
- n > 100: Difference becomes negligible (<1%)


</details>

2. How do you handle missing data?
3. What's the difference between correlation and causation?
4. Explain the central limit theorem
5. How do you detect and handle outliers?
6. What's the difference between parametric and non-parametric tests?
7. How do you choose between different types of regression?
8. Explain cross-validation and its importance