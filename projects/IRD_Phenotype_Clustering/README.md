# IRD Phenotype Clustering

![Language](https://img.shields.io/badge/Language-Python-FFD43B)
![Focus](https://img.shields.io/badge/Focus-Computational_Biology_%26_Network_Analysis-7B1FA2)
![Status](https://img.shields.io/badge/Status-Demo_Available-00695C)

*Shalev Yaacov — HPO-based phenotypic similarity clustering of IRD genes using graph community detection.*

---

## Overview

Inherited Retinal Diseases (IRDs) involve hundreds of genes with substantial phenotypic heterogeneity.
While these genes vary genetically, they often converge into **shared phenotypic modules**, reflecting
common biological mechanisms (e.g., ciliary dysfunction, phototransduction defects).

The Human Phenotype Ontology (HPO) enables standardised representation of clinical phenotypes and
computing semantic similarity between genes. This pipeline transforms a binary gene–HPO annotation
matrix into **phenotype-informed functional modules** using:

- IC-based phenotype filtering (specificity, depth, gene frequency)
- Lin's semantic similarity + Best Match Average (BMA) gene similarity
- kNN and threshold graph construction with systematic parameter sweep
- Leiden community detection with modularity × cohesion/separation graph selection
- Perturbation-based stability scoring and core/peripheral gene classification
- Fisher's exact test enrichment with Benjamini-Hochberg FDR for module phenotypic signatures

This public repository provides a **self-contained synthetic demonstration** of the complete workflow.
The framework forms the phenotypic layer of a broader multi-omics module integration platform (phenotype
+ evolutionary co-profiling + expression + interaction networks).

---

## 🔬 Scientific Background

Semantic similarity provides a complementary lens to evolutionary co-profiling (NPP/LPP), capturing
phenotypic convergence across IRD genes. IC-based filtering is essential for removing overly general
HPO terms that inflate similarity scores and obscure true modules. Graph-based community detection
(Leiden) is well-suited for complex similarity landscapes, and module stability under perturbation
distinguishes robust biological signals from noise-driven assignments.

---

## Workflow

| Step | Section | Description |
|------|---------|-------------|
| 1 | § 1 | Load binary gene × HPO annotation matrix |
| 2 | § 2 | IC-based phenotype filtering (depth, IC, frequency, coverage thresholds) |
| 3 | § 3 | Term–term similarity (Lin's / Jaccard proxy) → BMA gene–gene similarity |
| 4 | § 4 | Build kNN and threshold graph variants |
| 5 | § 5 | Leiden community detection; select best graph by modularity × cohesion/separation |
| 6 | § 6 | Perturbation stability analysis; classify genes as core / peripheral / unstable |
| 7 | § 7 | Module QC — flag small and large modules |
| 8 | § 8 | Phenotypic signatures: Fisher's exact test + Benjamini-Hochberg FDR |
| 9 | § 9 | Summary table across all modules |

---

## Files

```
IRD_Phenotype_Clustering/
├── README.md
├── scripts/
│   └── ird_phenotype_clustering_demo.ipynb   Full 9-section demo notebook
├── data/
│   └── synthetic_gene_hpo_matrix.csv         Synthetic 40-gene × 60-HPO binary input
└── outputs/
    └── demo_figures/
        ├── gene_similarity_matrix_demo.csv    Gene × gene BMA similarity matrix
        ├── module_QC_assignment_demo.csv      Module sizes and QC status
        ├── similarity_distribution.png        BMA gene–gene similarity histogram
        ├── gene_stability_heatmap.png         Co-clustering stability matrix (40×40)
        ├── module_size_distribution.png       Module size bar chart with QC thresholds
        └── module_hpo_heatmap.png             Module × top HPO terms (−log₁₀ q-value)
```

---

## Input & Output

**Input:** `data/synthetic_gene_hpo_matrix.csv`
Binary matrix (rows = genes, columns = HPO term IDs, values = 0/1). 40 fabricated genes
(`GENE_A01…GENE_D10`) × 60 fabricated HPO term IDs (`HP:SYNTH_001…HP:SYNTH_060`), generated
with a fixed random seed and a block co-annotation structure to simulate biological modules.

**Outputs** (all saved to `outputs/demo_figures/` on notebook execution):

| File | Contents |
|------|----------|
| `gene_similarity_matrix_demo.csv` | 40×40 gene–gene BMA similarity matrix |
| `module_QC_assignment_demo.csv` | Module ID, size, and QC status per community |
| `similarity_distribution.png` | Histogram of pairwise BMA similarity scores |
| `gene_stability_heatmap.png` | Co-clustering frequency heatmap (module boundaries marked) |
| `module_size_distribution.png` | Module size bar chart with small/large thresholds |
| `module_hpo_heatmap.png` | Module × HPO terms heatmap (FDR-significant signatures) |

---

## Example Usage

```bash
# Open and run interactively
jupyter notebook scripts/ird_phenotype_clustering_demo.ipynb

# Run non-interactively
jupyter nbconvert --to notebook --execute scripts/ird_phenotype_clustering_demo.ipynb \
    --output scripts/ird_phenotype_clustering_demo_executed.ipynb
```

---

## Key Methods

| Method | Purpose |
|--------|---------|
| **IC-based filtering** | Retain specific HPO terms above an IC floor and depth floor; remove terms exceeding a gene-frequency ceiling or below a minimum gene-support count. Demo values are illustrative — the real pipeline uses calibrated thresholds not disclosed here. |
| **Lin's similarity** | Semantic term–term similarity via Most Informative Common Ancestor (real pipeline uses `pyhpo`). Demo uses Jaccard co-annotation similarity as a self-contained proxy. |
| **Best Match Average (BMA)** | Bidirectional best-match aggregation from term-level to gene-level pairwise scores. |
| **kNN / threshold graphs** | Systematic topology sweep (illustrative k and percentile values) to explore community structure space. |
| **Leiden algorithm** | Resolution-parameterised community detection with an illustrative demo resolution value. |
| **Modularity × cohesion/separation** | Joint criterion for graph selection across the parameter sweep. |
| **Perturbation stability** | Gaussian noise re-clustering over multiple iterations to score assignment confidence; core / peripheral / unstable thresholds are demo values. |
| **Fisher's exact test + BH FDR** | One-sided HPO term enrichment per module; α = 0.05 (standard convention); module-frequency pre-filter applied before testing. |

---

## Dependencies

```
numpy
pandas
networkx
python-igraph
leidenalg
scipy
statsmodels
matplotlib
seaborn
```

Install with:
```bash
pip install numpy pandas networkx python-igraph leidenalg scipy statsmodels matplotlib seaborn
```

> **Note:** The real research pipeline additionally requires `pyhpo` (≥ 4.0.0) and the HPO ontology
> files (`hp.obo`, `phenotype.hpoa`) for IC computation via Lin's similarity. The demo replaces this
> step with synthetic IC values and Jaccard-based term similarity to remain fully self-contained.

---

## ⚠️ Data & Privacy Disclaimer

> **All gene names, HPO term identifiers, and values in this project are entirely synthetic and fabricated.**
> No real patient, clinical, or unpublished research data are included. Synthetic data are generated
> with a fixed random seed to demonstrate pipeline functionality while ensuring full confidentiality.
> All numeric thresholds in the demo are illustrative values chosen for this synthetic dataset
> and do not reflect the calibrated parameters of the real unpublished pipeline. Real datasets
> (full gene–HPO annotation matrices, IC values, clustering results) remain confidential.

---

*Part of the Evolutionary Genomics & Multi-Omics Portfolio*

