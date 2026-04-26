# Quantum Volunteer's Dilemma

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.9%2B-blue?style=flat-square&logo=python" alt="Python">
  <img src="https://img.shields.io/badge/Qiskit-2.4.0-6929C4?style=flat-square&logo=qiskit" alt="Qiskit">
  <img src="https://img.shields.io/badge/IBM%20Quantum-ibm__fez-1192E8?style=flat-square" alt="IBM Quantum">
  <img src="https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey?style=flat-square" alt="License">
  <img src="https://img.shields.io/badge/Status-Reproducible-success?style=flat-square" alt="Status">
</p>

<p align="center">
  Implementation and experimental validation of the <strong>n-player Volunteer's Dilemma</strong> within the quantum game theory paradigm, using the Eisert–Wilkens–Lewenstein (EWL) protocol.<br>
  Experiments are executed across three environments: ideal simulation, real IBM Quantum hardware (<code>ibm_fez</code>), and a calibration-based Digital Twin.
</p>

---

> **Based on:**
> Koh, D. E., Kumar, K., & Goh, S. T. (2025). Quantum volunteer's dilemma.
> *Physical Review Research*, 7(1), 013104.
> [https://doi.org/10.1103/PhysRevResearch.7.013104](https://doi.org/10.1103/PhysRevResearch.7.013104)

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
| **3. Disentanglement** | The inverse operator `J†` maps the entangled state back to the computational basis. |
| **4. Measurement & Payoff** | Bitstring outcomes are mapped to economic payoffs according to the Volunteer's Dilemma cost-sharing rules. |

The **symmetric quantum Nash equilibrium** is parametrized as `(θ = 0, φ = π/N)`, analyzed across N = 2 to 9 players.

---

## 📊 Experiments

Hardware experiments were executed on **`ibm_fez`** via IBM Quantum Runtime (SamplerV2 primitive) at transpilation optimization level 0. All player counts were submitted as a single batched job. Results are stored locally for full offline reproducibility.

Each job subdirectory in `experiments/` contains:

| File | Description |
|---|---|
| `{job_id}_result.json` | SamplerV2 primitive results (measurement counts per bitstring). |
| `{job_id}_properties.json` | Backend calibration snapshot (T1, T2, gate errors, readout errors) at execution time. |
| `backend.pkl` | Serialized backend target object for offline transpilation. |
| `backend_config.json` | Backend configuration for noise model reconstruction. |

---

## 📁 Repository Structure

```
.
├── experiments/
│   ├── backend.pkl                  # Serialized IBM backend target
│   ├── backend_config.json          # Backend configuration for noise model
│   ├── run_01/                      # QPU execution run 1 (N=2–9, ibm_fez)
│   │   ├── job_metadata.json        # Job info: ID, backend, cost, timestamps
│   │   ├── backend_calibration.json # T1, T2, gate/readout errors at runtime
│   │   └── measurement_results.json # SamplerV2 bitstring counts
│   ├── run_02/                      # QPU execution run 2
│   │   ├── job_metadata.json
│   │   ├── backend_calibration.json
│   │   └── measurement_results.json
│   ├── run_03/                      # QPU execution run 3
│   │   ├── job_metadata.json
│   │   ├── backend_calibration.json
│   │   └── measurement_results.json
│   └── run_04/                      # QPU execution run 4
│       ├── job_metadata.json
│       ├── backend_calibration.json
│       └── measurement_results.json
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
| `pylatexenc` | latest |
| `scikit-learn` | latest |
| `scipy` | latest |
| `matplotlib` | latest |
| `ipywidgets` | latest |

---

## ▶️ Usage

The notebook is designed primarily for **Google Colab**. Open it directly or upload manually:

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)

It can also be run locally:

```bash
jupyter notebook "Quantum_Volunteer's_Dilemma.ipynb"
```

### Authentication

The notebook supports two execution paths:

**IBM Quantum (live hardware):**
Enter your API token and instance in Section 0. Credentials are never stored in the notebook.

```python
# Section 0 — IBM Quantum credentials (not stored)
IBM_TOKEN = "your_token_here"
IBM_INSTANCE = "your_instance_here"
```

**Offline (GitHub fallback):**
If no IBM Quantum connection is available, the notebook automatically clones this repository and loads stored experimental results. A GitHub Personal Access Token (PAT) is required if the repository is private.

> ⚠️ **Hardware re-execution is disabled by default** to prevent unintended QPU usage. All stored results are sufficient to reproduce every figure and metric in the notebook.

---

## 📈 Output

Running the notebook end-to-end produces the following analyses and visualizations:

| Output | Description |
|---|---|
| **Interactive Payoff Explorer** | Full (N, θ, φ) strategy landscape via sliders (ideal simulation). |
| **Fidelity Decay Plots** | Target-state success probability P(\|1⟩^⊗N) vs. N, with linear regression for QPU and Digital Twin. |
| **Payoff Comparison** | Experimental QPU results vs. theoretical quantum benchmark and classical Nash equilibrium, with 95% confidence intervals. |
| **Noise Characterization** | Average undesired-state probability as a function of N. |
| **Digital Twin Validation** | QPU vs. simulation agreement on success probability and payoff profiles. |
| **Statistical Metrics** | Coefficient of Variation (CV), MAE, RMSE, and MRE for QPU and Digital Twin relative to the theoretical benchmark. |

---

## 🚧 Notes

- All results are **fully reproducible from stored data** without an IBM Quantum account.
- The global reproducibility seed is `SEED = 42`, applied to all simulations.
- The interactive payoff explorer (Section 4) requires `ipywidgets` and a compatible environment (Colab or local Jupyter). It **does not render** on GitHub's static notebook viewer.
- The Digital Twin captures global noise trends but does not reproduce device-specific fluctuations such as crosstalk or calibration drift.

---

## 📄 Citation

If you use this repository, data, or code in your research, **please cite both this work and the original paper**:

**This repository:**
```bibtex
@software{quantum_volunteers_dilemma_repo,
  author       = {[Author(s)]},
  title        = {Quantum Volunteer's Dilemma: Experimental Validation via EWL Protocol on IBM Quantum Hardware},
  year         = {2025},
  url          = {https://github.com/[username]/[repository]},
  note         = {Experimental implementation and Digital Twin validation of the quantum Volunteer's Dilemma}
}
```

**Original theoretical work:**
```bibtex
@article{koh2025quantum,
  author  = {Koh, Dax Enshan and Kumar, Kaavya and Goh, Sheng Tao},
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
