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

- 
# Deterministic Ansatze for the MBVQE (Schroeder, Heller and Gachechiladze)
- The study introduces MBVQE-ansatze that respect determinism.

- They study the possible advantages of the MBQC in variational algorithms.

- Determinism is crucial to ensure that the variational steps in the classical training part converge even during the perfect noiseless simulations. 

- Simulations for the Schwinger moderl and the XY-model. 

- 