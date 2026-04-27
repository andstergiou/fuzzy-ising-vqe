# Fuzzy Sphere Ising VQE — companion code for *Universality of Quantum Gates in Particle and Symmetry Constrained Subspaces*

This repository contains the Jupyter notebook accompanying the paper

> **Universality of Quantum Gates in Particle and Symmetry Constrained Subspaces**  
> Andreas Stergiou and Nicolas PD Sawaya (2026)  
> [arXiv:2604.XXXXX [quant-ph]] *(link to be added)*

## Overview

The notebook `ising_n4.ipynb` implements the variational quantum simulation of the fuzzy sphere regularisation of the 3D Ising conformal field theory (CFT) described in Section 4 of the paper. It demonstrates two main theoretical results:

- **Lemma 2 (Jacobian spanning criterion):** a 19-parameter hardware-efficient circuit numerically achieves full Jacobian rank on the symmetry-constrained subspace, confirming that it can universally prepare any state within that subspace.
- **Proposition 3 (symmetry-preserving gates):** the circuit, built from `SingleExcitation` ($G^{(2)}$) and `DoubleExcitation` ($G^{(4)}$) rotations without Jordan–Wigner strings, generates the full orthogonal group on the $S_z=0$, half-filled subspace.

## Physical setup

| Parameter | Value | Meaning |
|-----------|-------|---------|
| $N$ | 4 | Number of electrons |
| Qubits | 8 | Two per orbital ($m = -\tfrac{3}{2}, -\tfrac{1}{2}, \tfrac{1}{2}, \tfrac{3}{2}$, each with spin $\uparrow/\downarrow$) |
| Subspace dimension $w_0$ | 18 | Half-filled, $S_z = 0$ sector |
| $V_0$ | 4.75 | Interaction strength |
| $V_1$ | 1 | Interaction strength |
| $h$ | 6.32 | Transverse field |

The parameters $(V_0, h)$ sit on the 3D Ising critical line with minimal finite-size effects, following [Zhu et al. (2022)](https://doi.org/10.1103/PhysRevX.13.021009).

## Contents of the notebook

| Section | What it does |
|---------|-------------|
| Definitions | Builds the fuzzy sphere Hamiltonian in second quantisation using OpenFermion, applies a Jordan–Wigner transformation, and imports it into PennyLane |
| Symmetry-constrained subspace & Jacobian criterion | Identifies the 18 half-filled $S_z=0$ basis states; verifies Lemma 2 by checking the rank of the finite-difference Jacobian at 100 random parameter points |
| Exact diagonalisation | Diagonalises the $18\times 18$ reduced Hamiltonian; extracts CFT scaling dimensions by normalising the spectrum on $T_{\mu\nu}$ (Table 1 of the paper) |
| VQE — ground state | Minimises the energy expectation value with a two-phase Adam + gradient-descent optimiser |
| VQD — first excited state | Adds a penalty term proportional to the overlap with the ground state |
| VQD — second excited state | Adds penalty terms for both previously found eigenstates |
| Reachability check | Confirms that the VQE-found states match the exact eigenstates to ten significant figures, and that each exact eigenstate can be reached by direct overlap maximisation |

## Results

The notebook reproduces Table 2 of the paper:

| State | Exact (ED) | VQE/VQD |
|-------|-----------|---------|
| Ground state | −16.18995794 | −16.18995782 |
| First excited state | −8.59299820 | −8.59299785 |
| Second excited state | 3.61550790 | 3.61550806 |

All three eigenvalues are recovered to within $4\times10^{-7}$ in absolute energy.

## Requirements

```
pennylane
pennylane-lightning
openfermion
sympy
scipy
matplotlib
numpy
```

Install with:

```bash
pip install pennylane pennylane-lightning openfermion sympy scipy matplotlib numpy
```

The notebook was developed and tested with Python 3.11. Exact package versions are recorded in the cell outputs.

## Usage

Open the notebook and run all cells in order:

```bash
jupyter notebook ising_n4.ipynb
```

Random seeds are fixed (`np.random.seed(0/1/2)` for the three VQE runs) so results are fully reproducible.

## Citation

If you use this code, please cite the paper:

```bibtex
@article{Stergiou:2026dts,
    author  = {Stergiou, Andreas and Sawaya, Nicolas P. D.},
    title   = {{Universality of Quantum Gates in Particle and Symmetry Constrained Subspaces}},
    eprint = "2604.xxxxx",
    archivePrefix = "arXiv",
    primaryClass = "quant-ph",
    month = "4",
    year = "2026"
}
```

## License

Released under the [MIT License](https://opensource.org/licenses/MIT).
