# Quantum Tunneling, Entanglement, and Quantum Computing

## 1. Quantum Tunneling

### 1.1 Conceptual Overview
Quantum tunneling is a phenomenon where a particle can cross a potential barrier even if its energy is less than the barrier height. 

**Scenario:** Two potential wells (boxes) separated by a finite potential barrier. An electron in one well may tunnel into the adjacent well.

### 1.2 Theoretical Explanation
Consider a **1D potential barrier** of height \(V_0\) and width \(a\):

$$
V(x) =
\begin{cases} 
0, & x < 0 \\
V_0, & 0 \le x \le a \\
0, & x > a
\end{cases}
$$

The **time-independent Schrödinger equation (TISE)** is:

$$
-\frac{\hbar^2}{2m} \frac{d^2 \psi(x)}{dx^2} + V(x)\psi(x) = E\psi(x)
$$

- For \(x < 0\) (region I) and \(x > a\) (region III), \(V(x) = 0\):

$$
\psi_{I}(x) = A e^{i k x} + B e^{-i k x}, \quad 
\psi_{III}(x) = F e^{i k x}, \quad 
k = \sqrt{\frac{2 m E}{\hbar^2}}
$$

- For \(0 \le x \le a\) (region II), \(V(x) = V_0 > E\):

$$
\psi_{II}(x) = C e^{\kappa x} + D e^{-\kappa x}, \quad 
\kappa = \sqrt{\frac{2 m (V_0 - E)}{\hbar^2}}
$$

**Transmission coefficient (probability of tunneling):**

$$
T = \frac{|F|^2}{|A|^2} \approx e^{-2 \kappa a} \quad \text{(for } V_0 \gg E\text{)}
$$

### 1.3 Physical Interpretation
- The wavefunction "leaks" into the barrier, allowing the particle to appear on the other side.

### Code
```
import numpy as np
import plotly.graph_objects as go
from scipy.linalg import expm


L = 1.0     
gap = 0.5    
N = 40       

x = np.linspace(0, 2*L + gap, N) 
y = np.linspace(0, L, N)
z = np.linspace(0, L, N)
X, Y, Z = np.meshgrid(x, y, z, indexing='ij')


def psi_box(X, center):
    sigma = 0.25
    return np.exp(-(X - center)**2 / (2*sigma**2))


center_L = L/2
center_R = L + gap + L/2

psi_L = psi_box(X, center_L)
psi_R = psi_box(X, center_R)

tunnel = 0.3  
H = np.array([[0, -tunnel],
              [-tunnel, 0]]) 


tau = 10.0  
U = expm(-1j * H * tau)


a, b = U @ np.array([1.0, 0.0])


psi_total = np.abs(a*psi_L + b*psi_R)


fig = go.Figure()

fig.add_trace(go.Isosurface(
    x=X.flatten(),
    y=Y.flatten(),
    z=Z.flatten(),
    value=psi_total.flatten(),
    isomin=0.05*np.max(psi_total), 
    isomax=np.max(psi_total),
    surface_count=3,
    colorscale='Viridis',
    opacity=0.6,
    caps=dict(x_show=False, y_show=False, z_show=False)
))

# Layout
fig.update_layout(
    title=f" Tunneling (time t={tau})",
    scene=dict(
        xaxis_title='x',
        yaxis_title='y',
        zaxis_title='z',
        aspectratio=dict(x=2.5, y=1, z=1)
    )
)

fig.show()
```
### Output

```
<img width="752" height="334" alt="image" src="https://github.com/user-attachments/assets/f971adfd-9ba4-4e2e-853a-c08f517d8ccf" />
```
---

## 2. Quantum Entanglement

### 2.1 Conceptual Overview
Entanglement is a quantum property where the states of two or more particles become correlated such that the state of one cannot be described independently of the others, even at large distances.

### 2.2 Theoretical Explanation
Consider two qubits (quantum bits) represented by states \(|0\rangle\) and \(|1\rangle\). An entangled state can be written as a 

$$
|\Phi^+\rangle = \frac{1}{\sqrt{2}} \left( |00\rangle + |11\rangle \right)
$$

**Properties:**
- Measurement of one qubit immediately determines the state of the other.
- Cannot be factored into single-qubit states:

$$
|\Phi^+\rangle \neq |\psi_1\rangle \otimes |\psi_2\rangle
$$

### 2.3 Density Matrix Representation
For a Bell state:

$$
\rho = |\Phi^+\rangle \langle \Phi^+| = \frac{1}{2}
\begin{pmatrix}
1 & 0 & 0 & 1 \\
0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 \\
1 & 0 & 0 & 1
\end{pmatrix}
$$

The **reduced density matrix** of one qubit is maximally mixed:

$$
\rho_1 = \text{Tr}_2 (\rho) = \frac{1}{2} I
$$

**Interpretation:** The single qubit is completely uncertain, but the joint system is perfectly correlated.

---

## 3. Connection to Quantum Computing

### 3.1 Qubits and Superposition
- Qubits exploit **superposition**, 

$$
|\psi\rangle = \alpha |0\rangle + \beta |1\rangle
$$

enabling parallel computation.
- Tunneling in quantum dots allows **coherent coupling** between qubits.

### 3.2 Entanglement as a Resource
- Entanglement allows **quantum gates** like CNOT to create correlations between qubits.
- Enables **quantum algorithms** like Shor’s factoring and Grover’s search.

### 3.3 Strengthening Quantum Computers
- **Speedup:** Entanglement allows qubits to represent exponentially many states simultaneously.
- **Error correction:** Entangled qubits form the basis of **quantum error-correcting codes**.
- **Connectivity:** Tunneling-mediated entanglement in quantum dots creates **scalable architectures**.

---

