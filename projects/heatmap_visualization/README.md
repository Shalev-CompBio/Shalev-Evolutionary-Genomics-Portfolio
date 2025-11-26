# Heatmap Generation Scripts (LPP & NPP)

![Language](https://img.shields.io/badge/Language-R-276DC3)
![Focus](https://img.shields.io/badge/Focus-Phylogenetic_Profiling_Visualization-7B1FA2)
![Status](https://img.shields.io/badge/Status-Publication_Ready-00695C)

### 🧬 Visualizing Evolutionary Conservation Across Species

**Shalev Yaacov** | M.Sc. Researcher @ Hebrew University (Tabach Lab)  
*R-based tools for producing publication-ready LPP and NPP phylogenetic heatmaps using ComplexHeatmap.*

---

## 📌 Overview

**Purpose:**  
This module provides a set of R scripts for generating high-quality heatmaps from phylogenetic profiles. Both **LPP** (probabilistic presence/absence) and **NPP** (normalized evolutionary signal) visualizations are supported, enabling rapid exploration of evolutionary conservation and modularity.

**Scientific Context:**  
LPP and NPP heatmaps highlight evolutionary conservation across species in different ways.  
- LPP emphasizes presence/absence transitions and strong evolutionary boundaries.  
- NPP provides a continuous standardized signal, revealing subtle co-evolutionary trends.  

Together, they offer complementary perspectives for studying gene-family evolution and functional modules.

---

## 🔬 Scripts Included

### 1. `gene_list_to_lpp_heatmap.R`
*Generates a standard LPP heatmap for a single gene list.*  
Ideal for rapid exploratory profiling using hierarchical clustering.

### 2. `gene_list_to_npp_heatmap.R`
*Creates a z-scored NPP heatmap with flexible row ordering and color clipping.*  
Useful for detecting fine-grained evolutionary coherence while stabilizing extreme outliers.

### 3. `lpp_multi_cluster_heatmap_with_inclusion.R`
*Produces an advanced multi-cluster LPP heatmap with row gaps, per-cluster clustering, and evidence annotations.*  
Designed for validating cluster structure and contrasting curated evidence categories.

---

## ⚙️ Core Workflow (All Scripts)

Each script:

- Reads user-provided **gene lists**, **cluster annotations**, and **LPP/NPP matrices**.  
- Aligns species columns to a **phylogenetically curated species map** for consistent evolutionary ordering.  
- Determines row order by clustering, input order, or predefined ordering.  
- Renders a high-quality **ComplexHeatmap** figure (PNG or optional PDF).  
- Saves reproducibility files, including sorted matrices, row orders, run parameters, logs, and session info.

---

## 📁 Files & Recommended Structure

```

projects/heatmap_visualization/
├─ scripts/
│  ├─ gene_list_to_lpp_heatmap.R
│  ├─ gene_list_to_npp_heatmap.R
│  └─ lpp_multi_cluster_heatmap_with_inclusion.R
├─ input/
│  ├─ demo_genes.csv
│  ├─ demo_clusters_genes_inclusion.csv
│  ├─ demo_lpp.tsv
│  ├─ demo_npp.tsv
│  └─ species_clades.csv
├─ results/
│  └─ example_multi_cluster_heatmap.png
└─ README.md

````

---

## 📦 Requirements

- R 4.0+  
- CRAN: optparse, readr, stringr, dplyr, glue, data.table, tidyr, tools, randomcoloR  
- Bioconductor: ComplexHeatmap, circlize  
- Automatic dependency installation supported via `BiocManager`  
- Recommended reproducible environment: **renv** or Docker  
- On Windows: Cairo support may be required for high-quality PNG rendering

---

## 🧪 Input Formats

### 1) Gene List (Scripts 1 & 2)
- Provided as `"GENE1,GENE2,..."` or CSV with one gene per row.

Example:
```csv
gene
ABCA4
RHO
USH2A
````

### 2) Cluster Details (Script 3)

CSV with one gene per row, including cluster information and evidence annotations.

Required columns: `cluster_id`, `cluster_genes`, `Inclusion_criterion`

Example:

```csv
cluster_id,cluster_genes,Inclusion_criterion
949,MT-ND5,Literature
949,MT-CYB,Literature
1257,TTC30B,CiliaCarta: Gold Standard
1257,CLUAP1,CiliaCarta: Predicted
```

### 3) LPP/NPP Profile Matrices

Tab/CSV/RDS tables with first column = gene names and remaining columns = species/taxids.

Example:

```
gene    9606    10090   3702
ABCA4   0.95    0.85    0.02
RHO     0.99    0.88    0.01
```

### 4) Clades Mapping

Defines species-to-clade relationships and fixes column order.

```csv
scientific_name,taxid,clade
Homo_sapiens,9606,Mammalia
Mus_musculus,10090,Mammalia
Danio_rerio,7955,Actinopteri
```

---

## ▶️ How to Run — Examples

### Standard LPP Heatmap

```bash
Rscript scripts/gene_list_to_lpp_heatmap.R \
  --genes "ABCA4,RHO,USH2A" \
  --lpp input/demo_lpp.tsv \
  --clades input/species_clades.csv \
  --outdir heatmap_runs \
  --out_prefix demo_lpp \
  --min_height_in 8
```

### Standard NPP Heatmap

```bash
Rscript scripts/gene_list_to_npp_heatmap.R \
  --csv input/demo_genes.csv \
  --npp input/demo_npp.tsv \
  --clades input/species_clades.csv \
  --row_order cluster \
  --z_clip 3 \
  --outdir heatmap_runs \
  --out_prefix demo_npp
```

### Multi-Cluster Annotated LPP Heatmap

```bash
Rscript scripts/lpp_multi_cluster_heatmap_with_inclusion.R \
  --clusters input/demo_clusters_genes_inclusion.csv \
  --lpp input/demo_lpp.tsv \
  --species input/species_clades.csv \
  --selected "949,1257" \
  --outdir heatmap_runs \
  --label demo_multi_cluster \
  --seed 42
```

---

## 🎛️ Main CLI Options (Summary)

Common options:

* `--clusters`, `--csv`, `--genes`
* `--lpp`, `--npp`
* `--species`, `--clades`
* `--outdir`
* `--out_prefix`, `--label`
* `--seed`
* `--png_width`, `--min_height_in`, `--png_height_per_gene`

NPP-specific:

* `--drop_first_rowcol`
* `--z_clip`
* `--row_order` (cluster / input / file)

Multi-cluster specific:

* `--selected`
* `--allow_explode`
* `--method` (average / single, etc.)

---

## 📤 Outputs (Per Run)

Each execution generates:

* Main heatmap PNG (optional PDF)
* Sorted matrix files
* Row-order file
* `runinfo_<timestamp>.txt` (all parameters)
* `runlog_<timestamp>.txt` (console output)
* Optional `session_info.txt`

All outputs are stored in timestamped subfolders.

---

## 🖼️ Example Outputs

### 1. Multi-Cluster Annotated LPP (Figure X3)

![LPP heatmap showing multiple clusters with Inclusion criteria annotations](results/Clusters_Dominated_by_Known_Genes.png)

### 2. Standard NPP Heatmap (Figure X4)

![Example NPP heatmap](results/NPP_7_genes_HeatMap_2025.png)

### 3. Standard LPP Heatmap (Figure X5)

![Example LPP heatmap](results/LPP_7_genes_HeatMap_2025.png)

---

## 🧠 Key Design Decisions

* **Clustering:** Pearson distance with average/single linkage for stable co-evolutionary grouping
* **Species Columns:** Fixed phylogenetic order for biological interpretability
* **Color Scales:**

  * LPP: sequential (0→1)
  * NPP: diverging with optional clipping
* **Reproducibility:** Timestamped runs, logs, dependency checks, environment capture

---

## 🔧 Troubleshooting

* Matrix reading errors: check separators and headers
* Missing genes: verify symbol consistency (case-insensitive rescue included)
* Species mismatch: ensure taxid/scientific_name matches the LPP/NPP matrix
* Figure cropping: tune `--png_width`, `--min_height_in`, `--png_height_per_gene`

---

## ⚠️ Data & Privacy Disclaimer

> **All data used in this project are synthetic and provided solely for demonstration purposes.**

Real datasets (LPP/NPP matrices, species maps, gene clusters, or any internal laboratory resources) remain confidential.  
All demo files in `input/` are small, randomly generated tables intended only to illustrate script functionality.

---

*This project is part of the Evolutionary Genomics & Multi-Omics Portfolio.*
