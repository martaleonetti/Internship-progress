
# First Week  

- Have a better understanding of the Variational Quantum Eigensolver (VQE) mechanisms.

\[
\mathcal{L}(\theta) = \langle \psi(\theta) | H | \psi(\theta) \rangle
\]

where \( H \) is the Hamiltonian and \( \psi(\theta) \) is the parameterized quantum state.

- Have a better understanding on stabilizer states and formalism 

- Read "Doped stabilizer states in many-body physics and where to find them"
  (Gu, Oliviero, Leone)

- Understand PGA (need some references about it)

Following...

- Try to collect some ideas on extending MBQC-based VQE beyond Stabilizer Hamiltonians.

**key question**: the MBQC framework naturally suits for stabilizer Hamiltonians, can be extended to more general quantum systems that exhibit non-stabilizer properties?

Some ideas:
-
-
- Spin system Hamiltonians 

...

# Interesting articles 

*    Stabilizer ground states for simulating quantum many-body physics: theory,
 algorithms, and applications
  (https://arxiv.org/abs/2403.08441)
(section E interesting)

 * (http://arxiv.org/abs/2312.13241) starting from this article, would like to try to think about generalization for other problems.


# VQE 

The variational quantum eigensolver is a quantum-classical hybrid algorithm that approximates the lowest eigenvalue and its corresponding eigenvector of a given hermitian operator, typically the Hamiltonian of the investigated quantum system. 

**BASIC IDEA**: generate quantum states using a parametrized quantum circuit, the so called ansatz, and classically optimize the parameters. 

- **How It Works**
1. **Hamiltonian Decomposition**:  
   \( H \) is written as a sum of Pauli operators:
   \[
   H = \sum_i c_i P_i
   \]
   Since quantum computers measure Pauli observables, this form is necessary.

2. **Parameterized Quantum Circuit (Ansatz)**:  
   A quantum circuit \( U(\boldsymbol{\theta}) \) generates a trial state:
   \[
   \ket{\psi(\boldsymbol{\theta})} = U(\boldsymbol{\theta}) \ket{0}
   \]
   The choice of ansatz is crucial.

3. **Energy Measurement**:  
   The expectation value is computed:
   \[
   E(\boldsymbol{\theta}) = \sum_i c_i \bra{\psi(\boldsymbol{\theta})} P_i \ket{\psi(\boldsymbol{\theta})}
   \]
   Each term is measured separately.

4. **Classical Optimization**:  
   A classical optimizer finds \( \boldsymbol{\theta} \) to minimize \( E(\boldsymbol{\theta}) \). This loop continues until convergence.


- **Difficulties**:
 **Choosing the right ansatz**: Needs to be expressive but efficient.
 **Optimization difficulties**: Some ansätze suffer from barren plateaus.
 **Measurement cost**: Evaluating all Pauli terms can be expensive.

# A measurement-based variational quantum eigensolver (2021)

- Propose a new approach to VQEs using the principles of MBQC. 

- Development of a new variational technique based on MBQC: called measurement-based VQE.
- Protocols determine the ground state of a target Hamiltonian. 

- Use a tailored entangled state called "custum state" that allows for exploring an appropriate corner of the system's Hilbert space. This custom state includes auxiliary qubits which, once measured, modify the state of the output qubits. (edge-decoration). 

- Start from an ansatz state represented as a graph, this ansatz is modified adding other qubits, these qubits are measured in rotated basis R($\theta$), where $\theta$ are the variational parameters.

- MB-VQE shifts the challenge from performing multi-qubits gates to  creating an entangled initial state. 

- **Toric code model**

- To make the computation deterministic, so-called byproduct operators and adaptive measurements are required. 

- Photonic quantum systems 

- MB-VQEs are advantageous whenever a perturbation $H_p$ is added to a Hamiltonian $H_0$ whose ground state, used as ansatz state $\ket{\psi_a}$, is a stabilizer state or a graph state. 



# Deterministic Ansatze for the MBVQE (Schroeder, Heller and Gachechiladze)
- The study introduces MBVQE-ansatze that respect determinism.

- They study the possible advantages of the MBQC in variational algorithms.

- Determinism is crucial to ensure that the variational steps in the classical training part converge even during the perfect noiseless simulations. 

- Simulations for the Schwinger moderl and the XY-model. 

- 
...
- 

Reference article:  
(https://arxiv.org/abs/2403.08441)

# Connecting Stabilizer Ground States to Physical Models

The article introduces the concept of **stabilizer ground states**, which are quantum many-body ground states that can be exactly described within the stabilizer formalism. This is particularly useful because stabilizer states have a simple mathematical structure, making them efficient to simulate and analyze.

The main result of the article is the development of an **efficient, exact algorithm** to determine stabilizer ground states for **1D local Hamiltonians**. This algorithm scales **linearly** and can also be extended to infinite periodic systems.


This article provides a way for efficiently identifying and computing stabilizer ground states of quantum many-body systems. The key **result is an algorithm that, under certain conditions, allows one to determine whether the ground state of a given Pauli Hamiltonian is a stabilizer state** and, if so, to compute it efficiently with a linear-scaling method.  However, my internship focuses on an extension **beyond stabilizer ground states**, aiming to explore how a variational measurement-based quantum computing (MBQC) Ansatz can approximate the ground state of more general Hamiltonians that are not exactly describable within the stabilizer formalism.

A key observation is that while stabilizer ground states are efficiently simulable, some interesting quantum many-body systems exhibit ground states with a richer entanglement structure that goes beyond what stabilizer states can capture. This motivates the need to test the MBQC Ansatz in a regime where stabilizer techniques begin to fail. One natural way to set up this problem is to consider Hamiltonians of the form
\[
H(t) = H_0 + t H_1,
\]
where \(H_0\) is a stabilizer Hamiltonian whose ground state is exactly solvable using the methods developed in the article, and \(H_1\) is a perturbation that breaks the stabilizer structure, leading to a more complex ground state. For small \(t\), the ground state remains close to the stabilizer regime and can still be approximated using minor modifications. However, as \(t\) increases, the system transitions into a regime where the stabilizer approximation is no longer valid, and alternative approaches, such as the variational MBQC Ansatz, must be tested.

To investigate this, I plan to start by selecting a class of Hamiltonians that naturally fit this framework. A first candidate is the **transverse-field Ising model**,
\[
H(t) = -\sum_i Z_i Z_{i+1} - t \sum_i X_i,
\]
which is stabilizer-based at \(t = 0\) but becomes highly entangled for \(t \gg 0\). Another interesting possibility is the **toric code** with a transverse field,
\[
H(t) = -\sum_{\text{plaquettes}} A_p - \sum_{\text{stars}} B_s - t \sum_i X_i,
\]
which exhibits a transition from a topologically ordered stabilizer state to a more complex phase.  

Once an appropriate Hamiltonian is chosen, the first step will be to verify that the stabilizer-based algorithm from the article correctly computes the ground state at \(t = 0\). This will serve as a baseline against which the MBQC Ansatz can be tested. The next stage will involve incrementally increasing \(t\) and analyzing the performance of the MBQC Ansatz in approximating the true ground state, comparing its fidelity with exact diagonalization for small system sizes. Particular attention will be given to how the Ansatz structure evolves as the system moves away from the stabilizer regime, and whether certain graph decorations or additional variational parameters improve the expressivity of the Ansatz.

The final stage will focus on benchmarking the MBQC Ansatz against other known techniques, such as tensor networks or variational Monte Carlo, to evaluate its efficiency and accuracy. A key objective will be to determine whether MBQC provides advantages in simulating non-stabilizer ground states, particularly in terms of computational scalability. 


First I need to understand the computational graph Ansatz generator machine created by the group, then the choice of a suitable Hamiltonian could be finalized, with preliminary numerical experiments for \(t = 0\). Then I could focus on implementing the MBQC Ansatz and testing its accuracy in the perturbative regime where \(t\) is small. I then could extend these tests to larger values of \(t\), analyzing how well the Ansatz captures the transition away from the stabilizer regime. 

