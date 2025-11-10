# Qiskit: A Quantum Computing Framework

## Overview

Qiskit is an open-source quantum computing software development framework developed by IBM. It provides tools to create quantum circuits, run them on simulators or real quantum hardware, and analyze results.

### Key Features
- **Circuit Building**: Easy-to-use API for constructing quantum circuits
- **Multiple Backends**: Simulators and real quantum processors
- **Optimization**: Pre-built optimization algorithms
- **Machine Learning**: Quantum machine learning components
- **Visualization**: Circuit and result visualization tools

---

## Installation

```bash
pip install qiskit qiskit-ibm-runtime
pip install matplotlib numpy scipy
```

---

## Core Concepts

### 1. Quantum Circuit

A quantum circuit is a sequence of quantum gates and measurements applied to qubits.

#### Basic Example: Bell State

```python
from qiskit import QuantumCircuit, QuantumRegister, ClassicalRegister

# Create a quantum circuit with 2 qubits and 2 classical bits
qc = QuantumCircuit(2, 2)

# Apply Hadamard gate to first qubit
qc.h(0)

# Apply CNOT gate (control: qubit 0, target: qubit 1)
qc.cx(0, 1)

# Measure qubits
qc.measure([0, 1], [0, 1])

# Display circuit
print(qc)
```

Output:
```
     ┌───┐     ┌─┐
q_0: ┤ H ├──■──┤M├───
     └───┘┌─┴─┐└┬┘┌─┐
q_1: ─────┤ X ├─┤M├
          └───┘ └┬┘
c: 2/═══════════════
```

### 2. Basic Quantum Gates

```python
from qiskit import QuantumCircuit
import numpy as np

qc = QuantumCircuit(1)

# Single-qubit gates
qc.x(0)           # Pauli-X (NOT) gate
qc.y(0)           # Pauli-Y gate
qc.z(0)           # Pauli-Z gate
qc.h(0)           # Hadamard gate
qc.s(0)           # Phase gate
qc.t(0)           # T gate
qc.rx(np.pi/4, 0) # Rotation around X-axis
qc.ry(np.pi/4, 0) # Rotation around Y-axis
qc.rz(np.pi/4, 0) # Rotation around Z-axis

print(qc)
```

### 3. Multi-Qubit Gates

```python
qc = QuantumCircuit(2)

# Two-qubit gates
qc.cx(0, 1)  # CNOT (Controlled-X)
qc.cy(0, 1)  # Controlled-Y
qc.cz(0, 1)  # Controlled-Z
qc.swap(0, 1)  # SWAP gate

print(qc)
```

---

## Example 1: Create and Run a Simple Circuit

```python
from qiskit import QuantumCircuit, QuantumRegister, ClassicalRegister
from qiskit_aer import AerSimulator
from qiskit.primitives import Sampler

# Create a quantum circuit
qc = QuantumCircuit(2, 2)
qc.h(0)         # Create superposition
qc.cx(0, 1)     # Entangle qubits
qc.measure([0, 1], [0, 1])

# Create a simulator
simulator = AerSimulator()

# Run the circuit
job = simulator.run(qc, shots=1000)
result = job.result()
counts = result.get_counts(qc)

print("Measurement Results:")
print(counts)
# Output: {'00': 495, '11': 505} (approximately 50-50 split)
```

---

## Example 2: Grover's Algorithm Implementation

```python
from qiskit import QuantumCircuit, QuantumRegister, ClassicalRegister
from qiskit_aer import AerSimulator
import numpy as np

def grover_circuit(n_qubits, marked_state):
    """
    Implements Grover's algorithm for searching a marked state.
    
    Args:
        n_qubits: Number of qubits
        marked_state: Binary string representing the target state
    
    Returns:
        QuantumCircuit: Grover's algorithm circuit
    """
    qc = QuantumCircuit(n_qubits, n_qubits)
    
    # Step 1: Initialize superposition
    for i in range(n_qubits):
        qc.h(i)
    
    # Step 2: Grover iterations
    iterations = int(np.pi/4 * np.sqrt(2**n_qubits))
    
    for _ in range(iterations):
        # Oracle: Mark the target state
        oracle = create_oracle(n_qubits, marked_state)
        qc.append(oracle, range(n_qubits))
        
        # Diffusion operator
        qc = diffusion_operator(qc, n_qubits)
    
    # Step 3: Measure
    qc.measure(range(n_qubits), range(n_qubits))
    
    return qc

def create_oracle(n_qubits, marked_state):
    """
    Creates an oracle that marks the target state.
    """
    oracle = QuantumCircuit(n_qubits)
    
    # Apply X gates to flip qubits where marked_state has '0'
    for i, bit in enumerate(marked_state):
        if bit == '0':
            oracle.x(i)
    
    # Multi-controlled Z gate (marked state gets phase -1)
    if n_qubits == 2:
        oracle.cz(0, 1)
    
    # Reverse X gates
    for i, bit in enumerate(marked_state):
        if bit == '0':
            oracle.x(i)
    
    return oracle

def diffusion_operator(qc, n_qubits):
    """
    Applies the diffusion operator (inversion about average).
    """
    # Hadamard gates
    for i in range(n_qubits):
        qc.h(i)
    
    # X gates
    for i in range(n_qubits):
        qc.x(i)
    
    # Multi-controlled Z
    if n_qubits == 2:
        qc.cz(0, 1)
    
    # X gates
    for i in range(n_qubits):
        qc.x(i)
    
    # Hadamard gates
    for i in range(n_qubits):
        qc.h(i)
    
    return qc

# Run Grover's algorithm
grovers_qc = grover_circuit(2, '11')
simulator = AerSimulator()
job = simulator.run(grovers_qc, shots=1000)
result = job.result()
counts = result.get_counts(grovers_qc)
print("Grover's Algorithm Result:")
print(counts)  # Should show '11' with high probability
```

