# **IRD HPO Anatomical Phenotype Visualization (Synthetic Demonstration)**

*Mapping clinical phenotype burden onto the human body — a pipeline for communicating data-rich bioinformatics findings to clinical and non-computational audiences.*

![Language](https://img.shields.io/badge/Language-Python%20%7C%20R-blue)
![Focus](https://img.shields.io/badge/Focus-HPO_Organ_Mapping-7B1FA2)
![Status](https://img.shields.io/badge/Status-Synthetic_Demo-00695C)

> **Note:** All gene names, module assignments, and cluster results in this repository are **synthetic and fabricated**.
> No real patient or research data is included.

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
│   ├── HPO_Organ_Label_Mapping.csv        ← gganatogram organ keys → human-readable labels
│   └── gganatogram_organs_input_DEMO.csv  ← Final organ-% table (output of demo notebook)
│
├── scripts/
│   ├── hpo_organ_mapping_demo.ipynb      ← Demo: HPO term → organ mapping + % aggregation
│   └── gganatomogram_Plot.Rmd            ← Render anatomogram figure from the output CSV
│
└── outputs/
    └── demo/
        └── module_1_cluster1_demo.png    ← Example output figure (synthetic data)
```

---

## ⚙️ Pipeline — Two Demo Steps

### Step 1 — HPO Mapping & Organ Percentage Aggregation (`hpo_organ_mapping_demo.ipynb`)

**Inputs:**
- `data/gene_hpo_module_DEMO.csv` — synthetic gene–HPO–module annotation table
- `data/HPO_Organ_Label_Mapping.csv` — organ key → label reference

**What it does:**
- Applies a keyword-based mapping from HPO term names to anatomical organ keys.
  This is a simplified illustrative approach for the demo; the real pipeline uses
  full HPO ontology traversal via `pronto` (BFS over the `is_a` DAG, no internet required,
  but needs the `hp.obo` file).
- For each module, computes the percentage of genes with at least one phenotype
  mapped to each organ: `value = (genes with organ phenotype / module size) × 100`
- Outputs: `data/gganatogram_organs_input_DEMO.csv` with columns `module_id, organ, value`

**Key simplification (demo vs. real):**

| Demo notebook | Real pipeline |
|---------------|---------------|
| Keyword substring matching on HPO term names | BFS traversal from each HPO term up the `is_a` DAG to the nearest organ target ancestor |
| No external files needed beyond `pandas` | Requires `hp.obo` (~10 MB) and `pronto` |
| Covers terms whose names contain explicit anatomical keywords | Covers all HPO terms regardless of name content |

---

### Step 2 — Anatomogram Visualization (`gganatomogram_Plot.Rmd`)

**Input:** `data/gganatogram_organs_input_DEMO.csv`

**What it does:**
- Joins module organ-% values onto the full human male anatomy background
- Renders a color-mapped figure: grey (0 %) → yellow → orange → dark red (100 %)
- Adds organ labels for the top-K organs by phenotype burden
- Exports PDF (vector), PNG (preview), and TIFF (print-quality)

---

## 📊 Demo Input & Output

The synthetic demo input (`gene_hpo_module_DEMO.csv`) contains 3 fabricated gene clusters:

| Module | Biological theme (synthetic) | Dominant organs |
|---|---|---|
| 1 | Neurological / retinal | eye, retina, brain |
| 2 | Metabolic / visceral | kidney, liver, blood |
| 3 | Cardiac / muscular | heart, skeletal muscle |

**Example output — Module 1 (synthetic):**

![Demo Anatomogram](outputs/demo/module_1_cluster1_demo.png)

*Color intensity = % of genes in the cluster with phenotypes mapped to each organ (synthetic data).*

---

## 🖥️ How to Run

### Requirements

```bash
# Python (for the demo notebook)
pip install pandas jupyter

# R (for the anatomogram plot)
Rscript -e 'install.packages(c("ggplot2","dplyr","readr","scales","ggrepel"))'
# gganatogram must be installed from GitHub:
Rscript -e 'if (!requireNamespace("devtools")) install.packages("devtools"); devtools::install_github("jespermaag/gganatogram")'
```

> **Note:** The demo notebook requires only `pandas` and runs without any ontology file.
> The real pipeline additionally requires `pronto` and the `hp.obo` HPO ontology file
> (available from [https://hpo.jax.org/data/ontology](https://hpo.jax.org/data/ontology));
> no download is needed for the demo.

### Run the demo

```bash
# Step 1 — Run the Python notebook (generates gganatogram_organs_input_DEMO.csv)
jupyter notebook scripts/hpo_organ_mapping_demo.ipynb
```

Then in RStudio, open `scripts/gganatomogram_Plot.Rmd`, set `MODULE_ID` to 1, 2, or 3, and knit.

---

## 🧠 Key Methodological Insights

- **HPO hierarchy traversal** is essential in the full pipeline: fine-grained terms (e.g., *Retinal dystrophy*) must be propagated to organ-level categories (e.g., *eye*) for meaningful aggregation. The demo approximates this with keyword matching.
- The **% of genes** metric is intentionally simple — designed for clinical readability, not statistical testing.
- The pipeline is **module-agnostic**: it works with any gene clustering output that provides gene → HPO → module assignments.
- This visualization serves as a **clinical communication layer** on top of computational gene clustering results.

---

## ⚠️ Data & Privacy Disclaimer

> All gene names, entrez IDs, module assignments, and cluster results in this repository are **entirely synthetic and fabricated**.
> No real patient, clinical, or unpublished research data are included.

---

*This project is part of the Evolutionary Genomics & Multi-Omics Portfolio.*
