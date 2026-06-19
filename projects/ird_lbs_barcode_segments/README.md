# IRD LBS Barcode Segments — Consensus Phylogenetic Profiling

![Language](https://img.shields.io/badge/Language-Python-3776AB) ![Focus](https://img.shields.io/badge/Focus-Consensus_Phylogenetic_Profiling-7B1FA2) ![Status](https://img.shields.io/badge/Status-Portfolio_Demo-00695C)

*Shalev Yaacov — A synthetic demonstration of consensus-based phylogenetic
profiling and evolutionary barcode detection for gene module analysis.*

---

## Overview

This project demonstrates a consensus phylogenetic profiling workflow built
around the concept of a Local evolutionary Barcode Segment (LBS): a species
window in which a gene group displays a distinctive, shared conservation
pattern that can serve as a query signature for genome-wide candidate
retrieval. The implementation uses a fully synthetic NPP matrix and covers
the complete pipeline from species window selection through genome-wide
correlation ranking and coherence validation. It is a methodological
exploration — a side project extending the broader IRD analysis framework —
not a validated component of the main pipeline.

---

## Rationale

Phylogenetic profiles across ~1,900 species are not uniformly informative.
A gene group defined by shared biology — a ciliopathy module, a retinal
degeneration cluster — does not leave an identical evolutionary signature
across the entire tree of life. The signal tends to concentrate in a specific
species window where that group's function either emerged or was lost in
parallel, producing a local pattern that is visually distinct in a heatmap
and statistically separable from the genomic background. That window is the
"Local" in LBS; the consensus profile of the group within it is the
"Barcode" — a compact evolutionary signature that encodes what makes that
module evolutionarily distinctive.

The methodological challenge is translating a visual observation into an
algorithm. Identifying the informative species window begins with heatmap
inspection — recognizing a region where the input gene set displays coherent,
group-specific behavior. The next step is formalizing that observation:
an algorithm detects the candidate window computationally, and a coherence
metric then determines whether the consensus built from it constitutes a
genuine barcode or collapses under noise. This progression from biological
intuition to algorithmic formalization to quantitative validation is the
central methodological contribution the project explores.

Aggregating multiple gene profiles into a single consensus vector rather
than searching with individual genes reduces gene-specific noise and
increases ranking stability. Five aggregation strategies are compared —
mean, median, trimmed mean, medoid, and PC1 — because each emphasizes
different statistical properties of the shared signal. Comparing them
directly exposes which aspects of evolutionary coherence are robust across
methods and which are method-dependent, a necessary step before treating
any consensus as a reliable query.

---

## Pipeline

1. **Synthetic data generation** — structured NPP matrix (20,000 genes ×
   1,900 species) with 50 functional blocks; a 7-gene segment is extracted
   from a strongly correlated block as the LBS.
2. **Species subset selection** — synthetic similarity scores simulate
   heatmap-guided identification of the most informative species window;
   ~400 species are retained.
3. **Consensus profile generation** — five methods (mean, median, trimmed
   mean, medoid, PC1) applied to the 7-gene LBS within the selected species.
4. **Genome-wide correlation ranking** — each consensus vector correlated
   against all 20,000 genes (Pearson and Spearman) restricted to the 400
   selected species.
5. **Coherence validation** — pairwise correlations within top-N ranked
   genes quantify whether high-ranking genes form a coherent evolutionary
   module.
6. **Negative control** — a randomized 7-gene segment is used as a control;
   its consensus collapses in coherence validation, confirming the pipeline
   distinguishes real signal from noise.

---

## Files

```
ird_lbs_barcode_segments/
├── notebook/
│   └── consensus_profile_demo.ipynb
├── data/
│   ├── synthetic_segments.csv
│   └── synthetic_full_matrix.csv
├── outputs/
│   └── demo_figures/
│       ├── segment_heatmap.png
│       ├── consensus_comparison.png
│       ├── coherence_boxplot.png
│       ├── correlation_distributions.png
│       └── signal_vs_noise_coherence.png
└── README.md
```

---

## Connection to Heatmap Visualization

Species window selection is guided by visual inspection of phylogenetic
heatmaps. An example of the heatmap output that informs this step is in the
`heatmap_visualization` project:

![Example phylogenetic heatmap used to guide LBS selection](../heatmap_visualization/results/NPP_7_genes_HeatMap_2025.png)

---

## Running the Demo

```bash
# Install dependencies
pip install numpy pandas scipy scikit-learn seaborn matplotlib jupyter

# Launch
jupyter notebook notebook/consensus_profile_demo.ipynb
```

Runtime: ~5–10 minutes. RAM: 4–8 GB recommended.

---

## Dependencies

```
numpy, pandas, scipy, scikit-learn, seaborn, matplotlib
```

---

## Data & Privacy Disclaimer

> All data in this project are fully synthetic. No real NPP matrices,
> gene identifiers, or internal laboratory data are included. Synthetic
> data emulate realistic evolutionary patterns solely for demonstration
> purposes.

---

*Part of the Evolutionary Genomics & Multi-Omics Portfolio.*

