# Integrated In Silico Drug Discovery Pipeline for EGFR Inhibitors in NSCLC


This project presents a complete **in silico drug discovery workflow** targeting the **Epidermal Growth Factor Receptor (EGFR)** for the treatment of **Non-Small Cell Lung Cancer (NSCLC)**.

The pipeline integrates:

- Bioactivity data retrieval and curation
- QSAR modeling
- SAR and chemical space analysis
- Virtual screening
- Molecular docking
- ADMET/toxicity filtering
- Multi-parameter lead prioritization

---

# Project Objective

The objective of this project was to design and implement a reproducible computational drug discovery pipeline capable of identifying and prioritizing potential EGFR inhibitors using cheminformatics, machine learning, molecular docking, and ADMET analysis.

---

# Disease & Target Selection

## Disease
 **Lung Cancer **

## Molecular Target
**Epidermal Growth Factor Receptor (EGFR)**  
ChEMBL ID: `CHEMBL203`

### Why EGFR?

EGFR is a receptor tyrosine kinase frequently mutated in NSCLC. Activating mutations result in uncontrolled signaling through pathways such as:

- MAPK
- PI3K/AKT

These mutations promote:

- Tumor proliferation
- Survival
- Reduced apoptosis



---

# Complete Workflow

```text
Target Selection
        ↓
Bioactivity Data Retrieval & Curation
        ↓
Descriptor & Fingerprint Generation
        ↓
QSAR Modeling & Validation
        ↓
SAR & Chemical Space Analysis
        ↓
Virtual Screening
        ↓
Protein Preparation
        ↓
Molecular Docking
        ↓
ADMET/Toxicity Filtering
        ↓
Lead Prioritization
```

---

# Repository Structure

```text
insilico_drug-discovery/
│
├── data/
│   ├── raw/
│   ├── curated/
│
├── notebooks/
│
├── scripts/
│
├── docking/
│
├── admet/
│
├── results/
│
├── docs/
│   └── images/
│
├── README.md
└── requirements.txt
```

---

# Step 1 — Bioactivity Data Retrieval & Curation

## Data Source

Bioactivity data was retrieved from the **ChEMBL database** for human EGFR.

### Activity Types Used
- IC50
- Ki

### Initial Dataset Size
- 25,604 bioactivity records

---

## Data Curation Process

The dataset was curated using **Python**, **Pandas**, and **RDKit**.

### Curation Steps

- Removed duplicate compounds
- Removed invalid/missing structures
- Removed salts
- Standardized SMILES
- Retained exact activity relations only
- Converted activity values to pIC50

---

## Final Curated Dataset

| Property | Value |
|---|---|
| Final Compounds | 10,592 |
| Activity Range | 1.26 – 17.30 pIC50 |
| Mean pIC50 | 6.92 |

---

# Step 2 — Molecular Descriptor & Fingerprint Generation

## Molecular Descriptors

A total of **208 physicochemical descriptors** were calculated using RDKit.

### Examples

- Molecular Weight (MW)
- LogP
- TPSA
- H-bond Donors/Acceptors
- Rotatable Bonds
- Ring Counts
 <img width="975" height="321" alt="image" src="https://github.com/user-attachments/assets/79057891-2eb6-418b-83df-2c4782c12010" />

---

## Molecular Fingerprints

Morgan Circular Fingerprints:

- Radius = 2
- 2048 bits

These features were used for:

- QSAR modeling
- Similarity analysis
- Chemical space visualization

---

# Step 3 — QSAR Modeling & Validation

## Models Developed

### 1. Multiple Linear Regression (MLR)
Baseline interpretable model.

### 2. Random Forest Regressor (RF)
Non-linear ensemble learning model.

### 3. XGBoost Regressor
Boosting-based machine learning model.

---

## Data Splitting Strategy

A **scaffold split** was used to ensure structural separation between training and test compounds.

---

## Evaluation Metrics

Models were evaluated using:

- R²
- RMSE
- MAE

---

# QSAR Visualizations

## Predicted vs Observed Plot

