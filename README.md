# Evolutionary Genomics & Multi-Omics Portfolio

![Genomics](https://img.shields.io/badge/Focus-Genomics_%26_Bioinformatics-7B1FA2) ![Language](https://img.shields.io/badge/Languages-Python_%7C_R-FF9800) ![Institution](https://img.shields.io/badge/Institution-Hebrew_University_of_Jerusalem-00695C)

### 🧬 Decoding Disease Architecture Through Evolutionary Signals

**Shalev Yaacov** | M.Sc. Researcher @ Hebrew University (Tabach Lab)
*Evolutionary gene clustering, multi-omics integration, and phylogenetic profiling for uncovering IRD & cilia-related disease networks.*

---

## Overview

This portfolio gathers the computational frameworks, pipelines, and visualization tools I develop for studying **Inherited Retinal Diseases (IRD)** and **Ciliopathies**.

My work focuses on addressing **missing heritability** in genetic disorders. By integrating **Normalized Phylogenetic Profiling (NPP)** with transcriptomic, phenotypic, and molecular datasets, I aim to uncover evolutionarily conserved gene modules and new disease candidates.

> **Note:** All datasets in this repository are synthetic, transformed, or strictly demonstrative to ensure confidentiality and compliance with institutional agreements.

---

## Visual Portfolio

For an interactive, narrative walkthrough of this work:

**[Launch the interactive portfolio →](https://shalev-compbio.github.io/Shalev-Evolutionary-Genomics-Portfolio/Visual_Portfolio/)**

It includes a guided explanation of the NPP method, case-study summaries for each project below, and ways to get in touch.

---

## Research Context

This portfolio is a public sample of methods developed for an ongoing M.Sc. thesis project at the Hebrew University of Jerusalem, aimed at identifying novel disease-causing genes in Inherited Retinal Diseases that current diagnostic panels miss. The approach combines phenotypic similarity (HPO), evolutionary co-profiling (NPP), and functional/clinical annotation into a single discovery pipeline, rather than relying on any single signal alone.

The five projects below are small-scale, simplified illustrations of individual methods from that pipeline, built specifically for public demonstration - **not the pipeline itself, and not its results.** They are presented in the order the corresponding stages are actually used, but each runs on synthetic data at a small fraction of the real scale. The actual implementation operates genome-wide, across thousands of species and real patient cohorts; its findings are unpublished pending the methodology manuscript currently in preparation.

---

## Key Projects

### 1. [IRD Phenotype Clustering](./projects/IRD_Phenotype_Clustering)
* **Goal:** Group known IRD genes into phenotype-driven functional modules from HPO annotations alone, as the first layer of the discovery pipeline.
* **Approach:** Information-content-based term filtering, Lin/BMA semantic similarity, kNN and similarity-threshold graph construction, and Leiden community detection, with perturbation-based stability scoring and Fisher's exact enrichment to characterize each resulting module.
* **Demonstrated on:** a synthetic 40-gene × 60-HPO-term matrix (`scripts/ird_phenotype_clustering_demo.ipynb`).

### 2. [Cilia Cluster Analysis](./projects/cilia_clusters)
* **Goal:** A focused case study applying the same module-validation logic to one functional gene class - ciliopathy genes - cross-checked against curated and literature evidence.
* **Includes:** automated annotation and GO-term enrichment, internal-distance and similarity metrics, detection of coherent functional submodules, and visualizations of gene-gene relationships and conserved signatures.

### 3. [High-Dimensional Visualization](./projects/heatmap_visualization)
* **Goal:** The shared visualization layer used across the pipeline to inspect similarity structure at every stage.
* **Tools:** custom Python and R scripts for species-aligned matrices, clustered heatmaps, annotation layers, and comparison plots.

### 4. [Genomic Segmentation Demo](./projects/ird_lbs_barcode_segments)
* **Goal:** Demonstrate the evolutionary co-profiling (NPP) side of the pipeline, run in parallel to the phenotypic layer, using a Local Barcode Segmentation (LBS)-inspired approach on synthetic data.
* **Includes:** synthetic NPP matrices and barcode segments, segmentation and consensus-profile workflows, and signal-vs-noise validation experiments.

### 5. [IRD HPO Anatomogram](./projects/IRD_HPO_Anatomogram)
* **Goal:** Translate module-level phenotype data into anatomical body maps, making the pipeline's output interpretable for clinical collaborators.
* **Approach:** maps HPO phenotype terms to organ systems and renders a color-coded human anatomogram showing which organs each gene module is most associated with.
* **Includes:** HPO ontology parsing and organ-mapping pipeline (Python), per-module organ percentage computation, and gganatogram-based anatomical visualization (R).

---

## Tech Stack & Methodology

* **Languages:** Python (Pandas, Scikit-learn, SciPy), R (ggplot2, pheatmap, ComplexHeatmap), Git/GitHub.
* **Core Methods:**
    * Normalized Phylogenetic Profiling (NPP).
    * Unsupervised graph-based clustering (Leiden community detection).
    * Comparative genomics and multi-omics data integration.
    * Feature engineering and ML-based candidate prioritization.

---

## Contact & Affiliation

* **Lab:** [Prof. Yuval Tabach Lab](https://tabach-lab.com/), Faculty of Medicine.
* **Institution:** Hebrew University of Jerusalem.
* **Role:** M.Sc. Candidate in Genomics & Bioinformatics.
* **Location:** Jerusalem, Israel.

---

*This repository evolves alongside my thesis research and the methodology manuscript currently in preparation, and will continue to expand with new analyses and demonstrations.*
