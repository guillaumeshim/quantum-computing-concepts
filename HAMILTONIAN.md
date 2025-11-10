# Hamiltonian in Quantum Mechanics

## Overview

The Hamiltonian is one of the most fundamental concepts in quantum mechanics. It is an operator that corresponds to the total energy of a quantum system and plays a central role in the time evolution of quantum states.

### Definition

The Hamiltonian (Ĥ) is the energy operator of a quantum system:
```
Ĥ = T̂ + V̂
```

Where:
- **T̂**: Kinetic energy operator
- **V̂**: Potential energy operator (interaction term)

---

## Mathematical Formulation

### Time-Independent Schrödinger Equation

```
Ĥ |ψ⟩ = E |ψ⟩
```

This is an eigenvalue equation where:
- **|ψ⟩**: Quantum state (eigenstate)
- **E**: Energy eigenvalue
- **Ĥ**: Hamiltonian operator

The eigenvalues are the possible energy values that can be measured.
The eigenstates are the states with definite energy.

### Time-Dependent Schrödinger Equation

```
iℏ ∂|ψ(t)⟩/∂t = Ĥ |ψ(t)⟩
```

This describes how a quantum state evolves in time under the influence of the Hamiltonian.

---

## Hamiltonian Components

### 1. Kinetic Energy Term

For a particle with momentum p:
```
T̂ = p̂²/(2m) = -ℏ²/(2m) ∇²
```

Where:
- **m**: Particle mass
- **∇²**: Laplacian operator
- **ℏ**: Reduced Planck constant

### 2. Potential Energy Term

For various physical systems:

#### Harmonic Oscillator
```
V̂ = (1/2)mω²x²
```
Common in vibrations and quantum optics.

#### Coulomb Potential (Hydrogen Atom)
```
V̂ = -e²/(4πε₀r)
```
Describes electron-nucleus interaction.

#### Magnetic Field Interaction
```
V̂ = -μ·B
```
Interaction of magnetic moment with external field.

---

## Properties of Hamiltonians

### 1. Hermitian Operator

The Hamiltonian must be Hermitian (self-adjoint):
```
Ĥ† = Ĥ
```

This ensures:
- Real eigenvalues (energy levels are real)
- Orthogonal eigenstates
- Unitary time evolution

### 2. Commutation Relations

For observables and Hamiltonian:
```
[Â, Ĥ] = 0  ⟹  ⟨A⟩ is conserved (constant in time)
```

If an observable commutes with H, it is a constant of motion.

### 3. Energy Spectrum

The eigenvalues form the energy spectrum:
```
E₀ ≤ E₁ ≤ E₂ ≤ ...
```

Where:
- **E₀**: Ground state energy (lowest energy)
- **Eₙ**: Energy of n-th excited state

---

## Common Hamiltonians in Quantum Computing

### 1. Ising Model Hamiltonian

```
Ĥ = -∑ᵢ hᵢZᵢ - ∑⟨ᵢ,ⱼ⟩ Jᵢⱼ ZᵢZⱼ
```

Components:
- **hᵢ**: Local field strength on qubit i
- **Jᵢⱼ**: Coupling strength between qubits i and j
- **Zᵢ**: Pauli-Z operator on qubit i

Applications:
- Optimization problems
- Spin glass models
- Phase transitions

### 2. XY Model Hamiltonian

```
Ĥ = ∑⟨ᵢ,ⱼ⟩ (Jˣᵢⱼ XᵢXⱼ + Jʸᵢⱼ YᵢYⱼ)
```

Characteristics:
- XY coupling instead of Z coupling
- Anisotropic interactions
- Used in quantum simulation

### 3. Heisenberg Model Hamiltonian

```
Ĥ = ∑⟨ᵢ,ⱼ⟩ (JˣᵢⱼXᵢXⱼ + JʸᵢⱼYᵢYⱼ + Jᶻᵢⱼ ZᵢZⱼ) - ∑ᵢ hᵢZᵢ
```

Features:
- Full interaction in all three directions
- Describes magnetic systems
- Fundamental in condensed matter

### 4. Transverse Field Ising Model (TFIM)

```
Ĥ = -∑ᵢ Zᵢ - Γ∑ᵢ Xᵢ
```

Where **Γ** is the transverse field strength.

Importance:
- Studies quantum phase transitions
- Used in quantum annealing
- Basis for adiabatic quantum computing

---

## Hamiltonian Simulation

### Purpose

Hamiltonian simulation is the process of implementing the time evolution operator:
```
U(t) = e^(-iĤt/ℏ)
```

This allows us to evolve quantum states according to the Hamiltonian.

### Methods

#### 1. Trotter-Suzuki Decomposition

```
e^(-iĤt/ℏ) ≈ [e^(-iV̂Δt/ℏ) e^(-iT̂Δt/ℏ)]^n
```

Breaks down complex evolution into simpler steps.

#### 2. Product Formula

```
e^(-i(A+B)t) ≈ e^(-iAt) e^(-iBt)  (first-order)
e^(-iAt/2) e^(-iBt) e^(-iAt/2)      (second-order, more accurate)
```

