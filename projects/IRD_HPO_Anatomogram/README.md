# **IRD HPO Anatomical Phenotype Visualization (Synthetic Demonstration)**

*Mapping clinical phenotype burden onto the human body — a pipeline for communicating data-rich bioinformatics findings to clinical and non-computational audiences.*

![Language](https://img.shields.io/badge/Language-Python%20%7C%20R-blue)
![Focus](https://img.shields.io/badge/Focus-HPO_Organ_Mapping-7B1FA2)
![Status](https://img.shields.io/badge/Status-Synthetic_Demo-00695C)

> **Note:** All gene names, module assignments, and cluster results in this repository are **synthetic and fabricated**.
> The HPO ontology file (`hp.obo`) and phenotype-to-organ mappings are derived from the publicly available
> [Human Phenotype Ontology](https://hpo.jax.org). No real patient or research data is included.

---

## 🧬 Overview

This project provides a **phenotype-to-anatomy visualization pipeline** that takes a set of gene clusters, maps their associated HPO phenotype terms to anatomical organ systems, and renders the result as an interpretable **color-coded anatomogram** of the human body.

The core question it answers:

> *"Given a cluster of genes from a bioinformatics analysis — which organs do their clinical phenotypes converge on?"*

This enables **clinicians, genetic counselors, and non-computational collaborators** to assess the biological relevance of computational gene modules in a single figure — without needing to interpret complex similarity matrices or network graphs.

---

## 🔬 Scientific Background

Inherited Retinal Diseases (IRDs) are a group of rare genetic disorders. While visual dysfunction is their hallmark, **IRD-associated genes frequently cause systemic phenotypes** affecting the nervous system, kidneys, heart, and other organs — reflecting their roles in conserved biological pathways (ciliary function, mitochondrial metabolism, etc.).

Computational analyses of IRD genes produce gene clusters based on phenotypic or evolutionary similarity. These clusters are rich in information but difficult to communicate to clinical collaborators.

This pipeline was designed to bridge that gap: it takes any gene cluster output and generates an anatomogram showing the **aggregate phenotype burden** across organ systems — a figure immediately readable by any clinician.

The **Human Phenotype Ontology (HPO)** provides the controlled vocabulary connecting gene annotations to clinical phenotypes, which are then mapped to anatomical systems.

---

## 🗂️ Repository Structure

```
IRD_HPO_Anatomogram/
│
├── data/
│   ├── gene_hpo_module_DEMO.csv          ← Synthetic gene-HPO-module input (DEMO)
│   ├── combined_phenotype_map.csv         ← HPO term → organ mapping (from hp.obo)
│   ├── HPO_Organ_Label_Mapping.csv        ← Human-readable organ labels
│   ├── gganatogram_organs_input_DEMO.csv  ← Final organ-% table (DEMO output of step 2)
│   └── hp.obo                             ← ⚠ Download separately (see below)
│
├── scripts/
│   ├── 01_hpo_structure_builder.ipynb    ← Step 1: Parse HPO ontology → organ map
│   ├── 02_hpo_to_organ_mapping.ipynb     ← Step 2: Gene clusters → organ percentages
│   └── 03_gganatomogram_Plot.Rmd         ← Step 3: Render anatomogram figure
│
└── outputs/
    └── demo/
        └── module_1_cluster1_demo.png    ← Example output figure (synthetic data)
```

---

## ⚙️ Pipeline — Three Steps

### Step 1 — HPO Ontology Parsing (`01_hpo_structure_builder.ipynb`)

**Input:** `hp.obo` (download from [HPO releases](https://hpo.jax.org/data/ontology))

**What it does:**
- Parses the HPO OBO file (19,393 terms, 23,748 is_a edges)
- Builds the HPO term hierarchy (parent-child relationships)
- Maps HPO terms to anatomical organ systems via text-matching and keyword dictionaries
- Outputs: `combined_phenotype_map.csv` — the HPO→organ mapping table

**Key parameters:**
```python
MIN_DEPTH = 2            # Exclude very general top-level terms
EXCLUDE_TERMS = [        # Exclude non-anatomical categories
    "History", "Exposure", "Procedure", ...
]
SYSTEM_KEYWORDS = {      # Keyword dictionaries per body system
    "nervous_system": ["brain", "cerebral", "neuron", ...],
    "circulation":    ["blood", "cardiac", "vascular", ...],
    ...
}
```

---

### Step 2 — Gene → Organ Mapping (`02_hpo_to_organ_mapping.ipynb`)

**Inputs:**
- `gene_hpo_module_DEMO.csv` — gene symbols, HPO term IDs, and module assignments
- `combined_phenotype_map.csv` — from Step 1

**What it does:**
- For each gene cluster (module), collects all associated HPO terms
- Looks up which organ(s) each HPO term maps to
- Computes: **% of genes in the module** with at least one phenotype linked to each organ
- Outputs: `gganatogram_organs_input_*.csv` with columns `module_id, organ, value`

**Key parameter:**
```python
MIN_PERCENT_TO_REPORT_ORGAN = 5.0   # Minimum % threshold for inclusion
```

---

### Step 3 — Anatomogram Visualization (`03_gganatomogram_Plot.Rmd`)

**Input:** `gganatogram_organs_input_DEMO.csv`

**What it does:**
- Joins module organ-% values onto the full human male anatomy background
- Renders a color-mapped figure: grey (0%) → yellow → orange → dark red (100%)
- Adds organ labels for top-K organs by phenotype burden
- Exports PDF (vector), PNG (preview), and TIFF (print-quality)

---

## 📊 Demo Input & Output

The synthetic demo input (`gene_hpo_module_DEMO.csv`) contains 3 fabricated gene clusters:

| Module | Biological theme (synthetic) | Dominant organs |
|---|---|---|
| 1 | Neurological / retinal | brain, nerve, eye |
| 2 | Metabolic / visceral | kidney, liver, blood |
| 3 | Cardiac / muscular | heart, skeletal muscle |

**Example output — Module 1 (synthetic):**

![Demo Anatomogram](outputs/demo/module_1_cluster1_demo.png)

*Color intensity = % of genes in the cluster with phenotypes mapped to each organ (synthetic data).*

---

## 🖥️ How to Run

### Requirements

```bash
# Python (for notebooks)
pip install pandas numpy pathlib

# R (for anatomogram plot)
Rscript -e 'install.packages(c("ggplot2","dplyr","readr","scales","ggrepel"))'
Rscript -e 'devtools::install_github("jespermaag/gganatogram")'
```

### Step 1 — Download hp.obo

```
Download from: https://hpo.jax.org/data/ontology
Save as: data/hp.obo
```

### Step 2 — Run the pipeline

Open notebooks in order in Jupyter:

```bash
jupyter notebook scripts/01_hpo_structure_builder.ipynb
jupyter notebook scripts/02_hpo_to_organ_mapping.ipynb
```

Then in RStudio, open `scripts/03_gganatomogram_Plot.Rmd`, set `MODULE_ID` and knit.

---

## 🧠 Key Methodological Insights

- **HPO hierarchy traversal** is essential: fine-grained terms (e.g., *Retinal dystrophy*) must be propagated to organ-level categories (e.g., *eye*) for meaningful aggregation.
- The **% of genes** metric is intentionally simple — designed for clinical readability, not statistical testing.
- The pipeline is **module-agnostic**: it works with any gene clustering output that provides gene → HPO → module assignments.
- This visualization serves as a **clinical communication layer** on top of computational gene clustering results.

---

## ⚠️ Data & Privacy Disclaimer

> All gene names, entrez IDs, module assignments, and cluster results in this repository are **entirely synthetic and fabricated**.
> No real patient, clinical, or unpublished research data are included.
> The `combined_phenotype_map.csv` is derived solely from the publicly available `hp.obo` ontology file.

---

*This project is part of the Evolutionary Genomics & Multi-Omics Portfolio.*