---

## Example 3: Quantum Fourier Transform

```python
from qiskit import QuantumCircuit
import numpy as np

def qft_circuit(n_qubits):
    """
    Implements the Quantum Fourier Transform.
    """
    qc = QuantumCircuit(n_qubits)
    
    for j in range(n_qubits):
        qc.h(j)
        for k in range(j+1, n_qubits):
            angle = 2 * np.pi / (2**(k-j+1))
            qc.cp(angle, k, j)
    
    # Swap qubits
    for i in range(n_qubits//2):
        qc.swap(i, n_qubits-i-1)
    
    return qc

# Create QFT circuit
qft = qft_circuit(3)
print("Quantum Fourier Transform:")
print(qft)
```

---

## Example 4: Variational Quantum Eigensolver (VQE)

```python
from qiskit.primitives import Estimator
from qiskit.circuit import ParameterVector
from qiskit_aer import AerSimulator
import numpy as np

def vqe_ansatz(params):
    """
    Creates a variational quantum circuit (ansatz).
    """
    qc = QuantumCircuit(2)
    qc.ry(params[0], 0)
    qc.ry(params[1], 1)
    qc.cx(0, 1)
    qc.ry(params[2], 0)
    return qc

def hamiltonian_expectation(params, hamiltonian):
    """
    Calculates expectation value of Hamiltonian.
    """
    qc = vqe_ansatz(params)
    # Measurement and classical post-processing
    # (Simplified version)
    return np.random.rand()  # Placeholder

# VQE optimization would iterate to find minimum eigenvalue
print("VQE provides a hybrid classical-quantum approach to find ground states.")
```

---

## Visualization Tools

### Circuit Visualization

```python
from qiskit import QuantumCircuit
from qiskit.visualization import circuit_drawer
import matplotlib.pyplot as plt

qc = QuantumCircuit(2)
qc.h(0)
qc.cx(0, 1)
qc.measure_all()

# Display circuit
print(qc.draw())

# Save as image
qc.draw(output='mpl', filename='circuit.png')
plt.show()
```

### Result Visualization

```python
from qiskit_aer import AerSimulator
from qiskit.visualization import plot_histogram
import matplotlib.pyplot as plt

qc = QuantumCircuit(2, 2)
qc.h(0)
qc.cx(0, 1)
qc.measure([0, 1], [0, 1])

simulator = AerSimulator()
job = simulator.run(qc, shots=1000)
result = job.result()
counts = result.get_counts(qc)

# Plot histogram
plot_histogram(counts)
plt.show()
```

---

## Working with Real Quantum Hardware

```python
from qiskit_ibm_runtime import QiskitRuntimeService
from qiskit import QuantumCircuit

# Save credentials (one-time setup)
QiskitRuntimeService.save_account(
    channel="ibm_quantum",
    instance="ibm-q/open/main",
    token="<YOUR_API_TOKEN>"
)

# Load account
service = QiskitRuntimeService()

# Get backend
backend = service.least_busy()

# Run circuit on real hardware
qc = QuantumCircuit(2, 2)
qc.h(0)
qc.cx(0, 1)
qc.measure([0, 1], [0, 1])

job = backend.run(qc, shots=1000)
result = job.result()
print(result.get_counts())
```

---

## Best Practices

1. **Start with simulators** before running on real hardware
2. **Use parameter binding** for efficient batch execution
3. **Optimize circuit depth** to reduce errors on real hardware
4. **Handle errors gracefully** with error mitigation techniques
5. **Monitor job status** when running on real devices
6. **Use appropriate basis gates** for your backend

---

## Resources

- Official Documentation: https://qiskit.org/documentation
- IBM Quantum: https://quantum.ibm.com
- GitHub Repository: https://github.com/Qiskit/qiskit
- Community Slack: Qiskit Community Slack
