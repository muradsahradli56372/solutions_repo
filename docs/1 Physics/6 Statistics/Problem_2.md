
# 🎯 Problem 2 – Estimating π via Monte Carlo Methods

Monte Carlo methods use randomness to solve problems. Two classic examples for estimating π are:

- The **Circle Method**, which uses random point generation inside a square.
- The **Buffon’s Needle Method**, which uses probabilities of line-crossing.

---

## ⚪ Part 1: Circle Method

### 📌 Theory

A unit circle is inscribed in a square with side length 2. The area ratio of circle to square is:

\[
\frac{\text{Area of Circle}}{\text{Area of Square}} = \frac{\pi \cdot r^2}{4} = \frac{\pi}{4}
\]

So, we estimate π by:

\[
\pi \approx 4 \cdot \frac{\text{Points inside circle}}{\text{Total points}}
\]

---

### 🧪 Code Block 1 – Estimate π and Plot Points

```python
import numpy as np
import matplotlib.pyplot as plt

def estimate_and_plot_circle(N):
    x = np.random.uniform(-1, 1, size=N)
    y = np.random.uniform(-1, 1, size=N)
    inside = (x**2 + y**2) <= 1
    pi_estimate = 4 * np.sum(inside) / N

    plt.figure(figsize=(6,6))
    plt.scatter(x[inside], y[inside], s=1, color='blue', label='Inside Circle')
    plt.scatter(x[~inside], y[~inside], s=1, color='red', label='Outside Circle')
    theta = np.linspace(0, 2*np.pi, 300)
    plt.plot(np.cos(theta), np.sin(theta), color='black', linewidth=1, label='Unit Circle')
    plt.title(f"Monte Carlo Estimation of π\nπ ≈ {pi_estimate:.6f} with N = {N}")
    plt.axis("equal")
    plt.legend()
    plt.grid(True)
    plt.show()

    return pi_estimate
```

### 💬 Explanation
- Generates `N` random points in a 2D square.
- Checks which ones fall inside the unit circle (`x² + y² ≤ 1`).
- Calculates π as `4 × (inside points / total points)` and visualizes them.

---

### 📊 Code Block 2 – Circle Convergence Plot

```python
def convergence_circle(sample_sizes):
    import math
    errors = []
    for N in sample_sizes:
        x = np.random.uniform(-1, 1, size=N)
        y = np.random.uniform(-1, 1, size=N)
        inside = (x**2 + y**2) <= 1
        pi_est = 4 * np.sum(inside) / N
        error = abs(pi_est - math.pi)
        errors.append(error)
        print(f"N = {N}, π ≈ {pi_est:.6f}, Error = {error:.6f}")
    return errors
```

### 💬 Explanation
- Estimates π for each sample size and prints error.
- Helps visualize convergence with increasing N.

---

## 🪡 Part 2: Buffon’s Needle Method

### 📌 Theory

If a needle of length `L` is dropped on a floor with lines spaced `d` units apart, the probability it crosses a line is:

\[
P = \frac{2L}{d\pi} \quad \Rightarrow \quad \pi \approx \frac{2L \cdot N}{d \cdot N_{\text{cross}}}
\]

For simplicity, set \( L = d = 1 \).

---

### 🧪 Code Block 3 – Estimate π Using Buffon’s Needle

```python
def estimate_pi_buffon(N, L=1.0, d=1.0):
    X = np.random.uniform(0, d/2, size=N)
    theta = np.random.uniform(0, np.pi/2, size=N)
    crosses = (L/2) * np.sin(theta) >= X
    N_cross = np.sum(crosses)
    if N_cross == 0:
        return None
    return (2 * L * N) / (d * N_cross)
```

### 💬 Explanation
- Simulates `N` needle drops.
- Uses crossing probability to estimate π.

---

### 📐 Code Block 4 – Visualize Buffon’s Needles

