# Quantum Volunteer's Dilemma

<p align="center">
  <img src="https://img.shields.io/badge/IBM%20Quantum-ibm__kingston-1192E8?style=flat-square" alt="IBM Quantum">
  <img src="https://img.shields.io/badge/Qiskit-2.4.0-6929C4?style=flat-square&logo=qiskit" alt="Qiskit">
  <img src="https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey?style=flat-square" alt="License">
  <img src="https://img.shields.io/badge/Status-Reproducible-success?style=flat-square" alt="Status">
</p>
<p align="center">
  <a href="https://colab.research.google.com/github/GermanDarDiaz/Quantum-Volunteer-s-Dilemma/blob/main/Quantum_Volunteer%27s_Dilemma.ipynb">
    <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open in Colab">
  </a>
  &nbsp;
  <a href="https://github.com/GermanDarDiaz/Quantum-Volunteer-s-Dilemma/blob/main/Quantum_Volunteer's_Dilemma.ipynb">
    <img src="https://img.shields.io/badge/View%20Notebook-GitHub-181717?style=flat-square&logo=github" alt="View on GitHub">
  </a>
</p>

<p align="center">
  Implementation and experimental validation of the <strong>n-player Volunteer's Dilemma</strong> within the quantum game theory paradigm, using the Eisert–Wilkens–Lewenstein (EWL) protocol.<br>
  Experiments are executed across three environments: ideal simulation, real IBM Quantum hardware (<code>ibm_kingston</code>), and a calibration-based Digital Twin.
</p>

---

