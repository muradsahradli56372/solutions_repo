# Problem 1: Equivalent Resistance Using Graph Theory

## Motivation

Calculating equivalent resistance is a key task in electrical circuit analysis. For simple networks, series and parallel rules are sufficient. However, for more complex circuits involving nested loops or multiple branches, these rules become tedious.

Graph theory offers a powerful and flexible solution: circuits can be modeled as **weighted graphs** where:

- **Nodes** represent connection points (junctions),
- **Edges** represent resistors with weights based on resistance values.

This method enables algorithmic simplification and automated analysis, particularly useful in large-scale simulations and electrical network design.

---

## 1. Problem Approach with Graph Theory

### 1.1 Circuit as a Graph

- A circuit is modeled as a graph $G(V,E)$.
- Each edge has an associated resistance $R$, converted to conductance $G = 1/R$.
- We use the **Laplacian matrix** of the graph to solve Kirchhoff’s current law equations.

---

## 2. Implementation (Python with NetworkX)

### 2.1 Kirchhoff-Based Resistance Solver

```python
import numpy as np
import networkx as nx

# Function to compute equivalent resistance using graph Laplacian
def equivalent_resistance(graph, node_a, node_b):
    G_matrix = nx.laplacian_matrix(graph, weight='conductance').toarray()
    n = len(G_matrix)

    b = np.zeros(n)
    b[node_a] = 1
    b[node_b] = -1

    try:
        potentials = np.linalg.lstsq(G_matrix, b, rcond=None)[0]
        current = potentials[node_a] - potentials[node_b]
        R_eq = 1.0 / current
        return abs(R_eq)
    except Exception as e:
        return f"Error: {e}"
```

## 2.2 Example Graph: Mixed Series and Parallel Configuration

### Create a circuit graph
G = nx.Graph()
G.add_nodes_from([0, 1, 2])

### Add resistors as edges with conductance attributes
G.add_edge(0, 1, resistance=2.0, conductance=1/2.0)
G.add_edge(1, 2, resistance=3.0, conductance=1/3.0)
G.add_edge(0, 2, resistance=6.0, conductance=1/6.0)

### Compute equivalent resistance between nodes 0 and 2
req = equivalent_resistance(G, 0, 2)
print(f"Equivalent resistance between node 0 and 2: {req:.3f} Ohm")

Equivalent resistance between node 0 and 2: 0.367 Ohm

## 3. Detailed Example Explanation

Let us consider a simple resistive circuit modeled as a graph with three nodes: **0**, **1**, and **2**. The circuit includes the following resistive components:

- A **2 Ω resistor** between nodes **0** and **1**
- A **3 Ω resistor** between nodes **1** and **2**
- A **6 Ω resistor** directly between nodes **0** and **2**

![alt text](<output (3).png>)

Visually, this forms a **triangle-shaped circuit**, also known as a **3-node loop or cycle**. In traditional circuit analysis, one would manually analyze each possible series and parallel combination:

- The **path from 0 to 2** can occur either **directly** through the 6 Ω resistor,  
- or **indirectly** via the two resistors in **series**: 0 → 1 → 2, which sums to 2 Ω + 3 Ω = **5 Ω**.

This makes the problem a combination of **parallel and series** resistor relationships.

Using **graph theory**, we model the system as follows:

- Each **resistor** becomes an **edge** in an undirected graph.
- The **weight** of each edge is the **conductance** (inverse of resistance): $G = \frac{1}{R}$.
- We construct a **Laplacian matrix** $L$ based on the connectivity and weights.
- Kirchhoff’s current law (KCL) is applied implicitly through matrix equations:  
  $$
  L \cdot \vec{V} = \vec{b}
  $$
  where $\vec{b}$ encodes potential differences between the source and destination nodes.

By solving this system, we obtain the **potential at each node**, and from this, compute the **net current** between the input and output nodes. The **equivalent resistance** is then simply the **inverse of the current**, assuming a 1-volt potential difference was applied.

This approach abstracts away manual rule application and instead applies consistent, scalable linear algebraic computation.

---

## 4. Advantages of Graph Theory in Circuit Analysis

Graph theory provides a mathematically elegant and computationally scalable method for solving circuit problems, especially when traditional analysis becomes cumbersome.

### 🔍 Generalization

- **Works on any circuit** regardless of complexity, topology, or symmetry.
- Supports **nested combinations** of resistors, even with multiple loops and branches.
- Handles **nontrivial configurations** like bridge networks, mesh circuits, and Wheatstone bridges.

### ⚙️ Automation & Scalability

- Perfectly suited for implementation in **circuit simulation software**.
- Once the graph is defined, **no need for manual simplification** or error-prone rule tracking.
- Can be combined with optimization tools for **automated component tuning**.

### 🧠 Educational Insight

- Reveals the **deep mathematical structure** of electrical networks.
- Links electrical engineering with **linear algebra**, **spectral graph theory**, and **numerical methods**.
- Encourages algorithmic thinking in solving traditionally “manual” problems.

### 🔄 Reusability & Modularity

- Graph-based methods can easily be extended to:
  - AC circuits (using complex impedances),
  - Controlled sources,
  - Power flow models.

In short, graph theory enables **robust**, **modular**, and **mathematically consistent** treatment of complex circuits.

---

## 5. Future Extensions and Project Ideas

The basic implementation shown here opens the door to a wide range of practical and educational enhancements. Here are some ideas to extend the project into a fully functional analysis tool:

### 🖥️ 1. Graphical User Interface (GUI)

- Allow users to **draw circuits interactively**.
- Real-time updates of equivalent resistance as components are added/removed.
- Frameworks: `Tkinter`, `PyQt`, or web-based (`Dash`, `Streamlit`).

### 🧮 2. Visual Circuit Diagram Generation

- Use `matplotlib`, `graphviz`, or `networkx.draw()` to show:
  - Nodes as terminals,
  - Edges as labeled resistors,
  - Node labels and current paths.

### ⚡ 3. Support for Active Components

- Extend to circuits containing:
  - **Voltage sources** (V),
  - **Current sources** (I),
  - **Dependent sources**.
- Use **Modified Nodal Analysis (MNA)** for full support.

### 📡 4. Frequency-Domain Analysis

- Add **complex impedance** handling:
  - Capacitors: $Z_C = \frac{1}{j\omega C}$
  - Inductors: $Z_L = j\omega L$
- Enables full AC analysis (phasor domain).

### 🔌 5. SPICE Integration

- Export graph-defined circuits into **SPICE netlists**.
- Analyze the same circuits using industry-standard simulation tools.

### 📈 6. Step-by-Step Reduction Logging

- Print or visualize how:
  - Series resistors are combined,
  - Parallel resistors are merged,
  - Nodes collapse as simplifications are applied.
- Great for **educational feedback** and **debugging complex reductions**.

---

By implementing these extensions, the project can grow from a basic tool into a powerful, scalable framework for **modern circuit analysis**, integrating concepts from **physics**, **computer science**, and **electrical engineering**.



