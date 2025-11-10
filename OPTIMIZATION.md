# Hamiltonian in Optimization Problems

## Overview

Hamiltonians play a fundamental role in quantum optimization algorithms. They encode the problem to be solved and guide the quantum computer toward optimal solutions. This document explores how Hamiltonians are constructed from optimization problems and their role in quantum algorithms.

---

## Problem Encoding

### From Classical Problem to Hamiltonian

Every combinatorial optimization problem can be mapped to a Hamiltonian whose ground state encodes the solution:

```
Classical Problem  →  Cost Function  →  Hamiltonian  →  Ground State
  (Minimize f(x))       (f(x))           (Ĥ_cost)        (Energy = min)
```

### Cost Function Mapping

For an optimization problem:
```
Minimize: f(x), where x ∈ {0,1}ⁿ
```

Create Hamiltonian:
```
Ĥ_cost = ∑ᵢ f_i(Z_i)  or  ĤProb = ∑ᵢ h_i Z_i + ∑_{i<j} J_{ij} Z_i Z_j
```

Where:
- Z_i eigenvalues ±1 represent x_i ∈ {0,1}
- Ground state |ψ₀⟩ has eigenvalue corresponding to minimum cost
- Measuring in computational basis gives optimal solution

---

## QAOA: Quantum Approximate Optimization Algorithm

### Concept

QAOA uses a time-evolving Hamiltonian with two components:
```
Ĥ(t) = Ĥ_cost(t) + Ĥ_mixer(t)
```

### Components

#### 1. Cost Hamiltonian

```
Ĥ_cost = ∑ᵢ h_i Z_i + ∑_{i<j} J_{ij} Z_i Z_j
```

Properties:
- Encodes the problem to solve
- Commutative: [Ĥ_cost, Z_i] depends on problem structure
- Ground state has energy = optimal value

#### 2. Mixer Hamiltonian

```
Ĥ_mixer = ∑ᵢ X_i
```

Properties:
- Allows transitions between computational basis states
- Enables exploration of solution space
- Non-commutative with cost Hamiltonian

### QAOA Circuit

```
|+⟩ ⊗ |+⟩ ... |+⟩  →  e^{-iĤ_cost γ}  →  e^{-iĤ_mixer β}  →  ... → Measure
        (preparation)     (p times)        (p times)
```

Parameters γ and β are optimized classically.

### Algorithm Steps

1. **Initialization**: Prepare uniform superposition |+⟩ ⊗ⁿ
2. **Problem Evolution**: Apply e^{-iĤ_cost γ}
3. **Mixer Evolution**: Apply e^{-iĤ_mixer β}
4. **Repetition**: Repeat p times (depth)
5. **Measurement**: Measure qubits in computational basis
6. **Classical Update**: Use measured values to update γ, β
7. **Iteration**: Repeat until convergence

### Mathematical Framework

```
⟨ψ(γ,β)|Ĥ_cost|ψ(γ,β)⟩ = expected cost
```

Optimize over (γ, β) parameters to maximize:
```
C(γ,β) = ⟨ψ(γ,β)|Ĥ_cost|ψ(γ,β)⟩
```

---

## Variational Quantum Eigensolver (VQE)

### Concept

VQE uses a parameterized circuit to find the ground state energy of a Hamiltonian:

```
E_min = min_{θ} ⟨ψ(θ)|Ĥ|ψ(θ)⟩
```

### Key Components

#### Ansatz (Parameterized Circuit)

```python
|ψ(θ)⟩ = U(θ₁,θ₂,...,θₙ) |0⟩ ⊗ n
```

Common choices:
- Hardware-efficient ansatz (HEA)
- UCC (Unitary Coupled Cluster) for chemistry
- Problem-inspired ansatz

#### Cost Function

```
f(θ) = ⟨ψ(θ)|Ĥ|ψ(θ)⟩
```

Calculated by:
- Measuring expectation value of Ĥ
- Decomposing Ĥ into Pauli measurements
- Averaging results

#### Classical Optimizer

Optimizes θ using gradient-free or gradient-based methods:
- COBYLA
- SLSQP
- ADAM
- Powell

---

## Hamiltonian Encoding of Common Problems

### 1. MAX-CUT Problem

**Problem**: Partition graph vertices to maximize edges between partitions

**Hamiltonian**:
```
Ĥ = -∑_{(i,j)∈E} Z_i Z_j / 2
```

Ground state energy encodes maximum cut.

### 2. 3-SAT Problem

**Problem**: Satisfy all clauses using boolean assignments

**Hamiltonian**:
```
Ĥ = ∑_c (1 - C_c) / 2
```

Where C_c represents clause c in Pauli operators.

### 3. Graph Coloring

**Problem**: Color vertices such that adjacent vertices have different colors

**Hamiltonian**:
```
Ĥ = ∑_{(i,j)∈E} (1 - Z_i Z_j) / 2
```

Penalizes same color on adjacent vertices.

### 4. Quadratic Unconstrained Binary Optimization (QUBO)

**Problem**: 
```
Minimize: ∑_{i,j} Q_{ij} x_i x_j, x_i ∈ {0,1}
```

**Hamiltonian**:
```
Ĥ_QUBO = ∑_{i,j} Q_{ij} (1 - Z_i)/2 (1 - Z_j)/2
```

Direct mapping from problem coefficients to Hamiltonian terms.

---

## Adiabatic Quantum Computing

### Concept

