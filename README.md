# Block Matrix Exponentials as Generating Functions for Time-Ordered Perturbations

A paper exploring the connection between structured block matrix exponentials and time-ordered integral operators in quantum mechanics, path integrals, and information geometry.

**Author:** Neil D. Lawrence  
**Institution:** University of Cambridge

---

## Overview

This work explores the connection between block matrix constructions from numerical linear algebra (Higham, Najfeld-Havel) and time-ordered perturbations in quantum mechanics (Dyson series, path integrals). 

### Key Insight

> The off-diagonal blocks of exp(M) for block upper-triangular M encode sums over all paths through the block structure. Each path corresponds to a specific time ordering of marked perturbations.

Time ordering emerges naturally from the algebraic structure of matrix blocks—different block topologies select different time orderings.

---

## Main Results

### 1. First-Order Formula (2n×2n block)

For the block matrix
```
M = [H   V]
    [0   H]
```
the (1,2) block of exp(M) equals:
```
L(H,V) = ∫₀¹ e^{(1-s)H} V e^{sH} ds
```
This is the first-order Dyson term, i.e. all insertion times summed automatically.

### 2. Symmetric Second-Order (Higham 4n×4n)

Higham's construction:
```
M = [H   E₁  E₂  0 ]
    [0   H   0   E₂]
    [0   0   H   E₁]
    [0   0   0   H ]
```
The (1,4) block computes the symmetric second Fréchet derivative, summing over both time orderings (E₁ before E₂, and E₂ before E₁).

### 3. Causal Second-Order (Najfeld-Havel 3n×3n)

The construction:
```
M = [H   E₁  0 ]
    [0   H   E₂]
    [0   0   H ]
```
The (1,3) block captures only one ordering (E₁ before E₂)—corresponding to causal/time-ordered propagation.

---

## Applications

### Quantum Exponential Families

For ρ(θ) = exp(H(θ))/Z(θ), compute the Fisher information matrix:
```
g_{ij}(θ) = ∂²/∂θᵢ∂θⱼ log tr(exp(H(θ)))
```
via one 4n×4n block exponential, avoiding nested integrals or finite differences.

### Dyson Series

Higher-order perturbation terms computed as off-diagonal blocks of (k+1)n × (k+1)n matrices.

### Kubo-Mori Metric

Quantum Fisher information expressed using block exponentials, clarifying symmetric vs. causal orderings.

### Linear Response Theory

First and second-order response functions directly from block exponentials.

---

## Repository Contents

```
.
├── README.md                          # This file
├── block-matrix-exponentials.tex      # Full LaTeX paper
├── block-matrix-exponentials.bib      # Bibliography
```

---

## Compilation

### Using pdflatex + biber
```bash
cd block-matrix-exponential-generating-functions
pdflatex block-matrix-exponentials.tex
biber block-matrix-exponentials
pdflatex block-matrix-exponentials.tex
pdflatex block-matrix-exponentials.tex
```

### Using latexmk (recommended)
```bash
latexmk -pdf -bibtex block-matrix-exponentials.tex
```

---

## Paper Structure

1. **Introduction** — Motivation, main results, applications
2. **First-Order Theory**  — 2×2 blocks and Dyson series
3. **Second-Order Symmetric**  — Higham 4×4 and Fréchet derivatives
4. **Second-Order Causal** — Najfeld-Havel 3×3 and time ordering
5. **Higher Orders** — Recursive constructions
6. **Applications** — Fisher information, Dyson, Kubo-Mori, response theory
7. **Numerical Benchmarks** — Accuracy, speed, stability comparisons
8. **Discussion** — Path integral connection, extensions
9. **Conclusions** 
10. **Appendices** — Proofs, code, alternative formulas

---

## Connections to Other Work

This paper provides technical foundations that can be cited from:
- **The Inaccessible Game (Origin)** — Conceptual bridge between time ordering and block structure
- **The Inaccessible Game (Budget)** — Computational efficiency considerations
- **The Inaccessible Game (Decoherence)** — Physical interpretation in decoherence contexts
- **QIG Code (`qig-code`)** — Implementation already exists in `qig/duhamel.py` (see CIP-000A)

---

## Code Implementations

### First-Order Block Exponential

```python
def first_order_block(H, V):
    """Compute L(H,V) = ∫₀¹ e^{(1-s)H} V e^{sH} ds"""
    n = H.shape[0]
    M = np.block([[H, V], [np.zeros((n,n)), H]])
    exp_M = expm(M)
    return exp_M[:n, n:]
```

### Symmetric Second-Order

```python
def second_order_symmetric_block(H, E1, E2):
    """Compute L⁽²⁾(H; E₁, E₂) using Higham 4n×4n"""
    n = H.shape[0]
    Z = np.zeros((n, n))
    M = np.block([[H, E1, E2, Z], [Z, H, Z, E2], 
                  [Z, Z, H, E1], [Z, Z, Z, H]])
    exp_M = expm(M)
    return exp_M[:n, 3*n:]
```

### Fisher Information

```python
def fisher_information(H, generators, theta):
    """Compute Fisher matrix for quantum exponential family"""
    # See qig-code softtware
```

**Dependencies:** `numpy`, `scipy` (uses `scipy.linalg.expm`)

**Status:** Implementations in `https://github.com/lawrennd/qig-code` as `/qig/duhamel.py`

---

## Numerical Results

Benchmarks on quantum spin systems (n = 50-200):

| Method | Accuracy | Speed | Stability |
|--------|----------|-------|-----------|
| **Block expm** | |  | |
| Nested quadrature (50 pts) | |  |  |
| Finite differences | |  |  |

**Hypothesis:** Block exponentials provide the best combination of accuracy, speed, and stability.

---

## Bridging Communities

The work looks to bridge across ideas in:

- **Numerical Linear Algebra** → Matrix functions, Fréchet derivatives (Higham, Najfeld-Havel)
- **Quantum Mechanics** → Dyson series, perturbation theory
- **Quantum Field Theory** → Time-ordered products, propagators
- **Information Geometry** → Fisher metrics, Kubo-Mori inner products
- **Path Integrals** → Sum over paths as algebraic structure (Feynman)

The question is to what extent are these are different perspectives on the same algebraic object.

---

## Citation

If you use this work, please cite:

```bibtex
@article{Lawrence-block25,
  title={Block Matrix Exponentials as Generating Functions for Time-Ordered Perturbations},
  author={Lawrence, Neil D.},
  journal={In preparation},
  year={2025}
}
```

---

## Acknowledgments

This work is directly inspired by Nicholas J. Higham's writings on block matrix constructions for Fréchet derivatives.