#### 3. Linear Combination of Unitaries (LCU)

Decomposes Hamiltonian into sum of unitary operations:
```
Ĥ = ∑ₖ αₖ Ûₖ
```

---

## Time Evolution

### Schrödinger Picture

State evolves, operators are fixed:
```
|ψ(t)⟩ = U(t) |ψ(0)⟩ = e^(-iĤt/ℏ) |ψ(0)⟩
```

### Heisenberg Picture

Operators evolve, state is fixed:
```
Â_H(t) = U†(t) Â_S U(t) = e^(iĤt/ℏ) Â_S e^(-iĤt/ℏ)
```

### Expectation Value Evolution

```
d⟨A⟩/dt = (i/ℏ)⟨[Ĥ, Â]⟩ + ⟨∂Â/∂t⟩
```

If A doesn't explicitly depend on time and commutes with H:
```
d⟨A⟩/dt = 0  (constant of motion)
```

---

## Eigenstate and Eigenenergy Computation

### Ground State

The eigenvector corresponding to minimum eigenvalue:
```
Ĥ |ψ₀⟩ = E₀ |ψ₀⟩
```

Ground state energy E₀ is the lowest possible energy.

### Excited States

Eigenvectors with higher eigenvalues:
```
Ĥ |ψₙ⟩ = Eₙ |ψₙ⟩,  where E₀ < E₁ < E₂ < ...
```

### Energy Gap

```
ΔE = E₁ - E₀
```

Important for:
- Quantum phase transitions
- Adiabatic quantum computing
- Energy scales of systems

---

## Hamiltonian Examples in Different Systems

### 1. Particle in a Box

```
Ĥ = p̂²/(2m)

Energy levels: Eₙ = n²π²ℏ²/(2mL²), n = 1,2,3,...
```

### 2. Quantum Harmonic Oscillator

```
Ĥ = p̂²/(2m) + (1/2)mω²x̂²

Energy levels: Eₙ = ℏω(n + 1/2), n = 0,1,2,...
```

### 3. Two-Level System (Qubit)

```
Ĥ = (ℏω/2)Z = (ℏω/2)[[1, 0], [0, -1]]

Energy levels: E₊ = ℏω/2, E₋ = -ℏω/2
```

---

## Hamiltonian in Quantum Computing

### Single Qubit Hamiltonian

```
Ĥ = αX + βY + γZ
```

Where α, β, γ are coupling strengths.

### Multi-Qubit Hamiltonian

```
Ĥ = ∑ᵢ hᵢZᵢ + ∑⟨ᵢ,ⱼ⟩ Jᵢⱼ ZᵢZⱼ + ∑ᵢ ΓᵢXᵢ
```

Terms:
- First sum: Local fields
- Second sum: Two-body interactions
- Third sum: Transverse fields

### Constructing Hamiltonians

```python
from qiskit.quantum_info import SparsePauliOp
import numpy as np

# Create Hamiltonian from Pauli strings
H = SparsePauliOp(['ZZ', 'XX', 'YY'], 
                   coeffs=[1.0, 0.5, 0.3])

# Matrix representation
H_matrix = H.to_matrix()

# Eigenvalues and eigenvectors
eigenvalues, eigenvectors = np.linalg.eigh(H_matrix)
```

---

## Conservation Laws and Symmetries

### Noether's Theorem

Every continuous symmetry corresponds to a conserved quantity:
- Time translation symmetry → Energy conservation (H is conserved)
- Spatial translation → Momentum conservation
- Rotation → Angular momentum conservation

### Symmetry Operators

If [Ŝ, Ĥ] = 0, then S is a conserved quantity:
```
Ĥ |ψ⟩ = E |ψ⟩  and  Ŝ |ψ⟩ = s |ψ⟩
```

Joint eigenstates exist with definite energy and symmetry eigenvalue.

---

## Physical Interpretation

### Energy as Observable

- Hamiltonian eigenvalues are measurable energy values
- Probability of measuring energy E: |⟨E|ψ⟩|²
- Average energy: ⟨Ĥ⟩ = ⟨ψ|Ĥ|ψ⟩

### Stability and Ground State

- System naturally evolves toward ground state
- Ground state is most stable configuration
- Excited states decay through energy release

---

## Hamiltonian in Different Domains

| Domain | Hamiltonian | Key Feature |
|--------|------------|-------------|
| Quantum Computing | Ising/QAOA | Optimization |
| Quantum Chemistry | Electronic Hamiltonian | Molecular properties |
| Solid State | Lattice Hamiltonian | Collective phenomena |
| Quantum Optics | Jaynes-Cummings | Light-matter interaction |
| Atomic Physics | Fine/Hyperfine structure | Energy levels |

---

## Summary

The Hamiltonian is:
1. **Generator of time evolution**: Determines how states change
2. **Energy operator**: Its eigenvalues are possible measured energies
3. **Observable**: Can be measured via energy measurements
4. **Fundamental tool**: Central to all quantum mechanics
5. **Foundation for optimization**: Basis of quantum optimization algorithms

Understanding Hamiltonians is essential for quantum computing, simulation, and optimization.