Smooth time-evolution from initial to problem Hamiltonian:
```
Ĥ(s) = (1-s)Ĥ_init + s·Ĥ_problem,  s ∈ [0,1]
```

### Process

1. **Start**: System in ground state of Ĥ_init (usually Ĥ_mixer)
2. **Evolve**: Slowly increase s (adiabatic evolution)
3. **End**: System in ground state of Ĥ_problem
4. **Measure**: Ground state encodes solution

### Adiabatic Condition

For adiabatic evolution to succeed:
```
T >> 1/Δ²
```

Where:
- T: Total evolution time
- Δ: Minimum energy gap during evolution

### Energy Gap Importance

```
Ĥ_gap = E₁(s) - E₀(s)
```

Small energy gap requires longer evolution time.

---

## Quantum Annealing

### Hardware Implementation

Devices like D-Wave implement:
```
Ĥ(t) = A(t)Ĥ_mixer + B(t)Ĥ_problem
```

Where A(t) and B(t) are annealing schedules.

### Process

1. **Initialization**: Strong mixer, weak problem Hamiltonian
2. **Annealing**: Gradually increase problem strength
3. **Read-out**: Measure final state
4. **Solution**: Computational basis state encodes answer

### Advantage

- Hardware-native optimization
- Can tunnel through energy barriers
- Effective for specific problem classes

---

## Constructing Problem Hamiltonians

### General Approach

```python
from qiskit.quantum_info import SparsePauliOp
import numpy as np

# Define problem parameters
n_qubits = 3
h = [1, 0.5, 0.3]  # Local fields
J = {(0,1): 1.0, (1,2): 0.5}  # Couplings

# Build Hamiltonian
pauli_list = []
coeffs = []

# Add local field terms
for i in range(n_qubits):
    pauli = 'I' * (n_qubits - i - 1) + 'Z' + 'I' * i
    pauli_list.append(pauli)
    coeffs.append(h[i])

# Add coupling terms
for (i, j), strength in J.items():
    pauli = ['I'] * n_qubits
    pauli[i] = 'Z'
    pauli[j] = 'Z'
    pauli_list.append(''.join(pauli))
    coeffs.append(strength)

# Create Hamiltonian
H = SparsePauliOp(pauli_list, coeffs)
H_matrix = H.to_matrix()
```

### Example: MAX-CUT on Triangle Graph

```python
# Graph: Triangle (3-cycle)
# Edges: (0,1), (1,2), (2,0)

H_maxcut = SparsePauliOp(
    ['ZZ', 'ZZ', 'ZZ'],
    [-1.0, -1.0, -1.0],
    num_qubits=3
)

# Ground state = state with maximum edges cut
```

---

## Performance Analysis

### Approximation Ratio

For QAOA:
```
r = ⟨ψ(γ*,β*)|Ĥ_cost|ψ(γ*,β*)⟩ / E_opt
```

Measures solution quality relative to optimal.

### Depth Requirements

- **p = 1**: O(1) approximation for some problems
- **p = polylog(n)**: Better approximation
- **p = poly(n)**: Near-classical accuracy

### Quantum Advantage

Achieved when:
1. Problem encoded in problem Hamiltonian
2. Quantum evolution explores efficiently
3. Measurement extracts solution reliably

---

## Implementation Challenges

### 1. Parameter Optimization

**Challenge**: Landscape has many local minima
**Solution**:
- Warm-starting from previous depths
- Using better classical optimizers
- Problem-specific initialization

### 2. Noise and Errors

**Challenge**: Gate errors reduce solution quality
**Solution**:
- Error mitigation techniques
- Shorter circuits (fewer gates)
- Noise-aware optimization

### 3. Classical Simulation Limits

**Challenge**: Classical simulation becomes intractable
**Threshold**: ~15-20 qubits for naive simulation

---

## Applications

### Industry Problems

| Application | Hamiltonian | Expected Benefit |
|-------------|------------|------------------|
| Portfolio Optimization | QUBO Hamiltonian | Faster convergence |
| Drug Discovery | Electronic Hamiltonian | Energy calculations |
| Supply Chain | Combinatorial Ĥ | Better routing |
| Scheduling | Assignment Ĥ | Reduced wait time |
| Machine Learning | Feature Ĥ | Faster training |

### Research Areas

- Quantum error correction using problem Hamiltonians
- Variational optimization for chemistry
- Hybrid classical-quantum algorithms
- Machine learning applications

---

## Future Directions

### Quantum Advantage

```
Quantum Speedup = Classical Time / Quantum Time
```

Expected for:
- Specific optimization landscapes
- Large-scale combinatorial problems
- Applications with quantum structure

### Hardware Improvements

- Reduced error rates
- More qubits and longer coherence
- Native multi-qubit gates
- Better calibration

### Algorithm Development

- Better ansätze and mixers
- Adaptive circuit construction
- Problem-specific encoding
- Hybrid approaches

---

## Summary

Hamiltonians are central to quantum optimization:

1. **Encoding**: Problem maps to Hamiltonian ground state energy
2. **Algorithms**: QAOA, VQE, adiabatic quantum computing use different Hamiltonian strategies
3. **Implementation**: Decompose into quantum gates and measurements
4. **Advantage**: Quantum can explore optimization landscapes more efficiently
5. **Challenge**: Requires careful problem encoding and parameter tuning

Mastering Hamiltonian-based optimization is essential for practical quantum computing applications.
