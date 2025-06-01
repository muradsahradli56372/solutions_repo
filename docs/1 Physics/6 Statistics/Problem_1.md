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


## 5. Convergence Analysis of Sample Means

The plot titled **"Convergence Diagnostics for Sample Means"** illustrates how the statistical properties of sample means change as the sample size increases, using three different distributions: **Uniform**, **Exponential**, and **Binomial**.

The figure is divided into four subplots:

1. **Skewness**:
   - As sample size increases, skewness converges toward 0, especially for symmetric distributions like Uniform and Binomial.
   - Exponential distribution shows positive skewness, which reduces with larger samples due to the Central Limit Theorem (CLT).

2. **Excess Kurtosis**:
   - Measures the "tailedness" of the sampling distribution.
   - Converges toward 0 as sample size increases, again supporting CLT expectations.

3. **SE Ratio (Observed / Expected)**:
   - Compares the empirically observed standard error with the theoretical expectation `σ/√n`.
   - All distributions show the ratio approaching 1, confirming that standard error behaves as expected under sampling.

4. **Shapiro–Wilk p-value**:
   - Tests for normality.
   - For large sample sizes, all distributions yield high p-values (above 0.05), suggesting that the sampling distributions become approximately normal — a core principle of CLT.

These plots collectively demonstrate that regardless of the underlying population distribution, the distribution of sample means becomes increasingly normal as the sample size increases.
```
import os
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from scipy import stats


np.random.seed(42)
plt.style.use('seaborn-v0_8-whitegrid')
sns.set_palette("viridis")


N = 100_000
populations = {
    "Uniform": np.random.uniform(0, 1, N),
    "Exponential": np.random.exponential(scale=1.0, size=N),
    "Binomial": np.random.binomial(n=10, p=0.3, size=N)
}

sample_sizes = [2, 5, 10, 15, 20, 30, 50, 100]
num_samples = 3000

def generate_distributions(population, sample_sizes, num_samples):
    return {
        n: np.array([np.mean(np.random.choice(population, size=n, replace=True)) for _ in range(num_samples)])
        for n in sample_sizes
    }

def compute_stats(distributions, population):
    pop_std = np.std(population)
    rows = []
    for n, means in distributions.items():
        subset = means if len(means) <= 5000 else np.random.choice(means, 5000, replace=False)
        _, p_sw = stats.shapiro(subset)
        rows.append({
            'Sample Size': n,
            'Skewness': stats.skew(means),
            'Kurtosis': stats.kurtosis(means),
            'SE Ratio': means.std() / (pop_std / np.sqrt(n)),
            'Shapiro-Wilk p-value': p_sw
        })
    return pd.DataFrame(rows)


all_stats = []
for name, population in populations.items():
    dist = generate_distributions(population, sample_sizes, num_samples)
    df = compute_stats(dist, population)
    df['Distribution'] = name
    all_stats.append(df)

df_all = pd.concat(all_stats)

# Vizualizasiya
fig, axes = plt.subplots(2, 2, figsize=(14, 12), constrained_layout=True)
axes = axes.flatten()
metrics = ['Skewness', 'Kurtosis', 'SE Ratio', 'Shapiro-Wilk p-value']
titles = ['Skewness', 'Excess Kurtosis', 'SE Ratio (Observed/Expected)', 'Shapiro–Wilk p-value']
y_labels = ['Skewness', 'Kurtosis', 'Ratio', 'p-value']
horizontal_lines = [0, 0, 1, 0.05]
yscales = ['linear', 'linear', 'linear', 'log']

for i, metric in enumerate(metrics):
    ax = axes[i]
    for dist in populations.keys():
        subset = df_all[df_all['Distribution'] == dist]
        ax.plot(subset['Sample Size'], subset[metric], 'o-', label=dist)
    ax.axhline(horizontal_lines[i], linestyle='--', color='red' if i==3 else 'black', alpha=0.7)
    ax.set_xscale('log')
    ax.set_yscale(yscales[i])
    ax.set_title(titles[i])
    ax.set_xlabel('Sample Size')
    ax.set_ylabel(y_labels[i])
    ax.legend()
    ax.grid(alpha=0.3)

fig.suptitle('Convergence Diagnostics for Sample Means', fontsize=18)
fig.savefig('convergence_analysis.png', dpi=300)
plt.show()
print("Saved plot as: convergence_analysis.png")
```
![alt text](<download (1).png>)


## 6. Practical Implications of CLT

The Central Limit Theorem underpins many real-world applications. Here are three areas where its impact is significant:

### 6.1 Quality Control

In manufacturing, measurements (e.g., lengths, weights) from a process may vary. Even if individual measurements are not normally distributed, the **average of a sample** of these measurements will be approximately normal, allowing for:

- Control chart construction
- Defect detection using statistical thresholds

### 6.2 Finance and Risk Modeling

Financial returns may have non-normal characteristics (e.g., skewness, kurtosis), but **averaging returns** over time or across portfolios often leads to normally distributed values. This assumption allows:

- Reliable estimation of expected returns
- Risk measurement tools like Value at Risk (VaR)

### 6.3 Statistical Inference

Most parametric tests (e.g., $t$-test, ANOVA) assume normality of the sampling distribution of the mean. CLT justifies this assumption, making such tests valid when:

- The sample size is sufficiently large (typically $n \geq 30$)
- The data are independently and identically distributed (i.i.d.)

---

**Conclusion**:  
Through simulation, we observed the power of the Central Limit Theorem. Regardless of how skewed or irregular the original data may be, the average of enough samples behaves in a predictable and structured way — it becomes approximately normal. This principle enables confidence in statistical modeling, even in uncertain and complex data environments.


