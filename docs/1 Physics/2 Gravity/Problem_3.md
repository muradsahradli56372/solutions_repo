
# Trajectories of a Freely Released Payload Near Earth

## Motivation:
When an object is released from a moving rocket near Earth, its trajectory depends on the initial conditions and gravitational forces. This scenario is important in orbital mechanics, as it helps understand how payloads are deployed or returned to Earth. The problem involves various types of trajectories (parabolic, hyperbolic, elliptical) based on the initial conditions and forces acting on the object.

## Task:
1. **Analyze the possible trajectories** (e.g., parabolic, hyperbolic, elliptical) of a payload released near Earth.
2. **Perform a numerical analysis** to compute the path of the payload based on given initial conditions (position, velocity, and altitude).
3. **Discuss how these trajectories relate** to orbital insertion, reentry, or escape scenarios.
4. **Develop a computational tool** to simulate and visualize the motion of the payload under Earth's gravity, considering initial velocities and directions.

## Background:

In orbital mechanics, the trajectory of an object depends on the gravitational force exerted by the Earth and the object's velocity. The possible types of trajectories are:
- **Elliptical**: The object remains in orbit around Earth, like a satellite.
- **Parabolic**: The object escapes Earth’s gravity, following a parabolic path, but will eventually return to Earth.
- **Hyperbolic**: The object escapes Earth's gravity and continues to travel away indefinitely.

The trajectory is governed by Newton's Law of Gravitation:

$$
F = \frac{GMm}{r^2}
$$

Where:
- $F$ is the gravitational force,
- $G$ is the gravitational constant $(6.67430 \times 10^{-11} \, \text{m}^3\text{kg}^{-1}\text{s}^{-2})$,
- $M$ is the mass of the Earth $(5.972 \times 10^{24} \, \text{kg})$,
- $m$ is the mass of the object,
- $r$ is the distance between the object and the center of the Earth.

The object's motion can be described using its initial position and velocity and the acceleration due to gravity.

## Approach:

### 1. **Mathematical Formulation**:
We use the gravitational force to derive the equation of motion for the object. The second law of motion states that:

$$
\vec{F} = m \vec{a}
$$

For an object under the influence of Earth's gravity:

$$
m \frac{d^2 \vec{r}}{dt^2} = - \frac{GMm}{r^2} \hat{r}
$$

Where $\vec{r}$ is the position vector, and $r$ is the radial distance from the center of the Earth. This equation can be simplified to solve for the object's trajectory.

### 2. **Numerical Solution**:
We will use numerical methods (e.g., Euler's method or Runge-Kutta method) to solve the system of differential equations for the motion of the payload. Python libraries such as NumPy and SciPy are ideal for this.

## Python Code Implementation:

We will simulate the trajectory of a freely released payload near Earth using initial position, velocity, and angle. The solution will be visualized to show the different types of trajectories.

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.integrate import solve_ivp

# Constants
G = 6.67430e-11  # gravitational constant (m^3 kg^-1 s^-2)
M_earth = 5.972e24  # mass of Earth (kg)
R_earth = 6.371e6  # radius of Earth (m)

# Define gravitational acceleration
def gravity_accel(x, y):
    r = np.sqrt(x**2 + y**2)
    a = -G * M_earth / r**3
    return a * x, a * y

# Define the system of differential equations
def equations(t, state):
    x, y, vx, vy = state
    ax, ay = gravity_accel(x, y)
    return [vx, vy, ax, ay]

# Simulation function
def simulate_payload(v0, angle_deg):
    # Initial conditions
    angle_rad = np.deg2rad(angle_deg)
    x0 = R_earth  # Start from Earth's surface at x = R_earth
    y0 = 0
    vx0 = v0 * np.cos(angle_rad)
    vy0 = v0 * np.sin(angle_rad)
    initial_state = [x0, y0, vx0, vy0]

    # Time span for the simulation
    t_span = (0, 20000)  # seconds
    t_eval = np.linspace(*t_span, 5000)

    # Solve the equations
    solution = solve_ivp(equations, t_span, initial_state, t_eval=t_eval, rtol=1e-8)

    # Extract the results
    x = solution.y[0]
    y = solution.y[1]

    # Plot the trajectory
    plt.figure(figsize=(8, 8))
    plt.plot(x, y, label="Payload Trajectory")
    earth = plt.Circle((0, 0), R_earth, color='blue', label="Earth", alpha=0.5)
    plt.gca().add_artist(earth)
    plt.xlabel('X Position (m)')
    plt.ylabel('Y Position (m)')
    plt.title(f"Payload Trajectory (v0 = {v0} m/s, angle = {angle_deg}°)")
    plt.axis('equal')
    plt.legend()
    plt.grid(True)
    plt.show()

# Example usage
simulate_payload(v0=8000, angle_deg=0)  # Low orbit-like speed
simulate_payload(v0=11200, angle_deg=45)  # Near escape velocity with angle
simulate_payload(v0=5000, angle_deg=90)  # Just upwards launch

```


![alt text](<download (2).png>)

## Explanation of the Code

This Python script simulates the trajectory of a payload released near Earth with a given initial velocity and angle. It models the motion considering Earth's gravity and visualizes the payload's path.

## Key Points:

The initial conditions are set by velocity (v0) and launch angle (theta).

Gravitational force is taken into account based on Newton's law of gravitation.

The Earth is represented as a circle on the plot.

The payload's trajectory is simulated over time and plotted.

## Usage:

Modify the v0 and theta values to observe different types of trajectories (orbital, escape, fall back).

Use this to understand how initial conditions affect motion near a planet.
---

## Graphical Representation

The output will be a plot of the payload's trajectory over time.  
This will show how the object moves away from Earth and either stays in orbit or escapes, depending on the initial velocity.

---

## Discussion

The possible types of trajectories that the payload can follow are:

- **Elliptical:**  
  If the velocity is below escape velocity but high enough to form an elliptical orbit, the payload will follow an elliptical path around the Earth.

- **Parabolic:**  
  If the velocity is exactly equal to the escape velocity, the payload will follow a parabolic path, moving away from Earth but eventually returning.

- **Hyperbolic:**  
  If the velocity exceeds the escape velocity, the payload will follow a hyperbolic trajectory, escaping Earth's gravity and continuing on its path.

In space mission planning, understanding these trajectories is crucial for determining the required velocity to reach a specific orbit, deploy satellites, or escape Earth's gravitational influence.

---

## Real-World Applications

- **Orbital Insertion:**  
  To place a satellite into orbit, the payload must reach the correct velocity for a stable elliptical orbit.

- **Reentry:**  
  Understanding the trajectory helps in planning safe reentry paths for returning payloads or spacecraft.

- **Escape:**  
  For interplanetary missions or space exploration, we need to ensure that the payload reaches escape velocity to leave Earth's gravitational influence.

---

