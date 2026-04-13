# Computational Prioritization and Analysis of Phytochemicals Targeting hnRNPA1 UP1 Domain

---

## Overview

This repository contains the complete computational workflow used for **high-throughput virtual screening (HTVS), ligand prioritization, docking analysis, and spatial characterization** of phytochemicals targeting the hnRNPA1 UP1 domain.

The workflow integrates:
- Blind docking using AutoDock Vina
- Descriptor-based ligand filtering from PubChem
- Heuristic composite prioritization
- Pose-level spatial analysis
- Clustering and interaction profiling

⚠️ This pipeline is designed for **ligand prioritization for downstream analysis (e.g., MD simulations)** and is **not intended as a predictive binding affinity model**.

---

## Repository Structure


SMA/
│
├── data/
│ ├── docking_results.csv
│ ├── pubchem_descriptors.csv
│ └── merged_dataset.csv
│
├── notebooks/
│ ├── Ligand_ranking_script_01.ipynb
│ ├── Ligand_ranking_script_02.ipynb
│
├── results/
│ ├── all_scores_merged.csv
│ ├── Top100_Ligands_Composite_OptionA.csv
│ ├── top10_*.csv
│
├── scripts/
│ ├── pose_analysis.py
│ ├── clustering_analysis.py
│
└── README.md




---

## Workflow Description

### 1. Ligand Library Preparation
- 2,847 KEGG-linked phytochemicals retrieved from PubChem
- Preprocessing:
  - Open Babel
  - PyRx
- Energy minimization: MMFF94
- Protonation: pH 7.4
- Stereochemistry preserved

---

### 2. High-Throughput Virtual Screening (HTVS)

Docking performed using AutoDock Vina:

- Exhaustiveness: 25  
- Output modes: 9 per ligand  
- Blind docking grid covering full protein surface  
- Target structures:
  - 2LYV (primary HTVS)
  - 6DCL (validation)

Output:
- 23,184 docking poses

---

### 3. Ligand Prioritization Strategy

A **heuristic composite ranking index** was used for screening triage.

#### Components:

1. **Docking Energy**
   - From AutoDock Vina
   - Absolute value used for normalization

2. **Pose-Consistency Indicator**
   - Derived from Vina lower-bound RMSD
   - Formula:
     ```
     Pose_Consistency = 1 / (RMSD_lb + ε)
     ```
   - Used only to assess reproducibility of docking solutions

3. **Descriptor-Based Tractability Proxy**
   - Molecular weight
   - XlogP
   - TPSA
   - Hydrogen bond donors/acceptors
   - Rotatable bonds

#### Composite Score:


Composite Score =
0.5 * Z(Docking Energy)

0.2 * Z(Pose Consistency)
0.3 * Z(Descriptor Proxy)



⚠️ Important:
- This score is **heuristic**
- Not a predictive or validated scoring model
- Used only for ranking candidates for downstream analysis

---

### 4. Pose-Level Spatial Analysis

- Center of Mass (COM) calculated for each ligand pose
- Interdomain cleft defined by residues 88–101
- Threshold: ≤ 8 Å

#### Null Model
- Random sampling within docking grid
- Used to estimate expected spatial distribution

---

### 5. Clustering and Interaction Profiling

- Clustering: DBSCAN
  - ε = 3.5 Å
  - min_samples = 2
- PCA used for visualization
- Contact metric:
  - Fraction of ligand atoms within 4.5 Å of pocket residues

---

### 6. Focused Docking Validation

Selected ligands:
- Withanolide D
- Ginkgetin
- Amentoflavone
- Rutin

Comparison:
- Blind docking vs site-focused docking
- COM distance analysis
- Clustering behavior

---

## Requirements

### Software
- Python 3.10+
- Jupyter Notebook / Google Colab

### Python Libraries

numpy
pandas
scikit-learn
scipy
matplotlib
seaborn


### External Tools
- AutoDock Vina
- Open Babel
- PyRx

---

## How to Run

1. Clone repository:

2. git clone https://github.com/romenmeitei/SMA.git

cd SMA


2. Open notebooks:

3. 
3. Run:
- `Ligand_ranking_script_01.ipynb`
- `Ligand_ranking_script_02.ipynb`

4. Results will be generated in:

5. 
---

## Output Files

| File | Description |
|------|------------|
| `Top100_Ligands_Composite_OptionA.csv` | Top-ranked ligands |
| `all_scores_merged.csv` | Full scoring dataset |
| `top10_*.csv` | Top docking poses |
  
---

## Limitations

- Docking scores are approximate, not absolute binding affinities  
- Composite score is heuristic and not validated  
- Pose-consistency metric is not physical stability  
- No experimental validation included  

---

## Reproducibility

- All scripts are provided
- Workflow compatible with Google Colab
- Deterministic pipeline

---

## Citation

If you use this workflow, please cite:

Meitei Lourembam et al.  
*Ensemble Docking and Molecular Dynamics Reveal Dynamic Modulation of hnRNPA1 UP1 Interdomain Behavior by a Natural Product*

---

## Contact

Dr. Romen Meitei Lourembam  
Institute of Bioresources and Sustainable Development (IBSD), India
