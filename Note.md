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


This article provides a way for efficiently identifying and computing stabilizer ground states of quantum many-body systems. The key **result is an algorithm that, under certain conditions, allows one to determine whether the ground state of a given Pauli Hamiltonian is a stabilizer state** and, if so, to compute it efficiently with a linear-scaling method.  However, my internship focuses on an extension **beyond stabilizer ground states**, aiming to explore how a variational measurement-based quantum computing (MBQC) Ansatz can approximate the ground state of more general Hamiltonians that are not exactly represented within the stabilizer formalism.

A key observation is that while stabilizer ground states are efficiently simulable, some interesting quantum many-body systems exhibit ground states with a more complicated entanglement structure that goes beyond what stabilizer states can capture. This motivates the need to test the MBQC Ansatz in a regime where stabilizer techniques begin to fail. One natural way to set up this problem is to consider Hamiltonians of the form
\[
H(t) = H_0 + t H_1,
\]
where \(H_0\) is a stabilizer Hamiltonian whose ground state is exactly solvable using the methods developed in the article, and \(H_1\) is a perturbation that breaks the stabilizer structure, leading to a more complex ground state. For small \(t\), the ground state remains close to the stabilizer regime and can still be approximated using minor modifications. However, as \(t\) increases, the system transitions into a regime where the stabilizer approximation is no longer valid, and alternative approaches, such as the variational MBQC Ansatz, must be tested.

To investigate this, and idea could be to start by selecting a class of Hamiltonians that naturally fit this needs. Some candidates??  **transverse-field Ising model**,
\[
H(t) = -\sum_i Z_i Z_{i+1} - t \sum_i X_i,
\]
which is stabilizer-based at \(t = 0\) but becomes highly entangled for \(t \gg 0\). Another possibility could be the **toric code** with a transverse field:
\[
H(t) = -\sum_{\text{plaquettes}} A_p - \sum_{\text{stars}} B_s - t \sum_i X_i,
\]
which exhibits a transition from a topologically ordered stabilizer state to a more complex phase.  

First, I need to check that the stabilizer-based algorithm correctly finds the ground state at \( t = 0 \). This will serve as a reference point to evaluate the MBQC Ansatz.

Then, I'll gradually increase \( t \) and see how well the Ansatz approximates the ground state, comparing it with exact diagonalization for small systems. The interesting part will be understanding how the Ansatz structure evolves as the system moves away from the stabilizer regime and whether graph decorations or extra variational parameters can improve its expressivity.

The final stage will be benchmarking against other known methods, like tensor networks and variational Monte Carlo, to see if MBQC offers any advantages, especially in terms of computational scalability.

But first, I need to fully understand how the computational graph Ansatz generator developed by the group works. Then, I can finalize the choice of a suitable Hamiltonian and run some initial numerical tests for \( t = 0 \). After that, I'll implement the MBQC Ansatz and test it in the perturbative regime before extending the analysis to larger \( t \) to see how things change outside the stabilizer regime.


## Initial Steps for the Project

First, I tried to understand the stabilizer formalism and what variational quantum algorithms are.

Variational quantum algorithms have been proposed to discover the ground state of physically motivated Hamiltonians. This is usually proposed in the
circuit model using a hardware-efficient Ansatz, meaning that the circuit is made of
parametrized gates assembled in a shallow way. This aims to provide
enough expressivity to approach the desired ground state while at the same time being short enough to avoid the adverse effects of decoherence.

More recently, the same approach has been proposed in the Measurement-Based Quantum Computation (MBQC) model. Yet, most of the work is still to be developed. The main questions are:

- How to construct Ansätze?
- How to perform the optimization?
- What loss function to use?
- How does it behave differently compared to circuit VQAs?

We have developed an approach to generate valid computational graphs that can serve as an Ansatz. Now, what is needed is to find a suitable problem to experiment with the full variational MBQC approach to guide the
development of theoretical tools.

# Stabilizer formalism

The stabilizer formalism was originally developed for quantum error correction but now plays a key role in various aspects of quantum computing.

## Stabilizer Gates
The set of stabilizer gates includes:
- **CNOT** (Controlled-NOT)
- **Hadamard**:  
  \[
  H = \frac{1}{\sqrt{2}} \begin{bmatrix} 1 & 1 \\ 1 & -1 \end{bmatrix}
  \]
