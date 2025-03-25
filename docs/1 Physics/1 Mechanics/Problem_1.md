# Problem 1: Investigating the Range as a Function of the Angle of Projection

## 1. Theoretical Foundation

To understand projectile motion, we start with the basic principles of physics. When an object is launched with an initial velocity \( v_0 \) at an angle \( \theta \), its motion occurs in two dimensions under the influence of gravity: horizontal (x) and vertical (y).

### Equations of Motion
- **Horizontally**, the velocity is constant (assuming no air resistance):
  \[
  x(t) = v_0 \cos(\theta) \cdot t
  \]
- **Vertically**, the motion is affected by gravitational acceleration \( g \):
  \[
  y(t) = v_0 \sin(\theta) \cdot t - \frac{1}{2} g t^2
  \]

### Range Equation
The object hits the ground when \( y = 0 \). Solving for time:
\[
0 = v_0 \sin(\theta) \cdot t - \frac{1}{2} g t^2
\]
This gives \( t = 0 \) (start) or \( t = \frac{2 v_0 \sin(\theta)}{g} \) (landing time). Substituting into the horizontal equation:
\[
R = v_0 \cos(\theta) \cdot \frac{2 v_0 \sin(\theta)}{g} = \frac{v_0^2 \cdot 2 \sin(\theta) \cos(\theta)}{g}
\]
Using the trigonometric identity \( 2 \sin(\theta) \cos(\theta) = \sin(2\theta) \):
\[
R = \frac{v_0^2 \sin(2\theta)}{g}
\]

### Family of Solutions
This equation depends on parameters like \( v_0 \) (initial velocity) and \( g \) (gravitational acceleration). Variations in these parameters produce a family of solutions with different ranges.

## 2. Analysis of the Range

### Dependence on Angle
The range \( R = \frac{v_0^2 \sin(2\theta)}{g} \) shows that \( R \) depends on \( \theta \). The term \( \sin(2\theta) \) reaches its maximum when \( 2\theta = 90^\circ \), i.e., \( \theta = 45^\circ \) (since \( \sin(90^\circ) = 1 \)). Thus, the maximum range is:
\[
R_{max} = \frac{v_0^2}{g}
\]

### Influence of Parameters
- Increasing \( v_0 \) increases the range proportionally to \( v_0^2 \).
- Increasing \( g \) decreases the range inversely (e.g., on the Moon, where \( g \) is smaller, the range increases).

## 3. Practical Applications

### Real-World Scenarios
- **Uneven Terrain**: If launched from a height (\( y_0 \neq 0 \)), the range equation changes.
- **Air Resistance**: In reality, air resistance reduces velocity and range, requiring differential equations for modeling.
- **Sports and Engineering**: This model applies to soccer kicks, rocket launches, and more.

## 4. Implementation

Below is a Python script to simulate and visualize the range as a function of the angle of projection:

```python
import numpy as np
import matplotlib.pyplot as plt

# Parameters
v0 = 20.0  # Initial velocity (m/s)
g = 9.81   # Gravitational acceleration (m/s^2)
angles = np.linspace(0, 90, 91)  # Angles from 0° to 90° (degrees)

# Range calculation (convert angles to radians)
radians = np.deg2rad(angles)
range_values = (v0**2 * np.sin(2 * radians)) / g

# Plot
plt.figure(figsize=(10, 6))
plt.plot(angles, range_values, label=f'v0 = {v0} m/s, g = {g} m/s²')
plt.xlabel('Angle (degrees)')
plt.ylabel('Range (m)')
plt.title('Range vs. Projection Angle')
plt.grid(True)
plt.legend()
plt.show()

# Find maximum range and corresponding angle
max_range = np.max(range_values)
max_angle = angles[np.argmax(range_values)]
print(f"Maximum range: {max_range:.2f} m, Angle: {max_angle}°")