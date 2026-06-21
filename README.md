# Data-driven-approach-for-modeling-Qubit-decoherence
# Quantum Key Distribution (QKD) Robustness & Quantum Decoherence Simulation Framework

This repository provides a simulation engine and machine learning evaluation pipeline for analyzing single-qubit quantum states under environmental noise channels and adversarial interventions. The codebase is structured into three computational layers: interactive decoherence tracking, multi-protocol Quantum Bit Error Rate (QBER) benchmarking under eavesdropping attacks, and classical regression profiling for quantum state reconstruction.

---

## 1. Architectural Components

### Module I: Interactive Quantum Decoherence Simulator
Simulates the discrete iterative decay of a pure maximum superposition state $|\psi\rangle = |+\rangle$ under distinct quantum noise channels. It tracks two physical properties across a configurable step range:
*   **Coherence:** Visualized via the absolute value of the off-diagonal density matrix element $|\rho_{01}|$.
*   **Purity:** Evaluated analytically using the trace of the squared density matrix $\text{Tr}(\rho^2)$.

### Module II: Multi-Protocol QKD Security & Eavesdropping Engine
Simulates quantum key distribution across $N = 20,000$ qubits. It includes an interactive intercept-resend attack model ("Eve") to compute empirical security degradation. The module benchmarks nine distinct protocol configurations:
*   **BB84 Variants:** Standard BB84 (mutually unbiased computational and Hadamard bases) and Decoy-state BB84.
*   **B92 Variants:** Standard B92 (non-orthogonal state signaling), Decoy-state B92, Differential Phase Shift (DPS) B92, Two-Way B92, Entanglement-Based B92, and Subcarrier Quantum Phase Modulation (SRP) B92.
*   **SARG04:** Four-state protocol using distinct state-sifting logic.

### Module III: Classical Machine Learning & State Reconstruction Pipeline
Generates a unique dataset of 1,000 randomized, normalized pure single-qubit states subjected to a Pauli-X noise channel. It evaluates the capability of classical machine learning algorithms to map a 5-dimensional coordinate space ($[p, \text{Re}(a), \text{Im}(a), \text{Re}(b), \text{Im}(b)]$) back to the 8-dimensional flattened real and imaginary components of the resulting noisy mixed density matrix $\rho_{\text{noisy}}$.

---

## 2. Theoretical Formulations

### Density Operators and Initial State
The system initializes using the symmetric pure state:

$$|\psi\rangle = \frac{|0\rangle + |1\rangle}{\sqrt{2}}$$

$$\rho_0 = |\psi\rangle\langle\psi| = \begin{pmatrix} 0.5 & 0.5 \\ 0.5 & 0.5 \end{pmatrix}$$

### Quantum Noise Channels
Noise environments are mapped via discrete transformations using the Pauli matrices $X$, $Y$, and $Z$:

*   **Bit Flip:** $\mathcal{E}(\rho) = (1-p)\rho + p X \rho X$
*   **Phase Flip:** $\mathcal{E}(\rho) = (1-p)\rho + p Z \rho Z$
*   **Depolarizing:** $\mathcal{E}(\rho) = (1-p)\rho + \frac{p}{3}(X\rho X + Y\rho Y + Z\rho Z)$

### Intercept-Resend Eavesdropping Model
An adversarial interaction introduces random bit randomization across targeted channels. The compromised transmission indices return a random binary state selection:

$$\text{bits}_{\text{attacked}} \in \{0, 1\} \quad \text{with probability } 0.5$$

---

## 3. Statistical Evaluation Metrics

### Quantum Bit Error Rate (QBER)
Measures the ratio of discrepant bits between Alice's sifted transmission array $a$ and Bob's received measurement array $b$:

$$\text{QBER} = \frac{1}{N} \sum_{i=1}^{N} [a_i \neq b_i]$$

### Matrix Mean Squared Error (Frobenius Norm Validation)
The machine learning model performance profile reconstructs complex $2 \times 2$ matrices from predicted coordinates to compute error metrics over the test splits:

$$\text{Matrix MSE} = \frac{1}{N} \sum_{i=1}^{N} \|\rho_{\text{true}}^{(i)} - \rho_{\text{pred}}^{(i)}\|_F^2$$

---

## 4. Evaluated Regressors

The framework runs a 5-fold Monte Carlo cross-validation pipeline tracking performance variations across:
1.  **Lasso ($L_1$ Regularization):** Linear baseline ($\alpha = 0.001$).
2.  **Ridge ($L_2$ Regularization):** Linear baseline ($\alpha = 1.0$).
3.  **Gradient Boosting Regressor:** Non-linear decision tree ensemble ($100$ estimators, maximum depth of 3).
4.  **Standard Multilayer Perceptron (MLP):** Feedforward PyTorch NN containing two hidden layers ($32$ neurons each) using ReLU activations.
5.  **Deep Residual Network (ResNet):** Incorporates explicit skip connections over dense interior blocks to preserve structural state transparency during optimization.

---

## 5. Prerequisites & Environment Setup

The workspace requires Python 3.x along with the following scientific computing packages:

```bash
pip install numpy pandas matplotlib scikit-learn torch ipywidgets

