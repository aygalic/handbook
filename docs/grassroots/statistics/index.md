# Core Statistical Concepts for Data Science Interviews


In this section, we introduce the main tool for statistical analysis.

- 1 - [Descriptive statistiques](1_descriptive_statistics/index.md)
- 2 - [Probability distributions](2_probability_distributions/index.md)
- 3 - [Computational rules](3_computational_rules/index.md)
- 4 - [Statistical inference](4_statistical_inference/index.md)
- 5 - [Regression analysis](5_regression_analysis/index.md)
- 6 - [Experimental design](6_experimental_design/index.md)
- 7 - [Bayesian statistics](7_bayesian_statistics/index.md)
- 8 - [Advanced topics](8_advanced_topics/index.md)






## Common Interview Questions

??? "When would you use a t-test vs z-test?"

    Let me break down the key differences between t-tests and z-tests and explain when to use each one:

    ### Key Distinctions

    1. Population Standard Deviation
        - Z-test: Used when we KNOW the population standard deviation (σ)
        - T-test: Used when we DON'T know the population standard deviation and must estimate it using sample standard deviation (s)

    2. Sample Size
        - Z-test: Generally used for large samples (n > 30)
        - T-test: Better for small samples (n < 30) because it accounts for extra uncertainty in estimating the standard deviation

    3. Distribution
        - Z-test: Assumes data follows a normal distribution
        - T-test: Uses Student's t-distribution, which has heavier tails than normal distribution to account for additional uncertainty

    ### Practical Examples

    **Scenario 1: Quality Control in Large Manufacturing Plant**
    - Testing widget weights
    - Years of historical data available
    - Known population standard deviation
    - Large daily samples
    → Use Z-test because you know σ and have large samples

    **Scenario 2: Medical Research Study**
    - Testing new drug effectiveness
    - Small patient group (n=20)
    - No known population standard deviation
    - Need to estimate variance from sample
    → Use T-test because of small sample size and unknown σ

    ### Mathematical Details

    #### Heavier Tails of T-Distribution

    The t-distribution is defined as:

    $$
    t = \frac{Z}{\sqrt{V/n}}
    $$

    where:
    - Z follows N(0,1)
    - V follows χ²(n) (chi-square with n degrees of freedom)
    - Z and V are independent

    The probability density function (PDF) of t-distribution with ν degrees of freedom is:

    $$
    f(t) = \frac{\Gamma(\frac{\nu + 1}{2})}{\sqrt{\nu\pi}\Gamma(\frac{\nu}{2})}(1 + \frac{t^2}{\nu})^{-\frac{\nu + 1}{2}}
    $$

    Compare this to the normal distribution PDF:

    $$
    f(x) = \frac{1}{\sqrt{2\pi}}e^{-\frac{x^2}{2}}
    $$

    #### Convergence to Normal Distribution

    As n → ∞, we can prove convergence using:

    1. The Central Limit Theorem for V/n:
        $$
        \frac{V/n - 1}{\sqrt{2/n}} \xrightarrow{d} N(0,1)
        $$

    2. Therefore, as n → ∞:
        $$
        \sqrt{V/n} \xrightarrow{p} 1
        $$

    3. Thus:
        $$
        t = \frac{Z}{\sqrt{V/n}} \xrightarrow{d} Z \sim N(0,1)
        $$

    #### Power Analysis

    The power function for a z-test:
    $$
    \pi_Z(\mu) = 1 - \Phi(z_{α/2} - \frac{\mu - \mu_0}{\sigma/\sqrt{n}}) + \Phi(-z_{α/2} - \frac{\mu - \mu_0}{\sigma/\sqrt{n}})
    $$

    The power function for a t-test:
    $$
    \pi_T(\mu) = 1 - F_t(t_{α/2,n-1} - \frac{\mu - \mu_0}{s/\sqrt{n}}) + F_t(-t_{α/2,n-1} - \frac{\mu - \mu_0}{s/\sqrt{n}})
    $$

    where:
    - Φ is the standard normal CDF
    - F_t is the t-distribution CDF
    - z_{α/2} is the normal critical value
    - t_{α/2,n-1} is the t critical value

??? "How do you handle missing data?"
    Content for handling missing data...

??? "What's the difference between correlation and causation?"
    Content for correlation vs causation...

??? "Explain the central limit theorem"
    Content for central limit theorem...

??? "How do you detect and handle outliers?"
    Content for detecting and handling outliers...

??? "What's the difference between parametric and non-parametric tests?"
    Content for parametric vs non-parametric tests...

??? "How do you choose between different types of regression?"
    Content for choosing between different types of regression...

??? "Explain cross-validation and its importance"
    Content for cross-validation and its importance...