```python
def plot_buffon_needles(N=100, L=1.0, d=1.0):
    x_center = np.random.uniform(-2, 2, size=N)
    y_center = np.random.uniform(0, d/2, size=N)
    theta = np.random.uniform(0, np.pi, size=N)

    plt.figure(figsize=(6,6))
    for y_line in range(-3, 4):
        plt.axhline(y_line * d, color='gray', linewidth=0.5)

    for i in range(N):
        dx = (L/2) * np.cos(theta[i])
        dy = (L/2) * np.sin(theta[i])
        x1, x2 = x_center[i] - dx, x_center[i] + dx
        y1, y2 = y_center[i] - dy, y_center[i] + dy
        color = 'blue' if int(y1 // d) != int(y2 // d) else 'red'
        plt.plot([x1, x2], [y1, y2], color=color, linewidth=1)

    plt.title("Buffon’s Needle Simulation")
    plt.axis("equal")
    plt.grid(True)
    plt.show()
```

### 💬 Explanation
- Visualizes each needle's placement and checks whether it crosses any line.

---

### 📊 Code Block 5 – Buffon’s Needle Convergence

```python
def convergence_buffon(sample_sizes, L=1.0, d=1.0):
    import math
    errors = []
    for N in sample_sizes:
        pi_est = estimate_pi_buffon(N, L, d)
        if pi_est:
            error = abs(pi_est - math.pi)
            errors.append(error)
            print(f"N = {N}, π ≈ {pi_est:.6f}, Error = {error:.6f}")
        else:
            print(f"N = {N}, No crossings → Cannot estimate π")
    return errors
```

## 📊 Comparative Analysis

In this section, we compare the two Monte Carlo methods used for estimating π — the **Circle Method** and **Buffon’s Needle Method** — in terms of convergence, computational efficiency, and ease of implementation.

### 🔍 Comparison Criteria

| Criterion                  | Circle Method                              | Buffon’s Needle Method                       |
|----------------------------|--------------------------------------------|----------------------------------------------|
| **Concept**                | Random points in a square                  | Randomly dropped needle crossing lines       |
| **Mathematical Foundation**| Geometric area ratio (π/4)                 | Probability of crossing (2L / dπ)            |
| **Code Simplicity**        | Very easy to implement                     | Slightly more complex due to trigonometry    |
| **Convergence Rate**       | O(1/√N)                                    | O(1/√N), slower in practice                   |
| **Variance**               | Lower variance per sample                  | Higher variance per sample                   |
| **Visualization**          | Simple 2D scatter plot                     | Requires line/needle visualization           |
| **Accuracy (small N)**     | Typically better at small N                | Less stable unless N is large                |
| **Historical Value**       | Modern numerical method                    | Classic probability-based geometry problem   |

### 📈 Observations

- The **Circle Method** tends to **converge faster** and gives a more stable estimate of π for small to moderate sample sizes.
- **Buffon’s Needle**, while mathematically elegant and historically significant, generally requires **more samples** to reach a similar level of accuracy due to its **higher variance**.
- The Circle Method relies purely on algebraic comparison \((x^2 + y^2 \leq 1)\), whereas Buffon’s Needle requires **trigonometric functions**, which can be computationally heavier.
- **Visualization** of the Circle Method is straightforward (color-coded dots), while the Needle Method offers more visually engaging outputs (lines crossing horizontal rules).

### ✅ Conclusion

Both methods demonstrate the power of randomness in approximating π. However, for educational, computational, and practical purposes, the **Circle Method** is typically preferred due to:

- Faster convergence with fewer samples
- Lower computational overhead
- Simpler implementation and clearer visualization

Buffon’s Needle remains a fascinating historical example of probabilistic geometry, ideal for illustrating connections between **randomness**, **geometry**, and **integration**.

> ✨ Tip: If your goal is speed and accuracy → use the Circle Method.  
> If your goal is demonstrating mathematical beauty → explore Buffon’s Needle!
