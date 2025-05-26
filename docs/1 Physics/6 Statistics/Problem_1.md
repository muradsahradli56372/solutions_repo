# Problem 1: Exploring the Central Limit Theorem (CLT) through Simulations

## Motivation

The Central Limit Theorem (CLT) is a foundational principle in statistics. It states that:

> Regardless of the population distribution, the distribution of the sample mean tends toward a normal distribution as the sample size increases.

This concept is critical in many statistical methods such as confidence intervals, hypothesis testing, and quality control. Here, we explore CLT using simulations on different population distributions.

---

## 1. Simulation Plan

### Population Distributions Used

- **Uniform Distribution** $U(0, 1)$  
![alt text](<output (5)-1.png>)
- **Exponential Distribution** $\text{Exp}(\lambda=1)$  
![alt text](<output (4)-1.png>)
- **Binomial Distribution** $B(n=10, p=0.5)$  
![alt text](<output (6)-1.png>)

For each, we:

1. Generate a large synthetic population dataset,
2. Randomly sample `n` elements (for $n = 5, 10, 30, 50$),
3. Repeat the sampling process 1000 times,
4. Calculate the **sample mean** in each trial,
5. Plot the **sampling distribution** of those means.

---

## 2. Python Code for Simulation

```python
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

sns.set(style="whitegrid")
np.random.seed(42)

distributions = {
    "Uniform": lambda size: np.random.uniform(0, 1, size),
    "Exponential": lambda size: np.random.exponential(1.0, size),
    "Binomial": lambda size: np.random.binomial(n=10, p=0.5, size=size)
}

sample_sizes = [5, 10, 30, 50]
num_samples = 1000

for dist_name, dist_func in distributions.items():
    plt.figure(figsize=(14, 10))
    for i, n in enumerate(sample_sizes, 1):
        means = [np.mean(dist_func(n)) for _ in range(num_samples)]
        plt.subplot(2, 2, i)
        sns.histplot(means, bins=30, kde=True, color='skyblue')
        plt.title(f"{dist_name} Distribution - Sample size: {n}")
        plt.xlabel("Sample Mean")
        plt.ylabel("Frequency")
    plt.suptitle(f"Sampling Distribution of the Mean — {dist_name}", fontsize=16)
    plt.tight_layout(rect=[0, 0, 1, 0.96])
    plt.savefig(f"clt_{dist_name.lower()}.png", dpi=150)
    plt.show()
```

## 3. Visual Results and Interpretation

In this simulation, we explored how the distribution of sample means changes as the sample size increases. Three types of population distributions were used:

- Uniform Distribution $U(0,1)$
- Exponential Distribution $\text{Exp}(1)$
- Binomial Distribution $B(n=10, p=0.5)$

For each, we took random samples of size $n = 5, 10, 30, 50$, repeated the sampling process 1000 times, and computed the sample mean each time.

### Key Observations

- **Uniform Distribution**: Initially flat and symmetric. As $n$ increases, the sample mean distribution becomes increasingly bell-shaped.
- **Exponential Distribution**: Originally skewed to the right. Small sample sizes retain the skewness, but as $n$ grows, the distribution of sample means becomes symmetric and normal-like.
- **Binomial Distribution**: Discrete and symmetric (when $p=0.5$). Sampling means smoothen out the discreteness, resulting in an approximate normal distribution for larger $n$.

These behaviors demonstrate the Central Limit Theorem in action — regardless of the population shape, the distribution of the sample mean tends toward normality as sample size increases.

---

## 4. Insights from Parameter Exploration

The simulation allowed us to explore how various parameters influence the convergence toward a normal distribution.

### 4.1 Sample Size ($n$)

- **Smaller $n$**: Sampling distributions resemble the original population shape.
- **Larger $n$**: The Central Limit Theorem dominates, and the distribution of sample means becomes nearly normal.

### 4.2 Shape of the Population Distribution

- **Symmetric distributions** (like uniform) converge more quickly to a normal distribution.
- **Skewed or heavy-tailed distributions** (like exponential) require larger sample sizes for normal-like behavior to emerge.

### 4.3 Population Variance

- Affects the **spread** of the sampling distribution.
- The standard deviation of the sample mean is given by:

  $$
  \sigma_{\bar{x}} = \frac{\sigma}{\sqrt{n}}
  $$

- So, increasing the sample size reduces the variability of the sample mean, making estimates more precise.

### 4.4 Summary Table

| Parameter            | Effect on Sampling Distribution                     |
|----------------------|------------------------------------------------------|
| Sample Size ($n$)    | Larger $n$ → smoother, more normal, less spread     |
| Skewness             | More skew → slower convergence to normality         |
| Variance ($\sigma^2$)| Higher variance → wider sampling distribution       |

---

## 5. Practical Implications of CLT

The Central Limit Theorem underpins many real-world applications. Here are three areas where its impact is significant:

### 5.1 Quality Control

In manufacturing, measurements (e.g., lengths, weights) from a process may vary. Even if individual measurements are not normally distributed, the **average of a sample** of these measurements will be approximately normal, allowing for:

- Control chart construction
- Defect detection using statistical thresholds

### 5.2 Finance and Risk Modeling

Financial returns may have non-normal characteristics (e.g., skewness, kurtosis), but **averaging returns** over time or across portfolios often leads to normally distributed values. This assumption allows:

- Reliable estimation of expected returns
- Risk measurement tools like Value at Risk (VaR)

### 5.3 Statistical Inference

Most parametric tests (e.g., $t$-test, ANOVA) assume normality of the sampling distribution of the mean. CLT justifies this assumption, making such tests valid when:

- The sample size is sufficiently large (typically $n \geq 30$)
- The data are independently and identically distributed (i.i.d.)

---

**Conclusion**:  
Through simulation, we observed the power of the Central Limit Theorem. Regardless of how skewed or irregular the original data may be, the average of enough samples behaves in a predictable and structured way — it becomes approximately normal. This principle enables confidence in statistical modeling, even in uncertain and complex data environments.


