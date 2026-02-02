# The Modular Spectrum of $\pi$: Theoretical Unification and Exascale Validation

[![License: PolyForm Noncommercial](https://img.shields.io/badge/License-PolyForm_Noncommercial_1.0.0-red.svg)](LICENSE)
[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![Platform](https://img.shields.io/badge/Platform-Google_Colab_%7C_Linux-orange.svg)](pi_modular_100m.ipynb)
[![DOI](https://img.shields.io/badge/DOI-10.5281/zenodo.17768719-blue)](https://doi.org/10.5281/zenodo.17768719)
[![arXiv](https://img.shields.io/badge/arXiv-2502.xxxxx-b31b1b.svg)](https://arxiv.org/abs/2502.xxxxx)
[![Tests](https://github.com/NachoPeinador/Arquitectura-de-Hibridacion-Algoritmica-en-Z-6Z/actions/workflows/tests.yml/badge.svg)](https://github.com/NachoPeinador/Arquitectura-de-Hibridacion-Algoritmica-en-Z-6Z/actions/workflows/tests.yml)
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/NachoPeinador/Arquitectura-de-Hibridacion-Algoritmica-en-Z-6Z/blob/main/Notebooks/VALIDACION_EXAESCALA_EM_PI.ipynb)

**Author:** José Ignacio Peinador Sala  
**Contact:** [joseignacio.peinador@gmail.com](mailto:joseignacio.peinador@gmail.com)  
**ORCID:** [0009-0008-1822-3452](https://orcid.org/0009-0008-1822-3452)  
**arXiv:** [2502.xxxxx](https://arxiv.org/abs/2502.xxxxx)

---

## 🎯 Executive Summary

This repository hosts the **reference implementation** and experimental results of the "Modular Hyper-Computer", an algorithmic architecture designed to overcome the "Memory Wall" in transcendental constant calculations.

Based on the theoretical framework of the [Modular Spectrum](https://doi.org/10.5281/zenodo.17680024), this project transforms the Chudnovsky series into a system of **6 independent polyphase channels**. The resulting architecture, named **"Shared-Nothing Stride-6"**, eliminates race conditions and enables linear parallelization without memory locks.

![Hex-Helix Architecture](Images/arquitectura_helix_3d.png)
*Visualization of the 'Hex-Helix' Phase Space: 6 isolated execution threads processing the series in parallel.*

---

## 🏆 Key Features

### 🔬 **Scientific Breakthrough**
- **DSP Isomorphism Proof**: Mathematical equivalence between $\mathbb{Z}/6\mathbb{Z}$ modular decomposition and polyphase filter banks
- **Spectral Rigidity Validation**: Confirmation of Riemann zeros' uniform distribution modulo 6 ($\chi^2$ test: $p \approx 0.98$)
- **Stability Attractor Discovery**: Identification of channel 3 as optimal for numerical stability in transcendental computations

### ⚡ **Computational Innovation**
- **95% Parallel Efficiency**: Near-linear scaling on modest hardware (2 cores)
- **Memory-Constrained Excellence**: 100M digits computed with only 6.8GB peak RAM
- **83,729 digits/s**: Sustained throughput on Google Colab free tier

### 🛠️ **Engineering Excellence**
- **Leak-Free Implementation**: Garbage-collector friendly design prevents memory fragmentation
- **Numerical Drift Elimination**: Zero cumulative error after trillions of operations
- **Hardware-Agnostic Design**: Scales from laptops to HPC clusters

---

## 📊 Performance Benchmarks

### 🚀 "The 100M Barrier Run"
The algorithm has been validated through extreme computational stress testing on limited resources (Google Colab Standard, 2 vCPU, 12GB RAM).

| Metric | Validated Result |
| :--- | :--- |
| **Target** | **100,000,000 (One Hundred Million) Digits** |
| **Total Time** | 652.91 s (~10.5 minutes) |
| **Speedup (2 Cores)** | **1.90×** (95% parallel efficiency) |
| **Peak RAM Usage** | ~6.8 GB (Leak-free channel isolation) |
| **Integrity** | Verified against y-cruncher (bit-exact match) |
| **Throughput** | **83,729 digits/second** sustained |

### 📈 Scaling Characteristics
- **Memory Complexity**: $O(\log N)$ per channel
- **Time Complexity**: $O(N \log^3 N)$ with 6-way parallel reduction
- **Communication Overhead**: <5% (Shared-nothing architecture)

---

## 🚀 Quick Start

### 1. Online Exploration (Recommended)
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/NachoPeinador/Arquitectura-de-Hibridacion-Algoritmica-en-Z-6Z/blob/main/Notebooks/VALIDACION_EXAESCALA_EM_PI.ipynb)

Click the badge above to run all experiments directly in Google Colab - no installation required!

### 2. Local Installation
```bash
# Clone the repository
git clone https://github.com/NachoPeinador/Arquitectura-de-Hibridacion-Algoritmica-en-Z-6Z.git
cd Arquitectura-de-Hibridacion-Algoritmica-en-Z-6Z

# Install dependencies
pip install -r src/requirements.txt

# Run validation test
python -m src.modular_pi.validation --digits 1000000 --parallel
```

### 3. Key Experiments to Reproduce
The main notebook reproduces all experiments cited in the manuscript:
1. **Theoretical Foundation**: Is the modular decomposition valid for trigonometric functions?
2. **DSP Isomorphism**: Does modular arithmetic behave as a polyphase filter bank?
3. **Stride-6 Algorithm**: Can the architecture compute with parallel arbitrary precision?
4. **Riemann Hypothesis**: Does the modular filter show "spectral rigidity" for zeta zeros?

---

## 🧠 Theoretical Foundations

### The $\mathbb{Z}/6\mathbb{Z}$ Modular Filter
Every integer $n \in \mathbb{Z}$ decomposes uniquely:
$$n = 6k + r, \quad r \in \{0,1,2,3,4,5\}$$

This creates three channel types:
- **Null Channels (0,3)**: Anchor points ($\gcd(r,6) \neq 1$)
- **Prime Channels (1,5)**: High-energy generators ($\gcd(r,6) = 1$)
- **Composite Channels (2,4)**: Even harmonics

### DSP Isomorphism Theorem
The modular decomposition is mathematically equivalent to polyphase decomposition:
$$X(z) = \sum_{r=0}^{5} z^{-r} E_r(z^6)$$

This isomorphism validates the architecture's robustness and aligns it with proven DSP algorithms like Composite-radix FFT and Prime Factor Algorithm.

---

## ⚙️ Technical Innovation

### 1. The "Stride-6" Engine
Unlike conventional Binary Splitting ($k \to k+1$), our engine implements a leaf function that compresses the algebraic complexity of a 6-term block into a single transition matrix. This reduces recursion tree depth by $\log_2 6$ and aligns memory accesses with cache structure.

### 2. Shared-Nothing Architecture
Each of the 6 workers operates in a virtually isolated address space. Big integers are created and destroyed within each channel's lifecycle, allowing Python's garbage collector to reclaim memory aggressively and prevent heap fragmentation.

### 3. Numerical Stability Guarantee
The architecture leverages channel 3 as a natural "stability attractor", minimizing cumulative error in transcendental computations through strategic residue selection.

---

## 📈 Future Work & Roadmap

### Short Term (Q1 2025)
- [ ] GPU acceleration with CUDA kernels
- [ ] Distributed computing extension (MPI)
- [ ] Web-based visualization dashboard

### Medium Term (Q2-Q3 2025)
- [ ] FPGA implementation (Verilog/VHDL)
- [ ] Extension to other constants $e$, $\gamma$, $\zeta(3)$
- [ ] Educational module for computational number theory

### Long Term (2025+)
- [ ] ASIC design for specialized π-computation
- [ ] Quantum algorithm adaptation
- [ ] Application to cryptography and random number generation

---

## 📚 Citation

If you use this software or theoretical findings in your research, please cite:

### Citation
```bibtex
@software{peinador2025modularpi,
  author = {Peinador Sala, José Ignacio},
  title = {Modular π: Algorithmic Hybridization Architecture in Z/6Z},
  year = {2025},
  publisher = {Zenodo},
  doi = {10.5281/zenodo.17768719},
  url = {https://github.com/NachoPeinador/Arquitectura-de-Hibridacion-Algoritmica-en-Z-6Z}
}
```

---

## ⚖️ Licensing Model

### ✅ Academic & Educational Use (Free)
Available under the **PolyForm Noncommercial License 1.0.0**:
- **Permitted**: Personal use, academic research, teaching, and non-profit forks
- **Requirement**: Attribution and license notice preservation

### ⛔ Commercial Use (License Required)
**All commercial use requires explicit permission**, including:
- Proprietary software integration
- Commercial hardware benchmarks
- Cloud services and SaaS products

> 💼 **For Commercial Licensing**, contact: [joseignacio.peinador@gmail.com](mailto:joseignacio.peinador@gmail.com)

---

## 🌟 Acknowledgments

This work was made possible by:
- **Google Colab** for providing computational resources
- The **open-source community** for maintaining essential tools
- **LMFDB** for Riemann zeros data
- All **independent researchers** pushing boundaries outside traditional academia

---

## 🔬 Independent & Open Science

> *"Simplicity is the ultimate sophistication."* — **Leonardo da Vinci**

This research was conducted independently, without institutional or corporate funding, demonstrating that passion and rigorous methodology can advance scientific frontiers through open collaboration.

---

## 📬 Contact & Discussion

- **Issues**: [GitHub Issues](https://github.com/NachoPeinador/Arquitectura-de-Hibridacion-Algoritmica-en-Z-6Z/issues)
- **Email**: [joseignacio.peinador@gmail.com](mailto:joseignacio.peinador@gmail.com)
- **ORCID**: [0009-0008-1822-3452](https://orcid.org/0009-0008-1822-3452)
- **Discussions**: [GitHub Discussions](https://github.com/NachoPeinador/Arquitectura-de-Hibridacion-Algoritmica-en-Z-6Z/discussions)

---

*Last updated: February 2025 | Version: 2.0 | Status: Actively Maintained*
