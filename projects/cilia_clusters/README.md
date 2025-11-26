# Ciliary Cluster Computational Pipeline

![Language](https://img.shields.io/badge/Language-Python-FFD43B)
![Focus](https://img.shields.io/badge/Focus-Genomics_%26_Pipeline_Dev-7B1FA2)
![Status](https://img.shields.io/badge/Status-Demo_Available-00695C)

### 🧬 Validating Evolutionary Signals via Multi-Omics Integration

**Shalev Yaacov** | M.Sc. Researcher @ Hebrew University (Tabach Lab)  
*A complete computational framework for transforming raw phylogenetic profiles into validated, functionally enriched, and publication-ready insights.*

---

## 📌 Overview

**Purpose:** This project illustrates a complete computational framework designed to address a common challenge in genomics: transforming large-scale phylogenetic profiles, raw NPP matrices, and preliminary cluster assignments into a validated, functionally enriched, and publication-ready analysis.

**The Challenge:** In computational biology, moving from raw, high-dimensional NPP data and heterogeneous gene clusters into actionable insight is rarely linear. The process is often noisy, iterative, and poorly documented. The challenge is to construct a workflow that remains scientifically rigorous while also being transparent, modular, systematic, and fully reproducible.

> **Our Solution:**  
> This repository documents the full step-by-step framework applied in this case study. The scripts are not standalone utilities; they function as modular components of a structured multi-step pipeline that ingests both precomputed ciliary clusters and raw NPP data, systematically filters and annotates each cluster, quantifies evolutionary and functional coherence, integrates multi-omic evidence, and culminates in a distilled set of high-confidence genes. The workflow expands known ciliary modules and identifies new candidates supported by coherent evolutionary, functional, and phenotypic signals.

---

## 🔬 Scientific Rationale

The ciliary system is highly modular. While initial phylogenetic profiling identified major ciliary modules (like IFT and BBS), this pipeline was built to systematically expand and quantify this modularity.

The objective was to begin with raw cluster outputs and construct a framework capable of evaluating and refining their biological meaning. We expanded the initial set from **14 to 27 cilia-associated clusters**, each assigned an evidence-based confidence level. Every gene was classified into categories such as *CiliaCarta*, *Literature-supported*, or *Novel Candidate*, enabling a shift from text-based enrichment toward a structured analysis of functional subnetworks.

---

## ⚙️ The Computational Workflow (Chronology)

This project is structured as a chronological pipeline, where each script performs a specific task in the research flow.

### Step 1: Filtering (Data Ingestion)
* **Scientific Goal:** Isolate a high-confidence set of "cilia-related" clusters from a raw dataset of 343 genomic clusters.
* **Script:** `extract_cilia_annotations.py`
* **Function:** Scans annotation columns for specified terms (e.g., "cilia") and exports a clean subset of relevant clusters.

### Step 2: Initial Enrichment (CiliaCarta)
* **Scientific Goal:** Systematically classify every gene in the filtered clusters by cross-referencing them with curated references.
* **Script:** `merge_clusters_genes_with_reference.py`
* **Function:** Explodes gene lists and merges them with external datasets (e.g., CiliaCarta), tagging each gene with a detailed evidence category (e.g., “Gold Standard”, “Literature-supported”, “Novel Candidate”).

### Step 3: Deeper Enrichment (Functional Context)
* **Scientific Goal:** Add deeper biological and disease context, differentiating between functional groups (e.g., motile vs. sensory) and mapping genes to multiple evidence types.
* **Script:** `extract_subsets_by_terms.py`
* **Function:** Flexible search tool for extracting functional subsets based on GO, KEGG, OMIM, VarElect, GeneCards, and user-defined terms.

### Step 4: Quantification & Analysis
* **Scientific Goal:** Quantify the distribution, co-occurrence, and overlap of evidence categories.
* **Scripts:**
    * `cluster_annotation_summary.py`: Generates summary statistics for all categories.
    * `co_occurrence_heatmap_cilia_genes.py`: Builds category-by-category co-occurrence matrices.
    * `group_overlap_analysis.py`: Computes gene-set overlap (counts/Jaccard) between evidence categories.

### Step 5: Validation & Benchmarking
* **Scientific Goal:** Validate ~100 novel candidate genes against external evidence and published studies.
* **Analysis:** Systematic comparison with Dobbelaere et al. (2023).
* **Key Finding:** The pipeline identified **166 unique genes** missed by the comparative study. Our “Novel Candidates” aligned conceptually with their “Unknown [U]” category, supporting the predictive strength of our framework.

### Step 6: Visualization
* **Scientific Goal:** Generate publication-ready figures summarizing evidence composition and evolutionary structure.
* **Scripts:**
    * `cluster_evidence_viz.py`: Produces stacked barplots of evidence composition.
      ![Distribution of gene evidence categories](results/gene_category_distribution_per_cluster.png)
    * `lpp_multi_cluster_heatmap_with_inclusion.R`: Generates annotated LPP heatmaps stratified by evidence type.
      ![LPP Heatmap stratified by evidence](results/Clusters_Dominated_by_Known_Genes.png)

---

## 🛠️ Scripts in this Project

All Python scripts are robust CLI tools featuring `argparse`, `logging`, and `pathlib` for reproducibility.

| Script | Purpose (One-Liner) |
| :--- | :--- |
| `extract_cilia_annotations.py` | **(Step 1)** Filters a master table for rows matching specific terms. |
| `merge_clusters_genes_with_reference.py` | **(Step 2)** Merges exploded gene lists with external references. |
| `extract_subsets_by_terms.py` | **(Step 3)** Extracts user-defined functional subgroups. |
| `cluster_annotation_summary.py` | **(Step 4)** Generates summary statistics for evidence categories. |
| `co_occurrence_heatmap_cilia_genes.py` | **(Step 4)** Computes category co-occurrence within clusters. |
| `group_overlap_analysis.py` | **(Step 4)** Calculates Jaccard/overlap between evidence groups. |
| `cluster_evidence_viz.py` | **(Step 6)** Creates stacked barplots for evidence composition. |

> *(Note: The R script `lpp_multi_cluster_heatmap_with_inclusion.R` is located in the [`../heatmap_visualization`](../heatmap_visualization) project folder.)*

---

## 💻 How to Run (Demo Example)

All scripts include a `--help` flag for detailed options. Execute scripts in chronological order.

**Example (Step 4 - Summary):**
```bash
python scripts/cluster_annotation_summary.py \
    --input data/demo_merged_data.csv \
    --cluster-col "Cluster_ID" \
    --category-col "Inclusion_criterion" \
    --outdir results/summary \
    --top-n 20
````

---

## ⚠️ Data & Privacy Disclaimer

> **All data provided in this project are synthetic and for demonstration purposes only.**

Real datasets (full NPP matrices, CiliaCarta, patient data) remain confidential. Demo files are randomly generated and intended solely to demonstrate the functionality of the pipeline.

---

*This project is part of the Evolutionary Genomics & Multi-Omics Portfolio.*
