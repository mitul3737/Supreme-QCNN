# Entangling Capability

Refers to a quantum circuit's ability to create quantum entanglement between qubits—a unique quantum phenomenon where qubits become correlated in ways impossible in classical systems. It measures how effectively a circuit can generate and distribute entanglement across its qubits, which is crucial for quantum advantage in algorithms like Shor's factoring, quantum machine learning, and error correction.


For example, GHZ state is an entangled quantum state for 3 qubits.
![image](https://github.com/user-attachments/assets/6603a986-156d-43a6-92bc-a5dc7feffc11)

It should have Mayer Wallach measure 1.

```
def meyer_wallach_entanglement(state_vector, num_qubits):
    """Computes the Meyer-Wallach entanglement measure."""
    sum_purities = 0.0

    for qubit in range(num_qubits):
        psi = state_vector.reshape([2] * num_qubits)  # Convert to tensor
        psi = np.moveaxis(psi, qubit, 0).reshape(2, -1)  # Shift qubit axis
        
        rho_k = psi @ psi.conj().T  # Compute reduced density matrix
        purity = np.trace(rho_k @ rho_k).real  # Purity Tr[ρ²]
        sum_purities += purity

    Q = 2 * (1 - sum_purities / num_qubits)  # Meyer-Wallach measure
    return Q


def entangling_capability(circuit_template, num_qubits, num_params, L, num_samples=1000):
    dev = qml.device("default.qubit", wires=num_qubits)

    @qml.qnode(dev)
    def run_circuit(params, input_state):
        qml.BasisState(input_state, wires=range(num_qubits))
        param_index = 0
        for _ in range(L):
            layer_params = params[param_index:param_index + num_params]
            circuit_template(layer_params, wires=range(num_qubits))
            param_index += num_params
        return qml.state()

    q_values = []

    for _ in range(num_samples):
        params = np.random.uniform(0, 2 * np.pi, num_params * L)
        for state_idx in range(2**num_qubits):
            input_state = [int(x) for x in np.binary_repr(state_idx, width=num_qubits)]
            state = run_circuit(params, input_state)

            Q = meyer_wallach_entanglement(state, num_qubits)  # Call MW measure
            q_values.append(Q)

    return np.mean(q_values)

def GHZ_circuit(params, wires):
    """Creates a 3-qubit GHZ state"""
    qml.Hadamard(wires=wires[0])
    qml.CNOT(wires=[wires[0], wires[1]])
    qml.CNOT(wires=[wires[1], wires[2]])

num_qubits = 3
num_params = 0  # GHZ state requires no parameters

Q_GHZ = entangling_capability(
    GHZ_circuit,  # Define GHZ circuit as discussed earlier
    num_qubits,
    num_params,
    L=1,
    num_samples=1000
)

print(f"GHZ State Meyer-Wallach Entanglement: {Q_GHZ:.4f}")
```

You can see the value 1  here.


Again for the circuits used in our code, if we detect entangling capability, here is the analysis:
The analysis of layerwise entangling capability reveals several key insights: U_9 demonstrates the strongest initial entanglement (EntCap=1.0 at Layer 1) due to its immediate CZ gate application, but shows volatile performance across layers, while U_6 and U_SU4 maintain consistently high entanglement (0.39-0.42) with more stable scaling. Most circuits exhibit increasing entanglement with depth, particularly U_5, U_13, and U_14, though U_9's sudden drop to zero at Layer 2 suggests potential measurement artifacts. The data highlights a clear trade-off between parameter efficiency and sustained entanglement, with U_9 achieving maximum early entanglement with just 2 parameters compared to U_5's 50 parameters needed for modest results. Architecturally, CRX gates (U_6) outperform CRZ gates (U_5), and the SU4 decomposition provides the most reliable scaling, making U_SU4 ideal for deeper circuits despite its higher parameter count, while U_9's initial performance makes it suitable for shallow applications. This suggests an optimal strategy might combine U_9's initial entanglement boost with U_SU4's scalable architecture for balanced performance across all layers.
