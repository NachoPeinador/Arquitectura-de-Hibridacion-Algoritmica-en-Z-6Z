# A Modular DSP Architecture for Extreme-Precision Computation of π

[![License: PolyForm Noncommercial](https://img.shields.io/badge/License-PolyForm_Noncommercial_1.0.0-red.svg)](LICENSE)
[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![Paper](https://img.shields.io/badge/Paper-Read_PDF-B31B1B?style=flat&logo=latex&logoColor=white)](https://github.com/NachoPeinador/Arquitectura-de-Hibridacion-Algoritmica-en-Z-6Z/blob/main/Papers/A_Modular_DSP_Architecture_for_EPC_of_π.pdf)
[![DOI](https://img.shields.io/badge/DOI-10.5281/zenodo.17768718-blue)](https://doi.org/10.5281/zenodo.17768718)
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/NachoPeinador/Arquitectura-de-Hibridacion-Algoritmica-en-Z-6Z/blob/main/Notebooks/A_Modular_DSP_Architecture_for_EPC_of_π.ipynb)

**Author:** José Ignacio Peinador Sala  
**Contact:** [joseignacio.peinador@gmail.com](mailto:joseignacio.peinador@gmail.com)  
**ORCID:** [0009-0008-1822-3452](https://orcid.org/0009-0008-1822-3452)  

---

## 🎯 TL;DR: What's This About?

**Problem:** Calculating π at extreme precision hits a "Memory Wall" — parallel algorithms choke on shared memory access.

**Breakthrough:** We discovered that π's calculation can be decomposed using **modular arithmetic (mod 6)**, creating 6 independent computation channels with zero inter-thread communication.

**Key Insight:** This decomposition is grounded in a **formal isomorphism** with polyphase filter banks in Digital Signal Processing (DSP), a bridge between number theory and engineering established in our companion work.

**Result:** 
- ✅ **100 million digits** of π computed with just **6.8 GB RAM** (95% parallelisation efficiency)
- ✅ **Shared-Nothing architecture** with strictly isolated memory per channel
- ✅ **Stride-6 transition leaf** with exact phase correction, compressing recursion depth by 2.6×
- ✅ **Open-source implementation** in Python/gmpy2, executable on Google Colab's free tier

**Why it matters:** This architecture transforms an intrinsically memory-bound problem into a CPU-bound one, enabling near-linear scaling on commodity hardware without specialised HPC infrastructure.

---

## 📖 Executive Summary

This repository hosts the **reference implementation** and experimental validation of the **Hybrid Stride-6 architecture** for extreme-precision computation of π. The architecture exploits the arithmetic structure of the Chudnovsky series by decomposing it into six independent modular channels, each processed by a dedicated worker with its own memory space.

The decomposition is not an ad hoc optimisation but rests on a rigorous mathematical foundation: the **polyphase isomorphism** between modular arithmetic on ℤ/6ℤ and multirate signal processing. This isomorphism guarantees perfect reconstruction (no information loss across channels) and orthogonality (no inter-channel interference).

The architecture is validated through the **100M Barrier Run**: computing 10⁸ digits of π on a resource-constrained Google Colab instance (2 vCPUs, 12 GB RAM) in under 20 minutes, with 95% parallelisation efficiency and a sustained throughput of over 83,000 digits per second.

---

## 🏆 Key Contributions

### 🔬 Theoretical Foundations (Summarised from Companion Work)
- **Polyphase Isomorphism**: Formal proof that modular decomposition of integer-indexed series is equivalent to polyphase decimation in DSP
- **Hexagonal Lattice Connection**: Geometric motivation via the A₂ lattice (densest circle packing in the plane)
- **Perfect Reconstruction Guarantee**: Mathematical proof that the six channels recombine without aliasing or leakage

### ⚡ Computational Architecture
- **Shared-Nothing Design**: Six independent Python processes with strictly isolated memory spaces
- **Stride-6 Transition Leaf**: Processes blocks of 6 consecutive terms in a single operation, reducing recursion tree depth by log₂6 ≈ 2.585
- **Critical Phase Correction**: Direct accumulation of the linear term B(k) prevents off-by-one-stride phase errors

### 📊 Experimental Validation
- **100M Barrier Run**: 100 million digits computed on 12 GB RAM with 95% parallel efficiency
- **Orthogonality Verification**: ℓ² norm of channel terms matches norm of original series to machine precision
- **Reference Comparison**: All 10⁸ digits match y-cruncher reference values exactly

---

## 📈 Performance Highlights

### 🚀 "The 100M Barrier Run" — Extreme Validation
| Metric | Result | Significance |
| :--- | :--- | :--- |
| **Digits Calculated** | 100,000,000 | Exascale-capable architecture |
| **Total Time** | 1,194.32 s (19.90 min) | Sustained performance on cloud hardware |
| **Parallel Efficiency** | **95%** (1.90× speedup) | Near-linear scaling on 2 cores |
| **Peak RAM Usage** | ~6.8 GB | Runs within 12 GB Colab limit |
| **Throughput** | **83,729 digits/second** | Competitive with optimised implementations |
| **Numerical Integrity** | Bit-exact match with y-cruncher | Zero cumulative error |

### 🏗️ Architectural Comparison
| Aspect | Monolithic Binary Splitting | **Hybrid Stride-6 (This Work)** | y-cruncher (State-of-Art) |
| :--- | :--- | :--- | :--- |
| **Memory Pattern** | Contiguous, saturates bus | **Local per core**, optimises cache | Sequential disk I/O |
| **Parallel Model** | Fine-grained synchronisation | **Embarrassingly parallel** (6 processes) | Optimised with locks |
| **Scalability** | Memory-bound | **CPU-bound**, linear to 6 cores | Disk-speed limited |
| **RAM Requirement** | Entire dataset in memory | **Working set reduced 6×** | Uses disk as RAM |
| **Design Philosophy** | Maximise single-thread speed | **Maximise resource efficiency** | Maximise absolute speed |

---

## 🚀 Quick Start & Reproduction

### 1. Instant Online Experiment (Recommended)
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/NachoPeinador/Arquitectura-de-Hibridacion-Algoritmica-en-Z-6Z/blob/main/Notebooks/A_Modular_DSP_Architecture_for_EPC_of_π.ipynb)

Click above to run the complete experimental validation in Google Colab — no installation required!

### 2. Key Experiments to Reproduce
The companion notebook provides step-by-step reproduction of all manuscript claims:

1. **Theoretical Foundation**: Verify the polyphase decomposition and energy conservation
2. **Stride-6 Algorithm**: Test parallel computation with arbitrary precision (100k digits)
3. **100M Barrier Run**: Reproduce the full-scale benchmark (requires ~7 GB RAM)
4. **Performance Analysis**: Measure speedup and parallel efficiency

---

## ⚙️ Technical Implementation Details

### The "Stride-6" Computational Engine
Unlike conventional Binary Splitting (processes terms individually), our engine implements a **compressed transition leaf** that calculates the aggregate effect of 6 consecutive terms:

```python
def stride6_leaf(k_start):
    """Calculate compressed transition for block [k, k+5]"""
    P, Q, B_acc = 1, 1, 0
    for m in range(6):
        n = k_start + m
        P_n, Q_n, B_n = compute_chudnovsky_term(n)
        P *= P_n
        Q *= Q_n
        B_acc += B_n  # Critical phase accumulation
    T_leaf = Q * B_acc  # Correct phase synthesis
    return P, Q, T_leaf
```

**Key Innovation:** Direct accumulation of the linear term B(n) prevents phase drift, preserving arithmetic integrity at any scale.

### Shared-Nothing Architecture
Each of the 6 workers operates in complete memory isolation:
- **Independent address spaces** (no shared memory locks)
- **Local garbage collection** (prevents heap fragmentation)
- **Cache-optimised access patterns** (maximises L1/L2 utilisation)

### Numerical Stability Guarantees
- **Orthogonal decomposition** — zero information loss (verified experimentally)
- **Arbitrary precision backend** (gmpy2) with proven numerical stability
- **Exact phase correction** in the Stride-6 leaf

---

## 📂 Repository Structure

```text
Arquitectura-de-Hibridacion-Algoritmica-en-Z-6Z/
├── 📄 Papers/                                          # Scientific manuscript (PDF)
│   └── A_Modular_DSP_Architecture_for_EPC_of_π.pdf
├── 📓 Notebooks/                                       # Companion Colab notebook
│   └── A_Modular_DSP_Architecture_for_EPC_of_π.ipynb
├── 🖼️ Images/                                          # Generated figures
│   └── Validacion_exaescala.png
└── 📜 README.md
```

---

## 📚 Citation & Academic Use

If this work contributes to your research, please cite:

```bibtex
@article{peinador2026modularDSP,
  title={A Modular DSP Architecture for Extreme-Precision Computation of π},
  author={Peinador Sala, José Ignacio},
  journal={Zenodo},
  year={2026},
  doi = {10.5281/zenodo.17768718},
  url = {https://github.com/NachoPeinador/Arquitectura-de-Hibridacion-Algoritmica-en-Z-6Z}
}
```

The companion theoretical work establishing the polyphase isomorphism is:

```bibtex
@article{peinador2026polyphase,
  title={Polyphase Isomorphism between Modular Arithmetic and Multirate Signal Processing},
  author={Peinador Sala, José Ignacio},
  year={2026},
  publisher={Zenodo},
  doi = {10.5281/zenodo.17680023}
}
```

---

## 🌐 The Broader Research Programme

This architecture is one component of a larger investigation into the computational and physical consequences of the ℤ/6ℤ modular symmetry. Related projects include:

- **[Polyphase Isomorphism](https://github.com/NachoPeinador/Polyphase-Isomorphism-Modular-DSP):** Formal mathematical proof of the isomorphism between modular arithmetic and DSP.
- **[Modular Substrate Theory](https://github.com/NachoPeinador/Modular-Substrate-Theory):** Unified framework for cosmology and hadronic physics.
- **[Topological State Preparation](https://github.com/NachoPeinador/Phase-Pi-Quantum-Prior):** Quantum register initialisation and dissipative protection via ℤ/6ℤ superselection.

**Common Thread:** All projects leverage modular arithmetic (ℤ/6ℤ) as a fundamental organising principle across mathematics, physics, and computation.

---

## ⚖️ Licensing & Usage

### ✅ Academic & Research Use (Free)
Available under **PolyForm Noncommercial License 1.0.0**:
- **Permitted**: Academic research, teaching, personal projects, non-commercial forks
- **Requirements**: Attribution, license preservation, non-commercial use

### ⛔ Commercial Use (License Required)
**Commercial applications require explicit permission**, including:
- Integration into proprietary software products
- Commercial hardware benchmarking services
- SaaS platforms and cloud computing services

> 💼 **For Commercial Licensing Inquiries**:  
> Contact: [joseignacio.peinador@gmail.com](mailto:joseignacio.peinador@gmail.com)  
> Subject: "Commercial License Inquiry — Modular π Architecture"

---

## 🌟 Acknowledgments

This independent research was enabled by:

### Infrastructure & Tools
- **Google Colab** for democratised computational resources
- **Python ecosystem** (gmpy2, NumPy, SciPy, Jupyter) for scientific computing
- **GitHub** for open collaboration infrastructure

### Data & References
- **y-cruncher** for validation benchmarks
- **Digital Signal Processing** community for foundational theory

### Community & Inspiration
- The **open-source scientific community** for collective knowledge advancement
- **Independent researchers worldwide** pushing boundaries outside traditional institutions

---

*Last updated: June 2026 | Version: 3.0 | Status: Actively Maintained*
```