> **Based on:**
> Koh, D. E., Kumar, K., & Goh, S. T. (2025). Quantum volunteer's dilemma.
> *Physical Review Research*, 7(1), 013104.
> [https://doi.org/10.1103/PhysRevResearch.7.013104](https://doi.org/10.1103/PhysRevResearch.7.013104)

> **Also inspired by:**
> Díaz Agreda, G., Durán Paredes, C. A., Buenaventura Samboni, M., Andrade, J. A., & Cajas Ordoñez, S. (2025). Bridging Theory and Practice in Quantum Game Theory: Optimized Implementation of the Battle of the Sexes with Error Mitigation on NISQ Hardware.
> *2025 IEEE CHILEAN Conference on Electrical, Electronics Engineering, Information and Communication Technologies (CHILECON)*, pp. 1–10. IEEE.
> [IEEE](https://ieeexplore.ieee.org/document/11476221) · [DOI](https://doi.org/10.1109/CHILECON66915.2025.11476221) · [arXiv:2508.09050](https://arxiv.org/abs/2508.09050)

---

## Table of Contents

- [Overview](#-overview)
- [Background](#-background)
- [Approach](#-approach)
- [Experiments](#-experiments)
- [Repository Structure](#-repository-structure)
- [Requirements](#-requirements)
- [Usage](#-usage)
- [Output](#-output)
- [Notes](#-notes)
- [Citation](#-citation)
- [License](#-license)
- [Contact](#-contact)

---

## 📌 Overview

The **Volunteer's Dilemma** is a canonical game in which at least one participant must incur a personal cost to produce a collective benefit. If no one volunteers, all players receive the worst possible outcome. This project investigates how **quantum strategies** — enabled by entanglement and parameterized unitary operators — reshape cooperation dynamics, success probabilities, and payoff structures in multi-player settings.

**Primary objectives:**

- Validate the quantum Nash equilibrium strategy on real NISQ hardware.
- Quantify the performance gap between ideal theoretical predictions and experimental results.
- Assess whether quantum advantage over the classical Nash equilibrium persists under realistic noise conditions.
- Evaluate the accuracy of a calibration-driven Digital Twin as a practical noise model.

---

## 🧪 Background

Classical game theory predicts that, in the Volunteer's Dilemma, rational players adopt a mixed Nash equilibrium where each volunteers with a fixed probability. The resulting collective success probability decreases as the number of players grows — a counterintuitive effect sometimes called the **"bystander effect"** in game-theoretic form.

Quantum game theory offers an alternative: by entangling players' strategies, cooperation can be incentivized at the quantum Nash equilibrium. This repository empirically tests whether this theoretical advantage survives the noise inherent in current quantum hardware, for player counts **N = 2 to 9**.

---

## 🧠 Approach

The implementation follows the **EWL (Eisert–Wilkens–Lewenstein) protocol**, where each player is represented by one qubit. The circuit proceeds in four stages:

```
|0⟩^⊗N ──[ J ]──[ U(θ,φ) ]⊗N──[ J† ]──[ Measure ]
```

| Stage | Description |
|---|---|
| **1. Entanglement** | Operator `J` correlates all qubits into a shared entangled state, distinguishing the quantum game from its classical counterpart. |
| **2. Strategy Encoding** | Each player applies `U(θ, φ) = Rz(−π−φ) · Ry(θ) · Rz(π−φ)`, spanning the full SU(2) strategy space. |
| **3. Inverse Operator `J†`** | The inverse operator `J†` maps the entangled state back to the computational basis. |
| **4. Measurement & Payoff** | Bitstring outcomes are mapped to economic payoffs according to the Volunteer's Dilemma cost-sharing rules. |

The **symmetric quantum Nash equilibrium** is parametrized as `(θ = 0, φ = π/N)`, analyzed across N = 2 to 9 players.

---

## 📊 Experiments

Hardware experiments were executed on **`ibm_kingston`** (Heron r2 family) via IBM Quantum
Runtime (SamplerV2 primitive). Four independent jobs were submitted — one per transpiler
optimization level (0–3) — each covering N = 2 to 9 players with **20 repetitions per
circuit** and **2048 shots per repetition**. Results are stored locally for full offline
reproducibility.

Each `run_0X/` subdirectory in `experiments/` corresponds to one optimization level and contains:

| File | Description |
|---|---|
| `measurement_results.json` | SamplerV2 primitive results (measurement counts per bitstring, all repetitions). |
| `properties.json` | Backend calibration snapshot (T1, T2, gate errors, readout errors) at execution time. |
| `backend.pkl` | Serialized backend target object for offline transpilation. |
| `backend_configuration.json` | Backend configuration for noise model reconstruction. |
| `job_metadata.json` | Job identifiers, backend name, timestamps, and execution parameters. |

---

## 📁 Repository Structure

```
.
├── experiments/
│   ├── run_01/                        # Optimization level 0
│   │   ├── backend.pkl                # Serialized IBM backend target
│   │   ├── backend_configuration.json # Backend configuration for noise model
│   │   ├── job_metadata.json          # Job ID, backend, timestamps, parameters
│   │   ├── measurement_results.json   # SamplerV2 bitstring counts (20 reps × N)
│   │   └── properties.json            # T1, T2, gate/readout errors at runtime
│   ├── run_02/                        # Optimization level 1
│   │   └── ...
│   ├── run_03/                        # Optimization level 2
│   │   └── ...
│   └── run_04/                        # Optimization level 3
│       └── ...
├── Quantum_Volunteer's_Dilemma.ipynb
├── requirements.txt
├── LICENSE
└── README.md
```

---

## ⚙️ Requirements

Python 3.9+ is recommended. Install all dependencies with:

```bash
pip install -r requirements.txt
```

**Core dependencies:**

| Package | Version |
|---|---|
| `qiskit` | 2.4.0 |
| `qiskit-ibm-runtime` | 0.46.1 |
| `qiskit-aer` | latest |
| `mthree` | latest |
| `pylatexenc` | latest |
| `scikit-learn` | latest |
| `scipy` | latest |
| `matplotlib` | latest |
| `ipywidgets` | latest |

---

## ▶️ Usage

The notebook is designed primarily for **Google Colab**. Open it directly or upload manually:

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/GermanDarDiaz/Quantum-Volunteer-s-Dilemma/blob/main/Quantum_Volunteer%27s_Dilemma.ipynb)
[![View on GitHub](https://img.shields.io/badge/View%20Notebook-GitHub-181717?style=flat-square&logo=github)](https://github.com/GermanDarDiaz/Quantum-Volunteer-s-Dilemma/blob/main/Quantum_Volunteer's_Dilemma.ipynb)

It can also be run locally:

```bash
jupyter notebook "Quantum_Volunteer's_Dilemma.ipynb"
```

### Authentication

The notebook supports two execution paths:

**IBM Quantum (live hardware):**
Paste your API token and instance into the credentials cell (Section 1). The notebook calls `QiskitRuntimeService.save_account()` directly — credentials are not stored in the notebook file.

```python
QiskitRuntimeService.save_account(
    channel="ibm_quantum_platform",
    token="",      # paste your IBM Quantum API token here
    instance="",   # paste your instance (e.g. "ibm-q/open/main")
    overwrite=True,
    set_as_default=True
)
```

**Offline (GitHub fallback):**
If no IBM Quantum connection is available, the notebook automatically clones this repository and loads stored experimental results. A GitHub Personal Access Token (PAT) is required if the repository is private.

> ⚠️ **Hardware re-execution is disabled by default** to prevent unintended QPU usage. All stored results are sufficient to reproduce every figure and metric in the notebook.

---

## 📈 Output

| Output | Description |
|---|---|
| **Interactive Payoff Explorer** | Full (N, θ, φ) strategy landscape via sliders (ideal simulation). |
| **Fidelity Decay Plots** | Target-state fidelity P(\|1⟩^⊗N) vs. N with linear regression and 95% t-Student CIs, for raw QPU and readout-corrected results. |
| **Target-State Weighted Payoff** | Weighted payoff contribution P(\|1⟩^⊗N)·(b−c/N) vs. theoretical benchmark, with analytically derived CIs. |
| **Noise Characterization** | Average undesired-state probability vs. N, with and without readout correction. |
| **Hamming Distance Distribution** | Probability distribution of undesired states grouped by Hamming distance from the target state, aggregated over 20 repetitions. |
| **Global Average Payoff** | QPU and corrected payoffs vs. quantum benchmark and classical Nash equilibrium, with 95% t-Student CIs. |
| **Digital Twin Scaling** | Noisy simulation fidelity with linear regression. 20 seeds are used to match QPU repetition count for statistical comparison and to verify model stability under stochastic variation. |
| **KS Test Validation** | Kolmogorov–Smirnov test comparing QPU and Digital Twin payoff distributions for each N. |
| **Payoff Benchmark** | Overlay of theoretical, Digital Twin, QPU (raw), QPU (corrected), and classical Nash payoff profiles. |
| **Benchmark Error Metrics** | AE, and RE for QPU (raw), QPU (corrected), and Digital Twin vs. theoretical prediction. |
| **Readout Correction Impact** | Per-N absolute and relative payoff offset introduced by readout error correction. |
| **Gate Count Analysis** | ECR gate count and circuit depth per optimization level and N, with fidelity decay slope for correlation analysis. |

---

## 🚧 Notes

- All results are **fully reproducible from stored data** without an IBM Quantum account.
- The global reproducibility seed is `SEED = 42`, applied to all simulations. Hardware repetitions are independent physical executions with no seed.
- Readout error correction is applied in post-processing using the `mthree` library, calibrated from the `properties.json` file of each run.
- The interactive payoff explorer (Section 4) requires `ipywidgets` and a compatible environment (Colab or local Jupyter). It **does not render** on GitHub's static notebook viewer.
- The Digital Twin captures global noise trends but does not reproduce device-specific fluctuations such as crosstalk or calibration drift.
- Four optimization levels (0–3) are analyzed independently. Results are organized by `run_0X/` folder.
- The Digital Twin is executed with 20 independent random seeds. Low variance across seeds confirms that the model's stochastic component (shot noise) is small relative to the systematic noise structure, validating its use as a stable reference for QPU comparison.

---

## 📄 Citation

If you use this repository, data, or code in your research, **please cite both this work and the original paper**:

**This repository:**
```bibtex
@software{quantum_volunteers_dilemma_repo,
  author       = {D{\'i}az Agreda, Germ{\'a}n and Andrade Hoyos, Jhon Alejandro and
                  Dur{\'a}n, Carlos Andr{\'e}s and Cajas Ordo{\~n}ez, Sebasti{\'a}n Andr{\'e}s and
                  Hebdon, Noah Dane and Goh, Siong Thye and Koh, Dax Enshan},
  title        = {Quantum Volunteer's Dilemma: Experimental Validation via EWL Protocol on IBM Quantum Hardware},
  year         = {2025},
  url          = {https://github.com/GermanDarDiaz/Quantum-Volunteer-s-Dilemma},
  note         = {Experimental implementation and Digital Twin validation of the quantum Volunteer's Dilemma on ibm\_kingston}
}
```

**Original theoretical work:**
```bibtex
@article{koh2025quantum,
  author  = {Koh, Dax Enshan and Kumar, Kaavya and Goh, Siong Thye},
  title   = {Quantum volunteer's dilemma},
  journal = {Physical Review Research},
  volume  = {7},
  number  = {1},
  pages   = {013104},
  year    = {2025},
  doi     = {10.1103/PhysRevResearch.7.013104}
}
```

---

## ⚖️ License

This repository is licensed under the **Creative Commons Attribution 4.0 International License (CC BY 4.0)**.

You are free to:
- **Share** — copy and redistribute the material in any medium or format.
- **Adapt** — remix, transform, and build upon the material for any purpose, even commercially.

Under the following terms:
- **Attribution** — You must give appropriate credit, provide a link to the license, and indicate if changes were made. See the [Citation](#-citation) section above for the correct format.

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)

See the [`LICENSE`](./LICENSE) file for full terms.

---

## 📬 Contact

For questions, bug reports, or collaboration proposals, feel free to open an [Issue](../../issues) or reach out directly via the repository.
