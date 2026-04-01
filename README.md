# Robust Docking Analysis: Blind vs Site-Focused Validation

## Overview

This repository contains the analysis workflow used to evaluate the robustness of molecular docking results against grid-size effects in the hnRNPA1 UP1 domain.

The study investigates whether ligand localization to the interdomain cleft is an artifact of a large blind-docking grid or represents a genuine binding preference.

---

## Key Objective

To determine whether docking poses obtained using a large blind-docking grid remain consistent when compared with a smaller, site-focused grid centered on the interdomain cleft.

---

## Methodology

### Docking Conditions

Two docking strategies were compared:

1. **Blind Docking (Large Grid)**
   - Grid size: 59.01 × 63.36 × 61.05 Å
   - Covers the entire UP1 surface
   - Enables unbiased identification of potential binding sites

2. **Site-Focused Docking (Small Grid)**
   - Centered on interdomain cleft (residues 75–115)
   - Reduced search space
   - Used for validation of binding site specificity

---

## Analysis Workflow

The notebook performs the following steps:

### 1. Parsing Docking Outputs
- Reads AutoDock Vina `.pdbqt` output files
- Extracts all docking poses (MODEL entries)
- Retrieves binding scores and atomic coordinates

### 2. Defining Binding Pocket
- Interdomain cleft defined as residues **75–115 (chain A)** of 2LYV
- Pocket atoms extracted from receptor structure

### 3. Distance Calculation
- Computes **minimum distance between ligand atoms and pocket atoms**
- Provides physically meaningful binding proximity metric

### 4. Pose Classification
- Classifies poses as “near pocket” using a distance cutoff (default: 5 Å)

### 5. Statistical Analysis
- Compares blind vs focused docking using:
  - Mean and median distances
  - Standard deviation
  - Percentage of poses near pocket
- Performs statistical tests:
  - Mann–Whitney U test (primary)
  - Welch’s t-test (reference)

### 6. Visualization
- Boxplots of pose-to-pocket distances
- Scatter plots of all poses
- Bar plots of % poses localized to pocket

---

## Key Findings

- Docking poses from the large blind grid are already predominantly localized in the interdomain cleft
- Focused docking improves spatial convergence without altering binding site identity
- Statistical analysis confirms that ligand localization is not an artifact of grid size

---

## Requirements

- Python 3.x
- NumPy
- Pandas
- Matplotlib
- SciPy

---

## Usage

Run the notebook in Google Colab:

1. Upload:
   - Receptor `.pdbqt` file
   - Docking output `.pdbqt` files (blind + focused)

2. Update file paths in the notebook

3. Execute all cells

---

## Output Files

- `docking_pose_localization_results.csv`
- `docking_pose_localization_summary.csv`
- `pose_distance_boxplot.png`
- `pose_distance_scatter.png`
- `pose_near_pocket_percent.png`
- `pose_distance_stats.csv`

---

## Reproducibility

This workflow is designed to be fully reproducible and directly corresponds to the analysis described in the manuscript.

---

## Citation

If you use this workflow, please cite the associated manuscript and provide a link to this repository.

---

## Author

Romen Meitei
