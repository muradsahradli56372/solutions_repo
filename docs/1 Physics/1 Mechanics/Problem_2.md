#  Investigating the Dynamics of a Forced Damped Pendulum

## **1. Theoretical Foundation**

The motion of a forced damped pendulum is governed by the equation:
$$ \frac{d^2\theta}{dt^2} + \gamma \frac{d\theta}{dt} + \omega_0^2 \sin\theta = F_0 \cos(\omega t) $$
where:
- $ \theta $ is the angular displacement,
- $ \gamma $ is the damping coefficient,
- $ \omega_0 = \sqrt{\frac{g}{L}} $ is the natural frequency,
- $ F_0 $ is the amplitude of the external force,
- $ \omega $ is the driving frequency.

For small angles ($ \theta \approx \sin\theta $), the equation simplifies to:
$$ \frac{d^2\theta}{dt^2} + \gamma \frac{d\theta}{dt} + \omega_0^2 \theta = F_0 \cos(\omega t) $$

### **Resonance Condition**
Resonance occurs when the external driving frequency matches the system's natural frequency ($ \omega \approx \omega_0 $), leading to large amplitude oscillations. The steady-state solution is:
$$ \theta_{ss}(t) = A \cos(\omega t - \delta) $$
where the amplitude $ A $ depends on the damping coefficient and driving force.

---

## **2. Analysis of Dynamics**

### **Effects of System Parameters:**
- **Damping coefficient ($ \gamma $):** Higher damping reduces oscillation amplitude and shifts resonance frequency.
- **Driving amplitude ($ F_0 $):** Increases oscillation amplitude; beyond a threshold, chaotic motion emerges.
- **Driving frequency ($ \omega $):** Controls resonance and synchronization behavior.

### **Chaotic Motion:**
For high driving forces and low damping, the system can exhibit chaotic behavior, meaning small changes in initial conditions lead to unpredictable long-term behavior.

---

## **3. Practical Applications**

- **Energy Harvesting:** Used in devices that convert mechanical vibrations into electrical energy.
- **Suspension Bridges:** Engineering designs must consider resonance to prevent catastrophic oscillations (e.g., Tacoma Narrows Bridge).
- **Electrical Circuits:** Analogous to driven RLC circuits where voltage replaces angular displacement.

---

## **4. Implementation in Python**

### **Numerical Simulation using Runge-Kutta Method**
```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.integrate import solve_ivp

# Parameters
g = 9.81  # Gravity (m/s^2)
L = 1.0   # Length of pendulum (m)
gamma = 0.2  # Damping coefficient
F0 = 1.2  # Driving force amplitude
omega = 2.0  # Driving frequency

# Equation of motion
def pendulum(t, y):
    theta, omega_ = y
    dydt = [omega_, -gamma * omega_ - (g/L) * np.sin(theta) + F0 * np.cos(omega * t)]
    return dydt

# Solve using Runge-Kutta method
Tmax = 50
initial_conditions = [0.1, 0]  # Small initial displacement
sol = solve_ivp(pendulum, [0, Tmax], initial_conditions, t_eval=np.linspace(0, Tmax, 1000))

# Plot Results
plt.figure(figsize=(10, 5))
plt.plot(sol.t, sol.y[0])
plt.xlabel("Time (s)")
plt.ylabel("Angle (rad)")
plt.title("Forced Damped Pendulum Motion")
plt.grid()
plt.show()
```

---

### **Phase Portrait & Poincaré Section**
To analyze chaos, we plot the phase space:
```python
plt.figure(figsize=(8, 6))
plt.plot(sol.y[0], sol.y[1], lw=0.5)
plt.xlabel("Theta (rad)")
plt.ylabel("Angular Velocity (rad/s)")
plt.title("Phase Portrait")
plt.grid()
plt.show()
```

For the Poincaré section (stroboscopic map at multiples of the driving period):
```python
time_mod = sol.t % (2 * np.pi / omega)
poincare_indices = np.where(time_mod < 0.01)[0]
plt.scatter(sol.y[0][poincare_indices], sol.y[1][poincare_indices], s=10)
plt.xlabel("Theta (rad)")
plt.ylabel("Angular Velocity (rad/s)")
plt.title("Poincaré Section")
plt.grid()
plt.show()
```

---

## **5. Deliverables Summary**

1. **Markdown document with Python simulations**
2. **Mathematical derivations of small-angle solutions**
3. **Graphical representations:**
   - Time evolution of motion
   - Phase portrait
   - Poincaré section
4. **Discussion of limitations and potential extensions:**
   - Nonlinear damping effects
   - Stochastic forcing (random perturbations)
   - Multi-degree-of-freedom pendulum interactions

---