<img width="975" height="583" alt="image" src="https://github.com/user-attachments/assets/19776851-4e27-49e4-b2e9-a2d08293c990" />


---

## Residual Distribution Plot

<img width="713" height="841" alt="image" src="https://github.com/user-attachments/assets/3b0f0be5-c571-43d4-b8c1-8965eb79cbd3" />


---

## Key Findings

- Non-linear models outperformed linear regression
- XGBoost demonstrated superior predictive performance
- Residuals were centered around zero, indicating low systematic bias
- Predictions were most reliable in the mid-activity range

---

# Step 4 — SAR & Chemical Space Analysis

## Activity Classification

Compounds were grouped into:

- High Activity (pIC50 > 7)
- Medium Activity (5–7)
- Low Activity (< 5)

Approximately 50% of compounds belonged to the high-activity class.

---

## Scaffold Analysis

Murcko scaffold analysis identified the **quinazoline scaffold** as the dominant active core.

This validates the dataset because quinazoline is also present in:

- Erlotinib
- Gefitinib
<img width="975" height="363" alt="image" src="https://github.com/user-attachments/assets/4f3a7f3a-8005-4a7f-861d-06b8337b4762" />

---

## Substructure SAR

### Positive Activity Contributor
- Anilinoquinazoline group

### Negative Activity Contributor
- Sulfonamide-containing compounds

---

## Chemical Space Visualization

### PCA Projection

PCA based on physicochemical descriptors revealed clustering of highly active compounds.

<img width="975" height="388" alt="image" src="https://github.com/user-attachments/assets/2005ae04-09be-4395-997d-a3ac86a87aa5" />

---

### t-SNE Projection

t-SNE based on Morgan fingerprints showed clear structural clustering.

<img width="975" height="388" alt="image" src="https://github.com/user-attachments/assets/d0b4adb7-6e41-47ff-b9e4-be2ddcdb1840" />

---

# Step 5 — Protein Structure Preparation

## Protein Structure

PDB ID: **1IVO**

### Why 1IVO?

- High-resolution crystal structure
- Co-crystallized with Erlotinib
- Clearly defined ATP-binding pocket

---

## Receptor Preparation

Performed using AutoDockTools:

- Removed water molecules
- Added polar hydrogens
- Assigned Gasteiger charges
- Saved receptor in PDBQT format

---

## Key Binding Site Residues

| Residue | Role |
|---|---|
| Met793 | Hinge hydrogen bonding |
| Lys745 | Catalytic lysine |
| Thr790 | Gatekeeper residue |
| Cys797 | Covalent interaction site |
| Asp855 | DFG catalytic motif |

---

# Step 6 — Virtual Screening

## Screening Library Construction

A focused screening library was generated from:

- Curated EGFR-active compounds
- Structurally related analogs
- Similarity-based compound selection

---

## Screening Strategy

Compounds were ranked using:

- QSAR-predicted potency
- Structural similarity
- Drug-likeness properties

---

## Candidate Selection

The top-ranked compounds were selected for docking based on:

- Predicted activity
- Structural diversity
- Physicochemical suitability

Only **10 compounds** were selected for downstream docking analysis in accordance with project constraints.

---

# Step 7 — Molecular Docking & Binding Analysis

## Docking Software

- AutoDock Vina

---

## Docking Configuration

### Grid Center

```text
x = 2.98
y = 52.73
z = -31.25
```

### Grid Size

```text
22 × 22 × 22 Å
```

---

## Docking Workflow

For each ligand:

1. Ligands converted to PDBQT
2. Rotatable bonds assigned
3. Docking performed inside EGFR ATP-binding pocket
4. Best binding pose selected

---

## Interaction Analysis

Important interactions included:

- Hydrogen bonding
- Hydrophobic interactions
- π–π stacking
- Hinge region interactions

Key residues involved:

- Met793
- Lys745
- Thr790
- Cys797

---

## Docking Visualization

![Docking Pose](docs/images/docking_pose.png)

---

# Step 8 — ADMET & Toxicity Filtering

## Tool Used

- pkCSM

---

## Evaluated Properties

