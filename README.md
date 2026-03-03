# Quant-computing-mini-projects--SUBSET-SUM-PROBLEM-WITH-WITH-GROVER-S-ALGORITHM
This project implements a quantum algorithm for the **Subset Sum Problem** using **Grover’s search algorithm** in Qiskit.

Given a finite set of positive integers:

    {x0, x1, ..., x_{n-1}}

and a target integer `t`, the goal is to:

- Output a subset of indices I ⊆ {0,...,n−1} such that

      sum_{i ∈ I} xi = t

- Or decide (with high probability) that no such subset exists.

The implementation satisfies the assignment constraints:

- Uses only:
  - Controlled phase gates
  - Arbitrary 1-qubit gates
  - Multi-controlled X (Toffoli-type) gates
  - Ancilla qubits
- No classical bits or measurements inside the quantum construction
- Measurement is performed only at the end for output

---

## Quantum Approach

### 1. Superposition Preparation

The subset register of `n` qubits is initialized in uniform superposition using Hadamard gates:

    H^⊗n |0⟩ = (1/√(2^n)) ∑ |s⟩

Each computational basis state `|s⟩` encodes a candidate subset.

---

### 2. Phase Oracle Construction

The oracle applies a phase flip:

    |s⟩ → (-1)^{[sum(s_i x_i) = t]} |s⟩

To construct this:

1. A QFT-based adder computes the subset sum into an accumulator register.
2. Subtract target `t`.
3. Detect if the accumulator equals zero.
4. Apply multi-controlled X to a phase qubit (phase kickback).
5. Uncompute all work registers (reversibility requirement).

The oracle is fully unitary and uses:

- QFT without swaps
- Controlled phase gates
- Multi-controlled X gates
- Ancilla registers

---

### 3. Grover Diffuser

The diffuser performs inversion about the mean:

    D = H^n X^n (MCZ) X^n H^n

Implemented via:

- Hadamards
- X gates
- Multi-controlled X (converted to controlled-Z)

---

### 4. Grover Iterations

The algorithm performs:

    (Oracle → Diffuser)^k

where `k ≈ (π/4)√(N/M)`.

Since M (number of solutions) is unknown, multiple iteration counts are tried.

---

### 5. Final Decision Procedure

The solver:

1. Measures the subset register.
2. Selects the most frequent bitstring.
3. Classically verifies the sum.
4. If verified → returns subset I.
5. Otherwise → declares “no solution with high probability.”

Because Grover is probabilistic, the solver:
- Uses multiple iteration attempts
- Uses multiple shots per attempt
- Verifies classically before accepting

The sturcture of the files:
-1.The construction start with QFT and MCX construction.ipynb for enviroment preparation and construction of QFT and MCX
-2.qubit operation and oracle construction.ipynb contains the functions for oracle construction
-3.Then construct diffuser and Grover algorithm in diffuser and grover algorithm.ipynb
-4.result report function and simple test.ipynb is for the result translation
-5.benchmark.ipynb is the code for final benchmark
-6.All the result is contained in total codes for project.ipynb
