# Heatmap Visualization — LPP & NPP Phylogenetic Profiles

![Language](https://img.shields.io/badge/Language-R-276DC3) ![Focus](https://img.shields.io/badge/Focus-Phylogenetic_Profiling_Visualization-7B1FA2) ![Status](https://img.shields.io/badge/Status-Publication_Ready-00695C)

*Shalev Yaacov — R scripts for generating publication-quality heatmaps
of Local and Normalized Phylogenetic Profiles (LPP / NPP).*

---

## Overview

These scripts take a gene list and a phylogenetic profile matrix as input
and produce a ComplexHeatmap figure with phylogenetically ordered species
columns, clade color annotation, and hierarchical row clustering. Both LPP
(probabilistic presence/absence, range 0–1) and NPP (z-scored evolutionary
signal) are supported. A third script extends the single-gene-list design to
multiple clusters simultaneously, adding per-cluster row gaps and an
Inclusion Criterion annotation track. The output figures are publication-ready
and were used directly in manuscript preparation without post-processing.

---

## Rationale

Phylogenetic profiling encodes the evolutionary conservation of each gene as
a numeric vector across hundreds of species. The structure in that vector —
which clades retain the gene, where conservation drops, which genes share
similar patterns — is the primary biological signal. A heatmap with
phylogenetically ordered columns and hierarchically clustered rows makes that
structure immediately visible: conservation boundaries align with clade
transitions, and genes that co-evolve appear adjacent after clustering.

LPP and NPP capture complementary aspects of the same signal. LPP values
reflect the probability that a gene is present in a given species, emphasizing
discrete conservation boundaries and presence/absence transitions across
evolutionary distance. NPP z-scores capture the continuous deviation from
genome-wide expectation, revealing subtle co-evolutionary trends that LPP
thresholding may suppress. Using both on the same gene set exposes structure
that neither alone resolves.

The multi-cluster script adds a validation layer. After phenotypic clustering
produces gene modules, the question is whether those modules also carry
evolutionary coherence — whether genes grouped by clinical phenotype similarity
also co-evolve. Rendering multiple clusters in a single heatmap with
Inclusion Criterion annotation (Known IRD, Candidate, Gold Standard, Predicted)
makes convergence between the phenotypic and evolutionary signals directly
visible, which is the core claim the downstream analysis rests on.

---

## Scripts

- `gene_list_to_lpp_heatmap.R` — standard LPP heatmap for a single gene list;
  row clustering by 1 − Pearson distance, average linkage.
- `gene_list_to_npp_heatmap.R` — NPP z-score heatmap; diverging color scale
  with optional z-clipping; supports cluster, input, or file-defined row order.
- `lpp_multi_cluster_heatmap_with_inclusion.R` — multi-cluster LPP heatmap
  with row gaps between clusters and an Inclusion Criterion annotation track.

---

## Files

```
heatmap_visualization/
├── scripts/
│   ├── gene_list_to_lpp_heatmap.R
│   ├── gene_list_to_npp_heatmap.R
│   └── lpp_multi_cluster_heatmap_with_inclusion.R
├── input/
│   ├── demo_genes.csv
│   ├── demo_lpp.tsv
│   ├── demo_npp.tsv
│   ├── demo_clusters_genes_inclusion.csv
│   └── species_clades.csv
├── results/
│   ├── Clusters_Dominated_by_Known_Genes.png
│   ├── NPP_7_genes_HeatMap_2025.png
│   └── LPP_7_genes_HeatMap_2025.png
└── README.md
```

---

## Input Formats

**Gene list** (`demo_genes.csv`) — one column, header `gene`:
```csv
gene
GENE_A1
GENE_A2
GENE_A3
```

**LPP/NPP matrix** (`demo_lpp.tsv`, `demo_npp.tsv`) — tab-separated;
first column = gene names, remaining columns = species taxids or
scientific names:
```
gene    9606    10090   7955
GENE_A1 0.95    0.87    0.03
GENE_A2 0.91    0.79    0.01
```

**Cluster details** (`demo_clusters_genes_inclusion.csv`) — required columns
`cluster_id`, `cluster_genes`, `Inclusion_criterion`:
```csv
cluster_id,cluster_genes,Inclusion_criterion
CLUSTER_01,GENE_A1,CiliaCarta: Gold Standard
CLUSTER_01,GENE_A2,CiliaCarta: Gold Standard
CLUSTER_02,GENE_B1,Novel cilia-associated candidate
CLUSTER_02,GENE_B2,Novel cilia-associated candidate
```

**Species–clade map** (`species_clades.csv`) — required columns
`scientific_name`, `taxid`, `clade`:
```csv
scientific_name,taxid,clade
Homo_sapiens,9606,Mammalia
Mus_musculus,10090,Mammalia
Danio_rerio,7955,Actinopteri
```

---

## Example Usage

**Standard LPP heatmap:**
```bash
Rscript scripts/gene_list_to_lpp_heatmap.R \
  --csv input/demo_genes.csv \
  --lpp input/demo_lpp.tsv \
  --clades input/species_clades.csv \
  --outdir results/demo \
  --out_prefix LPP_demo \
  --min_height_in 6
```

**Standard NPP heatmap:**
```bash
Rscript scripts/gene_list_to_npp_heatmap.R \
  --csv input/demo_genes.csv \
  --npp input/demo_npp.tsv \
  --clades input/species_clades.csv \
  --row_order cluster \
  --z_clip 3 \
  --outdir results/demo \
  --out_prefix NPP_demo
```

**Multi-cluster LPP heatmap:**
```bash
Rscript scripts/lpp_multi_cluster_heatmap_with_inclusion.R \
  --clusters input/demo_clusters_genes_inclusion.csv \
  --lpp input/demo_lpp.tsv \
  --species input/species_clades.csv \
  --outdir results/demo \
  --label MultiCluster_demo \
  --seed 42
```

---

## Example Outputs

### Figure 1 — Multi-Cluster LPP with Inclusion Criterion Annotation
![Multi-cluster LPP heatmap with per-cluster row gaps and evidence annotation](results/Clusters_Dominated_by_Known_Genes.png)

### Figure 2 — Standard NPP Heatmap
![NPP z-score heatmap with diverging color scale and clade annotation](results/NPP_7_genes_HeatMap_2025.png)

### Figure 3 — Standard LPP Heatmap
![LPP presence/absence heatmap with hierarchical row clustering](results/LPP_7_genes_HeatMap_2025.png)

---

## Dependencies

```
# CRAN
optparse, readr, stringr, dplyr, glue, data.table, tidyr, randomcoloR

# Bioconductor
ComplexHeatmap, circlize
```

Automatic installation via `BiocManager` is included in each script.
Recommended environment management: **renv** or Docker.
On Windows, Cairo support may be required for high-resolution PNG export.

---

## Data & Privacy Disclaimer

> All gene names, cluster IDs, and profile values in `input/` are entirely
> synthetic and provided solely to demonstrate script functionality. No real
> LPP/NPP matrices, gene clusters, or internal laboratory data are included.

---

*Part of the Evolutionary Genomics & Multi-Omics Portfolio.*
