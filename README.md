# Quantum Volunteer's Dilemma

This repository contains the implementation and experimental validation of the 
n-player Volunteer's Dilemma within the quantum game theory paradigm, using the 
Eisert–Wilkens–Lewenstein (EWL) protocol. Experiments are executed across three 
environments: ideal simulation, real IBM Quantum hardware (ibm_fez), and a 
calibration-based Digital Twin.

This work extends the theoretical framework established in:

> Koh, D. E., Kumar, K., & Goh, S. T. (2025). Quantum volunteer's dilemma.
> *Physical Review Research*, 7(1), 013104.
> https://doi.org/10.1103/PhysRevResearch.7.013104

---

## 📌 Overview

The Volunteer's Dilemma is a game in which at least one participant must incur 
a cost to produce a benefit for the group. If no one volunteers, all players 
receive the worst outcome. This project investigates how quantum strategies — 
enabled by entanglement and parameterized unitary operators — affect cooperation 
dynamics, success probabilities, and payoff structures in multi-player settings.

The primary objectives are:

- Validate the quantum Nash equilibrium strategy on real NISQ hardware.
- Quantify the performance gap between ideal predictions and experimental results.
- Assess whether quantum advantage over the classical Nash equilibrium persists 
  under realistic noise conditions.
- Evaluate the accuracy of a calibration-driven Digital Twin as a noise model.

---

## 🧠 Approach

The implementation follows the **EWL (Eisert–Wilkens–Lewenstein) protocol**, 
where each player is represented by a qubit. The circuit proceeds in four stages:

1. **Entanglement** — An entangling operator J correlates all players' qubits, 
   creating a shared quantum state. This is the key step that distinguishes the 
   quantum game from its classical counterpart.
2. **Strategy Encoding** — Each player applies a parameterized unitary 
   U(θ, φ) = Rz(−π−φ) · Ry(θ) · Rz(π−φ), covering the full SU(2) strategy space.
3. **Disentanglement** — The inverse operator J† maps the entangled state back 
   to the computational basis for measurement.
4. **Measurement & Payoff Evaluation** — Bitstring outcomes are mapped to 
   economic payoffs according to the cost-sharing rules of the Volunteer's Dilemma.

The game is analyzed for **N = 2 to 9 players**, using the symmetric quantum 
Nash equilibrium strategy (θ = 0, φ = π/N).

---

## 📊 Experiments

Hardware experiments were executed on **ibm_fez** via IBM Quantum Runtime 
(SamplerV2 primitive) at transpilation optimization level 0. Results for all 
player counts were submitted as a single batched job and are stored locally for 
reproducibility.

The `experiments/` directory contains one subdirectory per job ID, each holding:

- `{job_id}_result.json` — Sampler V2 primitive results (measurement counts).
- `{job_id}_properties.json` — Backend calibration data (T1, T2, gate errors, 
  readout errors) at the time of execution.
- `backend.pkl` — Serialized backend target object for offline transpilation.
- `backend_config.json` — Backend configuration for noise model reconstruction.

---

## 📁 Repository Structure
```
.
├── experiments/
│   ├── backend.pkl
│   ├── backend_config.json
│   └── {job_id}/
│       ├── {job_id}_result.json
│       └── {job_id}_properties.json
├── Quantum_Volunteer's_Dilemma.ipynb
├── requirements.txt
└── README.md
```
---

## ⚙️ Requirements

```bash
pip install -r requirements.txt
```

Core dependencies: `qiskit==2.4.0`, `qiskit-ibm-runtime==0.46.1`, `qiskit-aer`, 
`pylatexenc`, `scikit-learn`, `scipy`, `matplotlib`, `ipywidgets`.

---

## ▶️ Usage

The notebook is designed primarily for **Google Colab**. Open it directly from 
the repository or upload it manually:
https://colab.research.google.com/
It can also be run locally:

```bash
jupyter notebook "Quantum_Volunteer's_Dilemma.ipynb"
```

### Authentication

The notebook supports two execution paths:

- **IBM Quantum (live):** Enter your API token and instance in Section 0. 
  Credentials are never stored in the notebook.
- **Offline (GitHub fallback):** If no IBM connection is available, the notebook 
  automatically clones this repository and loads stored experimental results. 
  A GitHub Personal Access Token (PAT) is required if the repository is private.

Hardware re-execution is disabled by default to prevent unintended QPU usage. 
All stored results are sufficient to reproduce every figure and metric in the notebook.

---

## 📈 Output

The notebook produces:

- **Interactive payoff explorer** — Full (N, θ, φ) strategy landscape via 
  adjustable sliders (ideal simulation).
- **Fidelity decay plots** — Target-state success probability P(|1⟩^⊗N) vs. N, 
  with linear regression for both QPU and Digital Twin.
- **Payoff comparison** — Experimental QPU results vs. theoretical quantum 
  benchmark and classical Nash equilibrium, with 95% confidence intervals.
- **Noise characterization** — Average undesired-state probability as a 
  function of N.
- **Digital Twin validation** — QPU vs. simulation agreement on success 
  probability and payoff profiles.
- **Statistical metrics** — Coefficient of Variation (CV), MAE, RMSE, and MRE 
  for both QPU and Digital Twin relative to the theoretical benchmark.

---

## 🚧 Notes

- All results are fully reproducible from stored data without an IBM Quantum account.
- The `SEED = 42` reproducibility seed is applied to all simulations.
- The interactive payoff explorer (Section 4) requires `ipywidgets` and a 
  compatible environment (Colab or local Jupyter). It does not render on GitHub's 
  static notebook viewer.
- The Digital Twin captures global noise trends but does not reproduce 
  device-specific fluctuations such as crosstalk or calibration drift.

---

## 📬 Contact

For questions or suggestions, feel free to reach out.
