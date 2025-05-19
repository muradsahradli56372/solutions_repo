# Problem 1: Simulating the Effects of the Lorentz Force

## Introduction

The Lorentz force is the fundamental law describing the motion of charged particles in electric and magnetic fields:

$$
\vec{F} = q(\vec{E} + \vec{v} \times \vec{B})
$$

It governs particle behavior in systems ranging from **cyclotrons**, **mass spectrometers**, and **plasma traps**, to the magnetic fields of astrophysical objects. In this project, we simulate particle motion under different field conditions to gain an intuitive understanding of how the Lorentz force governs motion.

---

## 1. Applications of the Lorentz Force

- **Particle Accelerators**: Lorentz force is used to steer and accelerate particles using electric/magnetic fields.
- **Mass Spectrometers**: Separate ions by charge-to-mass ratio using circular trajectories in magnetic fields.
- **Plasma Confinement**: Magnetic fields constrain charged particles in fusion devices like tokamaks.

Electric fields influence **acceleration** while magnetic fields affect **direction** — typically resulting in **circular** or **helical** motion.

---

## 2. Simulating Particle Motion

### 2.1 Equations of Motion

Using Newton’s second law:

$$
m\frac{d\vec{v}}{dt} = q(\vec{E} + \vec{v} \times \vec{B})
$$

This leads to a system of coupled differential equations which we solve numerically using the **Euler method**.

### 2.2 Initial Setup (Python)

```python
import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

# Constants
q = 1.0       # charge (C)
m = 1.0       # mass (kg)
B = np.array([0, 0, 1.0])  # Uniform magnetic field (T)
E = np.array([0.0, 0.0, 0.0])  # Uniform electric field (V/m)
v0 = np.array([1.0, 0.0, 1.0])  # Initial velocity (m/s)
r0 = np.array([0.0, 0.0, 0.0])  # Initial position (m)

# Time settings
dt = 0.01
num_steps = 2000

# Initialize position and velocity arrays
r = np.zeros((num_steps, 3))
v = np.zeros((num_steps, 3))
r[0], v[0] = r0, v0

# Euler method simulation
for i in range(num_steps - 1):
    F = q * (E + np.cross(v[i], B))
    a = F / m
    v[i+1] = v[i] + a * dt
    r[i+1] = r[i] + v[i] * dt
```
## 3. Parameter Exploration

By changing various parameters in the simulation, we can observe how they influence the trajectory of a charged particle. The most important parameters to explore include:

### 3.1 Magnetic Field Strength $|\vec{B}|$

- **Effect**: A stronger magnetic field increases the centripetal force on the particle.
- **Result**: The particle spirals more tightly; the radius of curvature (Larmor radius) decreases.
- **Formula**:

$$
r_L = \frac{mv_\perp}{|qB|}
$$

where:
- $r_L$ is the Larmor radius,
- $v_\perp$ is the component of velocity perpendicular to $\vec{B}$.

### 3.2 Electric Field Strength $|\vec{E}|$

- **Effect**: Causes net acceleration in the direction of the electric field.
- **Result**:
  - If $\vec{E}$ is parallel to $\vec{B}$ → motion stretches along the field lines.
  - If $\vec{E}$ is perpendicular to $\vec{B}$ → particle experiences **E × B drift**.

### 3.3 Initial Velocity $\vec{v}_0$

- **Parallel to $\vec{B}$** ($v_\parallel$): The particle moves linearly along the magnetic field direction.
- **Perpendicular to $\vec{B}$** ($v_\perp$): Circular motion due to Lorentz force.
- **Mixed Components**: The motion becomes **helical**.

### 3.4 Charge $q$ and Mass $m$

- **Effect**: These values directly affect the radius and acceleration.
- **Observation**:
  - Increasing charge $q$ → tighter spiral (stronger force).
  - Increasing mass $m$ → wider spiral (more inertia).
- **Cyclotron frequency**:

$$
\omega_c = \frac{|q|B}{m}
$$

This frequency determines how fast the particle revolves in the magnetic field.

---

### Summary Table

| Parameter          | Physical Meaning                            | Trajectory Effect                              |
|-------------------|---------------------------------------------|------------------------------------------------|
| $|\vec{B}|$        | Magnetic field strength                     | Tighter or looser spirals (affects $r_L$)      |
| $|\vec{E}|$        | Electric field strength                     | Linear acceleration or drift                   |
| $\vec{v}_0$        | Initial velocity                            | Determines shape: circular, helical, linear    |
| $q$ (charge)       | Particle’s electric charge                  | Stronger force → faster turns                  |
| $m$ (mass)         | Particle’s mass                             | Heavier particles spiral more slowly           |

---


