# Evolutionary Genomics & Multi-Omics Portfolio

![Genomics](https://img.shields.io/badge/Focus-Computational_Genomics_%26_Bioinformatics-7B1FA2) ![Language](https://img.shields.io/badge/Languages-Python_%7C_R-FF9800) ![Institution](https://img.shields.io/badge/Institution-Hebrew_University_of_Jerusalem-00695C)

*Shalev Yaacov — M.Sc. Researcher, Tabach Lab, Hebrew University of Jerusalem.
Computational pipeline development for novel gene discovery in Inherited Retinal Diseases,
integrating evolutionary profiling, phenotypic clustering, and multi-omics data.*

---

## Overview

This repository is a curated public sample of the computational methods and
tools I develop as part of an ongoing M.Sc. thesis at the Hebrew University
of Jerusalem, aimed at identifying novel disease-causing genes in
Inherited Retinal Diseases (IRD) that current diagnostic panels miss.

#### **Research Context**

The research partitions 450+ IRD-associated genes into co-evolved functional
modules using Normalized Phylogenetic Profiling (NPP) across ~2,000 genomes
and HPO-based phenotypic clustering, builds a bidirectional phenotype-to-gene
inference engine for clinical prioritization, and derives a genome-scale
candidate ranking model validated against 4,254+ real-world cases in
collaboration with Hadassah Medical Center.
The approach deliberately combines phenotypic, evolutionary, and functional signals —
rather than relying on any single one alone — into a unified discovery pipeline.
Its findings remain unpublished pending the methodology manuscript in preparation.

What is here is a structured window into the methods: five focused
demonstrations and tools drawn from the same framework, presented roughly in
the order the corresponding stages are used, each running on fully synthetic
data at a fraction of the real scale. They are not the pipeline, and not its
results — they are illustrations of the reasoning and implementation behind it.
A reader who works through them will understand the approach; a reader looking
for the findings should wait for the manuscript.

---

## Visual Portfolio

*For an interactive walkthrough of the research — click to launch:*

> [![Visual Portfolio](https://img.shields.io/badge/Visual_Portfolio-Launch_Interactive_Site_%E2%86%92-E91E63?style=for-the-badge)](https://shalev-compbio.github.io/Shalev-Evolutionary-Genomics-Portfolio/Visual_Portfolio/)
>
> *Narrative walkthrough of the research — NPP methodology, project case studies, and contact.*

---

## Repository Structure

```
portfolio/
├── README.md
├── Visual_Portfolio/
│   └── index.html
└── projects/
    ├── IRD_Phenotype_Clustering/
    │   ├── README.md
    │   ├── RATIONALE.md
    │   └── scripts/ird_phenotype_clustering_demo.ipynb
    ├── Cilia_Module_Validation/
    │   ├── README.md
    │   └── scripts/cilia_clusters_demo.ipynb
    ├── LPP_NPP_Heatmap_Visualization/
    │   ├── README.md
    │   ├── scripts/gene_list_to_lpp_heatmap.R
    │   ├── scripts/gene_list_to_npp_heatmap.R
    │   └── scripts/lpp_multi_cluster_heatmap_with_inclusion.R
    ├── LBS_Consensus_Profiling/
    │   ├── README.md
    │   └── notebook/consensus_profile_demo.ipynb
    └── IRD_HPO_Anatomogram/
        ├── README.md
        └── scripts/hpo_organ_mapping_demo.ipynb
```

Each project contains a README with scientific rationale, workflow
description, and usage instructions. `IRD_Phenotype_Clustering` additionally
includes a dedicated `RATIONALE.md` with an extended discussion of method
design decisions.

---

## Projects

**The five projects fall into three categories reflecting their role in the full pipeline.**

---

### 〉 Pipeline Stages
*Core methodological steps — each demonstrated on synthetic data at reduced scale*

#### [IRD_Phenotype_Clustering](./projects/IRD_Phenotype_Clustering)
*HPO-based semantic similarity clustering of IRD genes into functional disease modules*

Computes pairwise semantic similarity across IRD gene HPO annotations using
Lin similarity with Best-Match Average, constructs a gene–gene similarity graph,
and applies Leiden community detection to produce phenotype-driven disease modules.
Includes IC-based term filtering, perturbation stability scoring, and Fisher's
exact module characterization.

#### [LBS_Consensus_Profiling](./projects/LBS_Consensus_Profiling)
*Consensus-based evolutionary barcode detection for gene module signatures*

Explores a complementary evolutionary approach: identifying a Local Barcode Segment
— a species window where a gene group displays a distinctive, concentrated
conservation pattern — and using consensus profiles built from that window as query
signatures for genome-wide candidate retrieval. Compares five aggregation strategies
with coherence validation and a noise-based negative control.

---

### 〉 Validation Case Study
*Module structure tested against a well-characterized functional gene class*

#### [Cilia_Module_Validation](./projects/Cilia_Module_Validation)
*Cross-validation of module structure against curated ciliopathy gene evidence*

Applies the clustering and annotation logic to ciliopathy genes — a well-characterized
functional class with curated external evidence (CiliaCarta, literature). Serves as an
interpretability check: module structure should recover known biology before it is
trusted to reveal unknown biology.

---

### 〉 Shared Tools
*Visualization and translation layers used across multiple pipeline stages*

#### [LPP_NPP_Heatmap_Visualization](./projects/LPP_NPP_Heatmap_Visualization)
*Publication-ready LPP and NPP phylogenetic profile heatmaps via ComplexHeatmap*

R scripts for generating species-aligned, clade-annotated heatmaps from Local and
Normalized Phylogenetic Profile matrices. Supports single gene lists, multi-cluster
layouts with Inclusion Criterion annotation, and both LPP (presence/absence, 0–1)
and NPP (z-score, diverging scale) profile types. Output figures were used directly
in manuscript preparation.

#### [IRD_HPO_Anatomogram](./projects/IRD_HPO_Anatomogram)
*Translating gene module phenotype signatures into anatomical body maps*

Maps HPO phenotype annotations from gene modules to organ systems and renders
color-coded anatomograms for clinical and non-computational audiences. The
non-trivial step — traversing the HPO DAG from fine-grained terms upward to
anatomical categories — is what makes the output meaningful rather than arbitrary.

---

## Tech Stack

- **Python**: Pandas, NumPy, SciPy, scikit-learn, NetworkX, pronto
- **R**: ComplexHeatmap, circlize, ggplot2, gganatogram
- **Methods**: semantic similarity, graph-based clustering, phylogenetic profiling,
  Naive Bayes probabilistic scoring, multi-omics integration

---

## Contact & Affiliation

- **Lab**: [Prof. Yuval Tabach Lab](https://tabach-lab.com/), Faculty of Medicine
- **Institution**: Hebrew University of Jerusalem
- **Role**: M.Sc. Candidate in Genomics & Bioinformatics

---

> All datasets in this repository are fully synthetic.
> No real patient data, unpublished genomic results, or proprietary laboratory resources are included.
> The actual pipeline, its scale, and its findings remain confidential pending publication.

---

**This repository evolves alongside my thesis research and the methodology manuscript currently in preparation, and will continue to expand with new analyses and demonstrations.**

---