- **Phase Gate**:  
  \[
  P = \begin{bmatrix} 1 & 0 \\ 0 & i \end{bmatrix}
  \]

## Stabilizer Circuits and Stabilizer States
A stabilizer circuit is a quantum circuit composed entirely of stabilizer gates.  
A stabilizer state is a quantum state that can be generated by a stabilizer circuit starting from the initial state \(|00...0\rangle\).

## Universality and Limitations
The set \( S = \{\text{CNOT}, H, P\} \) is not universal for quantum computation, meaning a quantum circuit that involves only these gates cannot generate all quantum states, but only a subset. This is because:
- Stabilizer gates generate only a discrete set of states.
- The generated states are always equally weighted superpositions over an affine subspace of \(\mathbb{F}_2^n\).
- For example, for a single qubit, there are only 6 distinct stabilizer states: \(|0\rangle, |1\rangle, |+\rangle, |-\rangle, |i\rangle, |-i\rangle\).

## Definition of Stabilization
A unitary operator \(U\) stabilizes a quantum state \(|\Psi\rangle\) if
\[
U \,|\Psi\rangle = |\Psi\rangle.
\]
If instead \( U |\Psi\rangle = -|\Psi\rangle\), then \(U\) does not stabilize \(|\Psi\rangle\).  
The set of unitary matrices that stabilize \(|\Psi\rangle\) forms a group under multiplication.

## Pauli Matrices
The Pauli matrices, fundamental in quantum mechanics, are:
\[
I = \begin{bmatrix} 1 & 0 \\ 0 & 1 \end{bmatrix},\quad
X = \begin{bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix},\quad
Y = \begin{bmatrix} 0 & -i \\ i & 0 \end{bmatrix},\quad
Z = \begin{bmatrix} 1 & 0 \\ 0 & -1 \end{bmatrix}.
\]
These matrices satisfy important algebraic identities:

\[
X^2 = Y^2 = Z^2 = I, \quad XY = iZ, \quad YX = -iZ,
\]
\[
YZ = iX, \quad ZY = -iX, \quad ZX = iY, \quad XZ = -iY.
\]

Each Pauli matrix stabilizes specific quantum states:
\[
X |+\rangle = |+\rangle, \quad -X |-\rangle = |-\rangle,
\]
\[
Z |0\rangle = |0\rangle, \quad -Z |1\rangle = |1\rangle,
\]
\[
Y |i\rangle = |i\rangle, \quad -Y |-i\rangle = |-i\rangle.
\]

A stabilizer group for an \(n\)-qubit state \(|\Psi\rangle\) is the set of all tensor products of Pauli matrices that stabilize \(|\Psi\rangle\). Since Pauli matrices are closed under multiplication, stabilizer groups are abelian.  
If \(G_x\) is a stabilizer group, the fact that it's **abelian** means that for all \(g, h \in G_x\), the group operation is commutative:
\[
gh = hg.
\]
Hence, every element of \(G_x\) commutes with every other element.

For example, the stabilizer group of \(|0\rangle\) is:
\[
\{ I, Z \}, \quad \text{since } Z^2 = I,
\]
while for \(|+\rangle\):
\[
\{ I, X \}.
\]
The stabilizer of the product state \(|0\rangle \otimes |+\rangle\) is given by the Cartesian product:
\[
\{ II, IX, ZI, ZX \}.
\]

An important result states that any \(n\)-qubit stabilizer state has a stabilizer group of size \(2^n\), giving a structural characterization of stabilizer states independent of circuits.  

Consider the Bell state:
\[
|\psi\rangle = \frac{|00\rangle + |11\rangle}{\sqrt{2}}.
\]
This state is stabilized by:
\[
X_1 X_2 |\psi\rangle = |\psi\rangle, \quad Z_1 Z_2 |\psi\rangle = |\psi\rangle.
\]

More generally, the Pauli group on one qubit is:
\[
G_1 = \{ \pm I, \pm iI, \pm X, \pm iX, \pm Y, \pm iY, \pm Z, \pm iZ \},
\]
and for \(n\) qubits:
\[
G_n = \{ g_1 \otimes g_2 \otimes \dots \otimes g_n \mid g_i \in G_1 \}.
\]

