# Bloch Sphere: Visualizing Single-Qubit States

## Overview

The Bloch sphere is a geometric representation of a single-qubit quantum state. It provides a visual way to understand single-qubit quantum mechanics and the effects of quantum gates on qubit states.

### Bloch Sphere Visualization

![Bloch Sphere]([https://user-images.githubusercontent.com/quantum-diagrams/bloch-sphere-diagram.png](https://www.google.com/url?sa=i&url=https%3A%2F%2Fpublish.obsidian.md%2Fmyquantumwell%2FQuantum%2BMechanics%2FQuantum%2BInformation%2FBloch%2Bspheres&psig=AOvVaw1rOM3HunsL2QOaLwff2Yc4&ust=1762895551500000&source=images&cd=vfe&opi=89978449&ved=0CBUQjRxqFwoTCNCCnvq_6JADFQAAAAAdAAAAABAE))

*Bloch sphere showing quantum state |ψ⟩ represented by angles θ (polar) and φ (azimuthal). The computational basis states |0⟩, |1⟩ and superposition states are marked at key positions.*

### Key Points
- A single-qubit state is represented as a point on the surface of a unit sphere
- The sphere has radius 1 and is centered at the origin
- Pure quantum states correspond to points on the surface
- Mixed states correspond to points inside the sphere

---

## Bloch Sphere Coordinates

### Mathematical Representation

A single-qubit quantum state can be written as:
```
|ψ⟩ = cos(θ/2)|0⟩ + e^(iφ)sin(θ/2)|1⟩
```

Where:
- **θ** (theta): Polar angle from the north pole (range: 0 to π)
- **φ** (phi): Azimuthal angle in the xy-plane (range: 0 to 2π)
- **e^(iφ)**: Global phase factor

### Cartesian Coordinates on Bloch Sphere

The position on the Bloch sphere in Cartesian coordinates:
```
x = sin(θ)cos(φ)    [X-axis: Pauli-X expectation]
y = sin(θ)sin(φ)    [Y-axis: Pauli-Y expectation]
z = cos(θ)          [Z-axis: Pauli-Z expectation]
```

---

## Bloch Sphere Diagram

### 3D Representation (ASCII Art)

```
         |Z (|0⟩ pole)
         ↑
         |
         *----------- |0⟩ state (0, 0, 1)
        /|\  \         
       / | \  \       
      /  |  \  \      
     /   θ   \  \     
    /    |    \  \    
   /     |     \  \   
  /      |      \  \  
 +-------+-------+-------+ X-axis
 |      |       |\      
 |      |       | \     
 |      |       |  \    
 |      |Y      |φ  \   
 |      ↑       |     \  
 |  |1⟩*       |(-)|+⟩ * 
 |              |        

```

### Key Points on Bloch Sphere

| State | Position | Coordinates | Meaning |
|-------|----------|-------------|----------|
| \|0⟩ | North Pole | (0, 0, 1) | Ground state, eigenstate of Z |
| \|1⟩ | South Pole | (0, 0, -1) | Excited state, eigenstate of -Z |
| \|+⟩ | +X axis | (1, 0, 0) | Superposition, eigenstate of X |
| \|-⟩ | -X axis | (-1, 0, 0) | Superposition, eigenstate of -X |
| \|+i⟩ | +Y axis | (0, 1, 0) | Superposition, eigenstate of Y |
| \|-i⟩ | -Y axis | (0, -1, 0) | Superposition, eigenstate of -Y |

---

## Common Quantum States and Their Positions

### Computational Basis States

```python
# |0⟩ state: North pole
θ = 0, φ = arbitrary
Position: (0, 0, 1)
```

```
         ☆ |0⟩
        /|\
       / | \
      /  |  \
     /   |   \
    ------+------
         |     
        \\ |    
         \\ |   
          \|   
           ☆ |1⟩
```

### Hadamard Superposition

```python
# |+⟩ = (|0⟩ + |1⟩) / √2
θ = π/2, φ = 0
Position: (1, 0, 0)
```

### Y-basis Superposition

```python
# |+i⟩ = (|0⟩ + i|1⟩) / √2
θ = π/2, φ = π/2
Position: (0, 1, 0)
```

---

## Quantum Gate Operations as Rotations

Quantum gates perform rotations on the Bloch sphere. Each rotation is characterized by an axis and an angle.

### Pauli Gates

#### Pauli-X (NOT gate)
- **Rotation**: 180° around X-axis
- **Effect**: Swaps |0⟩ and |1⟩
- **Matrix**: [[0, 1], [1, 0]]

```
Before:           After:
     |0⟩               |1⟩
      ↑                 ↓
  ----+----  →  ----+---- (180° flip)
```

#### Pauli-Y (Y gate)
- **Rotation**: 180° around Y-axis
- **Effect**: Combines X and Z operations
- **Matrix**: [[0, -i], [i, 0]]

#### Pauli-Z (Phase gate)
- **Rotation**: 180° around Z-axis
- **Effect**: Applies phase -1 to |1⟩
- **Matrix**: [[1, 0], [0, -1]]

### Hadamard Gate
- **Rotation**: 90° around Y-axis, then 180° around X-axis
- **Effect**: Maps |0⟩ → |+⟩, |1⟩ → |-⟩
- **Geometric**: Creates equal superposition

```
|0⟩        Hadamard      |+⟩
 ↑  ════════════════→  /
 |                    /|\  
 |                   / | \ 
+-+-+               +--+--+
       \         /        |
        \       /         |
         \     /         ↓
          \   /         |-⟩
           \ /
            ✗
```

### Rotation Gates

#### RX(θ): Rotation around X-axis
```python
# Rotates state by angle θ around X-axis
# Effect: Turns the state vector around X-axis
# Matrix: [[cos(θ/2), -i*sin(θ/2)],
#          [-i*sin(θ/2), cos(θ/2)]]
```

#### RY(θ): Rotation around Y-axis
```python
# Rotates state by angle θ around Y-axis
# Effect: Most direct path between any two states
# Matrix: [[cos(θ/2), -sin(θ/2)],
#          [sin(θ/2), cos(θ/2)]]
```

#### RZ(θ): Rotation around Z-axis
```python
# Rotates state by angle θ around Z-axis
# Effect: Changes relative phase between |0⟩ and |1⟩
# Matrix: [[e^(-iθ/2), 0],
#          [0, e^(iθ/2)]]
```

---

## Bloch Sphere Visualization Examples

### Example 1: Hadamard Rotation

```
Initial state |0⟩:
    |0⟩
    ▲ 
    |
----+----→  (1,0,0) gives |+⟩ after Hadamard
    |
    |
    |1⟩
    ▼
```

### Example 2: Measuring along X-axis (Hadamard basis)

```
State |+⟩ = (|0⟩ + |1⟩)/√2:

         |0⟩
         ▲
         |  
    -----●----- |+⟩ (measurement gives 50-50)
    /    |    \\
   /     |     \\
  |1⟩    |     |-⟩
   \\     |     /
    \\    |    /
         ▼
        |1⟩
```

### Example 3: Bloch Sphere Path for Bell State Preparation

```
Preparing Bell state |Φ+⟩ = (|00⟩ + |11⟩)/√2 involves:

Qubit 0:
  |0⟩ → Hadamard → |+⟩
  
Qubit 1:
  |0⟩ → CNOT with qubit 0 → entangled state
```

---

## Mixed States and the Bloch Ball

### Pure vs Mixed States

- **Pure States**: Points exactly on the Bloch sphere surface
- **Mixed States**: Points inside the Bloch sphere
- **Completely Mixed State**: Center of the sphere (0, 0, 0)
  - Equal mixture of |0⟩ and |1⟩
  - Maximum entropy

### Purity Measure

The distance from origin indicates purity:
```
Purity = (3r² + 1) / 2  where r = distance from center

r = 1: Pure state (on surface)
r = 0: Maximally mixed state (at center)
```

---

## Visualization Tools for Bloch Sphere

### Qiskit Bloch Sphere Visualization

```python
from qiskit import QuantumCircuit, QuantumRegister, ClassicalRegister
from qiskit_aer import AerSimulator
from qiskit.visualization import plot_bloch_multivector
import matplotlib.pyplot as plt

# Create a simple quantum circuit
qc = QuantumCircuit(1)
qc.h(0)  # Apply Hadamard gate

# Get statevector
from qiskit_aer import AerSimulator
from qiskit.quantum_info import Statevector

sv = Statevector.from_instruction(qc)

# Plot Bloch sphere
from qiskit.visualization import plot_bloch_vector
from numpy import array, sqrt

# For |+⟩ state
plot_bloch_vector([1, 0, 0])
plt.show()
```

### Creating Bloch Sphere Diagrams

```python
from qiskit.visualization import plot_bloch_multivector
from qiskit.quantum_info import DensityMatrix
import matplotlib.pyplot as plt

# Create |0⟩ state
state_zero = DensityMatrix.from_label('0')
plot_bloch_multivector(state_zero)
plt.title('Bloch Sphere: |0⟩ State')
plt.show()

# Create |+⟩ state
from qiskit import QuantumCircuit
qc = QuantumCircuit(1)
qc.h(0)
sv = Statevector.from_instruction(qc)
plot_bloch_multivector(sv)
plt.title('Bloch Sphere: |+⟩ State')
plt.show()
```

---

## Measurements on the Bloch Sphere

### Z-basis Measurement
- Measures along Z-axis (|0⟩ vs |1⟩)
- Standard computational basis measurement
- Probability: P(0) = cos²(θ/2), P(1) = sin²(θ/2)

### X-basis Measurement (Hadamard basis)
- Measures along X-axis (|+⟩ vs |-⟩)
- Requires Hadamard before standard measurement
- Probability depends on angle around XY-plane

### Y-basis Measurement
- Measures along Y-axis (|+i⟩ vs |-i⟩)
- Requires S†H before standard measurement
- Rotates measurement axis

---

## Key Insights

1. **State Representation**: Any single-qubit state maps to exactly one point on Bloch sphere
2. **Quantum Gates**: All single-qubit gates are rotations on the Bloch sphere
3. **Measurement**: Measurement projects state onto measurement axis
4. **Superposition**: Superposition states lie on equator (θ = π/2)
5. **Entanglement**: Single-qubit operations affect only individual qubit's point

---

## Advanced Topics

### Multi-qubit Bloch Sphere Representation

For multi-qubit systems (n > 1):
- Cannot use single Bloch sphere
- State space is 2ⁿ-dimensional
- Requires different visualization techniques
- Reduced density matrices can be visualized on Bloch sphere

### Geometric Phase

- Berry phase emerges from closed paths on Bloch sphere
- Relevant for quantum computing and topological quantum states
- Provides geometric interpretation of quantum mechanics

---

## Summary

The Bloch sphere provides an intuitive geometric visualization of single-qubit quantum states and their transformations. Understanding the Bloch sphere is fundamental to quantum computing as it helps visualize quantum gates, measurements, and superposition states.

### Quick Reference

```
State          | Position    | Vector
|0⟩           | North Pole  | (0, 0, 1)
|1⟩           | South Pole  | (0, 0, -1)  
|+⟩ = (|0⟩+|1⟩)/√2 | +X      | (1, 0, 0)
|-⟩           | -X          | (-1, 0, 0)
H|0⟩ = |+⟩   | Equator     | (1, 0, 0)
```