### Absorption
- Intestinal absorption
- Caco-2 permeability
- P-glycoprotein substrate status

### Distribution
- VDss
- BBB permeability

### Metabolism
- CYP450 inhibition profile

### Toxicity
- AMES mutagenicity
- hERG inhibition
- Hepatotoxicity

---

## Lipinski Rule of Five

Compounds were filtered using:

- MW ≤ 500
- LogP ≤ 5
- HBD ≤ 5
- HBA ≤ 10

Compounds with ≥2 violations were removed.

---

## ADMET Outcome

| Result | Value |
|---|---|
| Compounds Tested | 10 |
| Passed ADMET | 9 |
| Eliminated | 1 |

TAK-285 was eliminated due to:

- High molecular weight
- Excessive lipophilicity

---

# ADMET Visualization

## ADMET Heatmap
<img width="975" height="477" alt="image" src="https://github.com/user-attachments/assets/cedb7e72-bdf5-436f-a007-85492862b9d1" />


---

# Step 9 — Multi-Parameter Lead Prioritization
<img width="975" height="588" alt="image" src="https://github.com/user-attachments/assets/f5cd8a3b-b75a-47dc-90aa-805f6bc4f15b" />

## Composite Lead Score

<img width="975" height="481" alt="image" src="https://github.com/user-attachments/assets/dd82f405-7dd5-4e6e-9be0-b498af0fd122" />


```text
Lead Score =
0.40 × Docking
+ 0.20 × Absorption
+ 0.20 × MTD
+ 0.10 × LOAEL
+ 0.10 × CYP Safety
```

---

# Final Lead Compounds

| Rank | Compound | Lead Score | Status |
|---|---|---|---|
| 1 | Osimertinib | 0.950 | LEAD 1 |
| 2 | Sapitinib | 0.470 | LEAD 2 |

---

# Lead 1 — Osimertinib

## Why Selected?

- Strongest docking score
- Highest intestinal absorption
- Excellent tolerability profile
- FDA-approved EGFR inhibitor
- Strong binding to EGFR ATP-binding pocket

---

# Lead 2 — Sapitinib

## Why Selected?

- Strong pharmacokinetic profile
- Minimal CYP inhibition
- Structurally diverse scaffold
- Good oral absorption

---

# Major Results

- Complete in silico drug discovery workflow successfully implemented
- XGBoost showed strongest QSAR performance
- Quinazoline scaffold identified as dominant EGFR pharmacophore
- Docking validated strong EGFR interactions
- ADMET filtering removed unsuitable compounds early
- Osimertinib identified as top-ranked lead

---

# Reproducibility

Clone repository:

```bash
git clone https://github.com/Hadia-Shafiq0900/insilico_drug-discovery.git
cd insilico_drug-discovery
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Run notebooks sequentially from:

```text
notebooks/
```

---

# Tools & Databases

## Databases

- ChEMBL
- Protein Data Bank (PDB)
- pkCSM

## Libraries & Software

- Python
- RDKit
- Pandas
- Scikit-learn
- XGBoost
- AutoDock Vina
- AutoDockTools
- Matplotlib

---

# Limitations

- Docking performed using rigid receptor approximation
- ADMET predictions are computational estimates only
- Experimental validation was not performed
- Molecular dynamics simulations were not included

---

# Conclusion

This project demonstrates a complete computational drug discovery pipeline targeting EGFR in NSCLC.

By integrating:

- QSAR modeling
- Virtual screening
- Molecular docking
- ADMET filtering
- Lead prioritization

we successfully identified:

- **Osimertinib** as Lead 1
- **Sapitinib** as Lead 2

The recovery of Osimertinib — a clinically approved EGFR inhibitor — validates the robustness of the computational workflow and demonstrates the effectiveness of integrated in silico drug discovery approaches.

---

# Authors

- Hadia Shafiq
- diya zeejah
- muhammad rizwan
- dua qaiser
---

# References

1. Trott & Olson (2010) — AutoDock Vina  
2. Pires et al. (2015) — pkCSM  
3. RDKit Documentation  
4. ChEMBL Database  
5. Lipinski et al. (2001)