A stabilizer is a subgroup \(S \subset G_n\) that defines a stabilized vector space, representing the code space:
\[
V_S = \{ |\psi\rangle \mid g |\psi\rangle = |\psi\rangle, \ \forall g \in S \}.
\]
Thus, \(S\) stabilizes \(V_S\), meaning every state in \(V_S\) remains unchanged under all elements of \(S\).

A fundamental question is how to represent stabilizer groups efficiently. It turns out that an \(n\)-qubit stabilizer group is always generated by exactly \(n\) elements, meaning that specifying a stabilizer state requires only \(n\) generators.  

The space required to store these generators is \(O(n^2)\) bits, an exponential reduction compared to the full state vector representation, which requires \(O(2^n)\) bits.

## Partially Stabilized States
In a system of \(n\) qubits, a fully stabilized state requires \(n\) independent stabilizer generators. However, if only \(k \leq n\) stabilizers are present, the state is *partially stabilized*, meaning it belongs to a subspace of dimension \(2^{n-k}\). For instance, in a 3-qubit system with only two stabilizer generators, the stabilized states form a 2-dimensional subspace, effectively encoding a single logical qubit. The logical space is spanned by basis states that remain invariant under the stabilizers, and logical operators commute with all stabilizers while acting nontrivially within the logical subspace.

## Unitary gates in the stabilizer formalism
The stabilizer formalism is not only useful for describing quantum states but also for understanding their evolution under unitary operations. Given a unitary \(U\) acting on a space \(V_S\) stabilized by \(S\), the transformed space is stabilized by:
\[
U S U^\dagger = \{ U g U^\dagger \mid g \in S \}.
\]
Thus, to update the stabilizer, it suffices to compute how \(U\) affects its generators.

A key example is the Hadamard gate, which transforms the Pauli matrices as:
\[
H X H^\dagger = Z, \quad H Y H^\dagger = -Y, \quad H Z H^\dagger = X.
\]
This implies that applying \(H\) to the stabilizer of \(|0\rangle\) results in the stabilizer of \(|+\rangle\). Extending this to \(n\) qubits, applying \(H^{\otimes n}\) to \(|0\rangle^{\otimes n}\) transforms the stabilizers from:
\[
\{ Z_1, Z_2, \dots, Z_n \} \quad \text{to} \quad \{ X_1, X_2, \dots, X_n \}.
\]

Similarly, entangling gates like the controlled-\(Z\) (CZ) gate conjugate stabilizers as:
\[
U X_1 U^\dagger = X_1 X_2, \quad U X_2 U^\dagger = X_2, \quad U Z_1 U^\dagger = Z_1, \quad U Z_2 U^\dagger = Z_1 Z_2.
\]
Other operators follow accordingly, allowing efficient tracking of stabilizer evolution under Clifford gates.

This efficiency enables classical simulation of stabilizer circuits, as formalized in the Gottesman--Knill theorem: any stabilizer circuit acting on an initial stabilizer state \(|00\ldots0\rangle\) can be simulated in polynomial time. Thus, stabilizer circuits alone cannot provide superpolynomial quantum speedups.

The Gottesman--Knill theorem states that certain quantum computations, despite involving entanglement, can be efficiently simulated on a classical computer. Specifically, a quantum circuit using only the following operations is classically simulable:
- State preparation in the computational basis
- Hadamard gates
- Phase gates
- Controlled gates
- Pauli gates
- Measurements of Pauli observables
- Classical control based on measurement outcomes

Since these operations can be efficiently tracked using the stabilizer formalism, a classical computer can simulate them in \(O(n^2)\) time per step, leading to an overall complexity of \(O(n^2 m)\) for a computation with \(m\) operations.


## Measurement-Based Quantum Computation (MBQC)

One major challenge in MBQC is ensuring determinism in measurement outcomes. This is particularly important for algorithms like the Variational Quantum Eigensolver (VQE), which is used to approximate the ground state of quantum systems. If the measurement sequence does not follow a well-defined flow, the algorithm might evaluate the cost function at irrelevant points, leading to incorrect results.

...
(Rest of MBQC section follows)
