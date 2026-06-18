# IRD Phenotype Clustering

![Language](https://img.shields.io/badge/Language-Python-FFD43B) ![Focus](https://img.shields.io/badge/Focus-Computational_Biology_%26_Network_Analysis-7B1FA2) ![Status](https://img.shields.io/badge/Status-Demo_Available-00695C)

*Shalev Yaacov - HPO-based phenotypic similarity clustering of IRD genes using graph community detection.*

---

## Overview

Inherited Retinal Diseases (IRDs) involve hundreds of genes with substantial phenotypic heterogeneity. While these genes vary genetically, they often converge into shared phenotypic modules, reflecting common biological mechanisms - for example ciliary dysfunction or phototransduction defects.

The Human Phenotype Ontology (HPO) enables standardized representation of clinical phenotypes and computation of semantic similarity between genes. This pipeline transforms a binary gene-HPO annotation matrix into phenotype-informed functional modules using IC-based phenotype filtering, Lin's semantic similarity with Best Match Average (BMA) aggregation, graph construction with systematic parameter sweep, Leiden community detection, perturbation-based stability scoring, and Fisher's exact enrichment testing.

This public repository provides a self-contained synthetic demonstration of the complete workflow. The framework forms the phenotypic layer of a broader multi-omics module integration platform - phenotype, evolutionary co-profiling, expression, and interaction networks combined.

---

## Rationale

This pipeline treats phenotypic similarity as an independent line of evidence for IRD gene discovery, not a secondary annotation layer - genes behind the same disease mechanism often converge phenotypically even when they share no obvious genetic or evolutionary relationship. Every design choice below (IC filtering, graph-based community detection, stability scoring, enrichment testing) follows from making that signal usable and trustworthy on its own, before it is combined with the pipeline's other independent signals.

**[Read the full scientific rationale →](./RATIONALE.md)**

---

## Workflow

1. **Load input** - binary gene x HPO annotation matrix.
2. **IC-based filtering** - depth, information content, gene-frequency, and coverage thresholds remove uninformative or near-universal terms.
3. **Term and gene similarity** - Lin's semantic similarity at the term level, aggregated to gene-gene similarity via Best Match Average (BMA).
4. **Graph construction** - kNN and similarity-threshold graph variants, built across a parameter sweep.
5. **Community detection** - Leiden algorithm; best graph selected by modularity combined with cohesion/separation.
6. **Stability analysis** - perturbation-based re-clustering; genes classified as core, peripheral, or unstable.
7. **Module QC** - flag modules that are too small or too large to be biologically interpretable.
8. **Phenotypic signature testing** - Fisher's exact test per module, with Benjamini-Hochberg FDR correction across terms.
9. **Summary** - consolidated table across all modules.

---

## Files

```
IRD_Phenotype_Clustering/
├── README.md
├── RATIONALE.md
├── scripts/
│   └── ird_phenotype_clustering_demo.ipynb   Full 9-section demo notebook
├── data/
│   └── synthetic_gene_hpo_matrix.csv         Synthetic 40-gene x 60-HPO binary input
└── outputs/
    └── demo_figures/
        ├── gene_similarity_matrix_demo.csv    Gene x gene BMA similarity matrix
        ├── module_QC_assignment_demo.csv      Module sizes and QC status
        ├── similarity_distribution.png        BMA gene-gene similarity histogram
        ├── gene_stability_heatmap.png         Co-clustering stability matrix (40x40)
        ├── module_size_distribution.png       Module size bar chart with QC thresholds
        └── module_hpo_heatmap.png             Module x top HPO terms (-log10 q-value)
```

---

## Input & Output

**Input:** `data/synthetic_gene_hpo_matrix.csv` - a binary matrix (rows = genes, columns = HPO term IDs, values = 0/1). 40 fabricated genes (`GENE_A01...GENE_D10`) by 60 fabricated HPO term IDs (`HP:SYNTH_001...HP:SYNTH_060`), generated with a fixed random seed and a block co-annotation structure to simulate biological modules.

**Outputs**, all saved to `outputs/demo_figures/` on notebook execution:

- **`gene_similarity_matrix_demo.csv`** - the full 40x40 gene-gene BMA similarity matrix.
- **`module_QC_assignment_demo.csv`** - module ID, size, and QC status per community.
- **`similarity_distribution.png`** - histogram of pairwise BMA similarity scores.
- **`gene_stability_heatmap.png`** - co-clustering frequency heatmap, with module boundaries marked.
- **`module_size_distribution.png`** - module size bar chart with small/large QC thresholds.
- **`module_hpo_heatmap.png`** - module x HPO terms heatmap of FDR-significant signatures.

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

- **IC-based filtering** - retains specific HPO terms above an information-content and depth floor, and removes terms exceeding a gene-frequency ceiling or below a minimum gene-support count. Demo values are illustrative; the real pipeline uses calibrated thresholds not disclosed here.
- **Lin's similarity** - semantic term-term similarity via the most informative common ancestor (the real pipeline uses `pyhpo`). The demo uses Jaccard co-annotation similarity as a self-contained proxy.
- **Best Match Average (BMA)** - bidirectional best-match aggregation from term-level to gene-level pairwise scores.
- **kNN / threshold graphs** - a systematic topology sweep over illustrative k and percentile values, exploring the community-structure space.
- **Leiden algorithm** - resolution-parameterized community detection, run at an illustrative demo resolution.
- **Modularity x cohesion/separation** - the joint criterion used for graph selection across the parameter sweep.
- **Perturbation stability** - Gaussian noise re-clustering over multiple iterations, scoring assignment confidence; core / peripheral / unstable thresholds are demo values.
- **Fisher's exact test with BH FDR** - one-sided HPO term enrichment per module at alpha = 0.05 (standard convention), with a module-frequency pre-filter applied before testing.

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

> **Note:** The real research pipeline additionally requires `pyhpo` (>= 4.0.0) and HPO ontology files (`hp.obo`, `phenotype.hpoa`) for IC computation via Lin's similarity. The demo replaces this step with synthetic IC values and Jaccard-based term similarity to remain fully self-contained.

---

## Data & Privacy Disclaimer

> All gene names, HPO term identifiers, and values in this project are entirely synthetic and fabricated. No real patient, clinical, or unpublished research data are included. Synthetic data are generated with a fixed random seed to demonstrate pipeline functionality while ensuring full confidentiality. All numeric thresholds in the demo are illustrative values chosen for this synthetic dataset and do not reflect the calibrated parameters of the real, unpublished pipeline. Real datasets - full gene-HPO annotation matrices, IC values, and clustering results - remain confidential.

---

*Part of the Evolutionary Genomics & Multi-Omics Portfolio*
