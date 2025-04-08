# Problem 3
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
G = 6.67430e-11  # Gravitational constant (m^3 kg^-1 s^-2)
M_earth = 5.972e24  # Mass of Earth (kg)
R_earth = 6.371e6  # Radius of Earth (m)

# Initial conditions: position and velocity (in polar coordinates)
initial_altitude = 200000  # 200 km altitude above Earth's surface
initial_velocity = 10000  # Initial velocity in m/s
initial_angle = 0  # Angle of release (horizontal)

# Define the equations of motion
def equations_of_motion(t, y):
    r, theta, r_dot, theta_dot = y  # r: radial distance, theta: angle, r_dot: radial velocity, theta_dot: angular velocity
    r_double_dot = -G * M_earth / r**2  # Radial acceleration due to gravity
    return [r_dot, theta_dot, r_double_dot, 0]  # The angular velocity is constant in this simplified model

# Initial state [r, theta, r_dot, theta_dot]
initial_state = [R_earth + initial_altitude, initial_angle, initial_velocity, 0]

# Time span for the simulation (0 to 2 hours)
t_span = (0, 7200)  # 7200 seconds (2 hours)
t_eval = np.linspace(*t_span, 10000)

# Solve the differential equations
solution = solve_ivp(equations_of_motion, t_span, initial_state, t_eval=t_eval)

# Extract results
r = solution.y[0]
theta = solution.y[1]

# Convert polar coordinates to Cartesian for plotting
x = r * np.cos(theta)
y = r * np.sin(theta)

# Plot the trajectory
plt.figure(figsize=(8, 6))
plt.plot(x, y, label="Payload Trajectory")
plt.scatter(0, 0, color="red", label="Earth Center", s=100)  # Earth center
plt.title('Trajectory of a Freely Released Payload Near Earth')
plt.xlabel('X (m)')
plt.ylabel('Y (m)')
plt.grid(True)
plt.legend()
plt.show()
```

## Explanation of the Code

**Constants:**  
We define the gravitational constant, the mass of the Earth, and its radius.

**Initial Conditions:**  
The payload is released at an altitude of 200 km with an initial horizontal velocity of 10,000 m/s.

**Equations of Motion:**  
The system of equations governing the motion of the payload is derived from Newton’s Law of Gravitation. We solve for the radial distance and angular velocity.

**Numerical Solution:**  
We use `solve_ivp` from SciPy to numerically integrate the equations of motion over a time span of 2 hours.

**Visualization:**  
The trajectory is plotted in Cartesian coordinates to show the path of the payload.

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

