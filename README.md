<div align="center">
  <h1>LandscapeMT</h1>
  <p><strong>Mapping the Conformational Free Energy Landscape of Molecular Systems</strong></p>
</div>

<p align="center">
  <a href="https://github.com/uibcdf/LandscapeMT/issues"><img alt="Issues" src="https://img.shields.io/github/issues/uibcdf/LandscapeMT"></a>
  <a href="https://github.com/uibcdf/LandscapeMT/blob/main/LICENSE"><img alt="License" src="https://img.shields.io/github/license/uibcdf/LandscapeMT"></a>
  <img alt="Python" src="https://img.shields.io/badge/python-3.10%2B-blue">
  <img alt="Backend" src="https://img.shields.io/badge/backend-Rust%20%7C%20PyTorch-orange">
</p>

## ⛰️ Overview

**LandscapeMT** is the statistical mechanics and conformational exploration engine of the **MolSysSuite** ecosystem. 

Moving beyond single static structures and raw dynamic trajectories, LandscapeMT provides a comprehensive framework to map, analyze, and infer the Free Energy Landscape (FEL) of molecular systems—ranging from small organic compounds and peptides to large protein complexes. By projecting continuous dynamics into discrete metastable states and transition networks, it serves as a critical bridge between molecular dynamics and advanced drug design.

## 🎯 Core Objectives

*   **Metastable State Identification:** Accurately delineate conformational basins to capture the structural diversity of a target.
*   **Kinetic & Thermodynamic Characterization:** Calculate transition rates, energy barriers, and relative free energies between conformational states.
*   **Rational Drug Design Support:** Empower structure-based drug discovery by enabling conformational selection, revealing cryptic pockets, and mapping allosteric communication pathways.
*   **Algorithmic Flexibility:** Unify physics-based simulations with modern generative AI foundation models under a single analytical API.

## 🚀 Key Methodological Pillars

LandscapeMT is structured to explore the conformational space through four synergistic pathways:

### 1. `landscapemt.sampling` (Enhanced Sampling Orchestration)
Instead of reinventing molecular dynamics engines, LandscapeMT leverages `molsysmt` to orchestrate advanced sampling strategies across popular simulators (e.g., OpenMM, Plumed) via microservices or run-scripts:
*   **Adaptive Sampling:** Iterative trajectory spawning from the frontiers of explored conformational space to rapidly discover new basins.
*   **Thermodynamic Methods:** Parallel Tempering and Replica Exchange Molecular Dynamics (REMD) to overcome high kinetic barriers.

### 2. `landscapemt.networks` (Kinetics & Conformational Networks)
Projecting the continuous phase space into robust, discrete mathematical models to understand connectivity:
*   **Markov State Models (MSMs):** Clustering, transition matrix estimation, and implied timescales validation.
*   **Conformational Markov Networks:** Network theory-based analysis of conformational topology.
*   **Transition Path Theory (TPT):** Calculation of reactive fluxes and Mean First Passage Times (MFPT) between critical sub-ensembles (e.g., *apo*-inactive to *apo*-active).

### 3. `landscapemt.ai` (AI-Driven Landscape Inference)
Integrating state-of-the-art foundation models via PyTorch to infer and predict conformational states without exhaustive physical simulation:
*   **Generative Ensembles:** Wrappers for flow-matching and generative models (e.g., BioEmu) to sample Boltzmann-weighted conformations natively.
*   **Latent Space Projections:** Unsupervised learning (Variational Autoencoders) to extract non-linear collective variables directly from structural ensembles.

### 4. `landscapemt.thermo` (Thermodynamic Projections & Reweighting)
Recovering the true unbiased thermodynamics of the system ($F = -k_B T \ln P$):
*   **Reweighting Algorithms:** Efficient implementations of MBAR and WHAM to unbias data from Metadynamics or Umbrella Sampling.
*   **Dimensionality Reduction:** Highly optimized TICA, PCA, UMAP, and t-SNE projections to visualize the Free Energy Landscape.

## 💡 Quick API Preview

LandscapeMT relies on the memory-efficient Rust iterators of `molsysmt` to handle massive out-of-core trajectory processing seamlessly.

```python
import molsysmt as msm
import landscapemt as lmt

# 1. Ingest trajectory via MolSysMT's Rust core
traj = msm.convert("trajectory.xtc", to="molsysmt.Trajectory")

# 2. Extract collective variables and reduce dimensionality
cvs = lmt.thermo.tica(traj, lag_time=10)

# 3. Build a Markov State Model
msm_model = lmt.networks.msm(cvs, n_states=5)

# 4. Extract metastable macrostates for downstream MolSysSuite modules
macrostates = msm_model.get_macrostates()