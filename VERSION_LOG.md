# Version Log

This file records changes and additions to the portfolio.

| Date       | Section                          | Description                                                                                                                                                                                             |
|------------|----------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 2025-10-27 | Setup                            | Created repository structure (README, LICENSE, .gitignore, projects/, notebooks/).                                                                                                                        |
| 2025-10-27 | Projects / Structure             | Added initial project templates: Cilia_Module_Validation, LPP_NPP_Heatmap_Visualization, LBS_Consensus_Profiling; added standard `.keep` files.                                            |
|            |                                  |                                                                                                                                                                                                         |
| 2025-10-27 | Project: LPP_NPP_Heatmap_Visualization | Added initial R scripts (`gene_list_to_lpp_heatmap.R`, `gene_list_to_npp_heatmap.R`) with full documentation and CLI options.                                                                                |
| 2025-10-28 | Project: LPP_NPP_Heatmap_Visualization | Added advanced R script `lpp_multi_cluster_heatmap_with_inclusion.R` for multi-cluster visualization with annotations and full CLI.                                                                       |
| 2025-10-28 | Project: LPP_NPP_Heatmap_Visualization | Added demo input templates (`input/`) and example result images (`results/NPP_7_genes_HeatMap_2025.png`, `results/LPP_7_genes_HeatMap_2025.png`, `results/Clusters_Dominated_by_Known_Genes.png`).           |
| 2025-10-28 | Project: LPP_NPP_Heatmap_Visualization | Added detailed project README, explaining scripts, inputs, outputs, and embedding example result images.                                                                                                    |
|            |                                  |                                                                                                                                                                                                         |
| 2025-10-28 | Project: Cilia_Module_Validation        | Added 7 Python scripts (`extract_cilia_annotations.py` to `cluster_evidence_viz.py`) comprising the complete computational pipeline for cluster analysis (Filter, Enrich, Quantify, Validate, Visualize). |
| 2025-10-28 | Project: Cilia_Module_Validation        | Added detailed project README outlining the scientific rationale, the 6-step computational workflow, script descriptions, and usage examples.                                                            |
| 2025-10-28 | Project: Cilia_Module_Validation        | Added example result images (`results/gene_category_distribution_per_cluster.png`, `results/Clusters_Dominated_by_Known_Genes.png`) and embedded them in the README.                                        |
| 2026-06-17 | Project: IRD_Phenotype_Clustering | Replaced empty stub with full synthetic demo (notebook, synthetic gene-HPO matrix, demo figures, updated README). |
| 2026-06-18 | Project: Cilia_Module_Validation | Fixed script filename mismatches, removed orphan stub, corrected broken How to Run example, generalized unpublished quantitative findings. |
| 2026-06-18 | Project: IRD_HPO_Anatomogram | Replaced unredacted real notebooks (01/02) with one self-contained synthetic demo notebook; fixed broken Input/data path mismatch; regenerated gganatogram_organs_input_DEMO.csv from actual demo data; updated README accordingly. |
| 2025-11-24 | Project: LBS_Consensus_Profiling | Added demo notebook (`consensus_profile_demo.ipynb`) covering five aggregation strategies with coherence validation and noise-based negative control. |
| 2025-11-26 | Project: LBS_Consensus_Profiling | Added project README with scientific rationale, workflow description, and usage instructions. |
| 2026-06-20 | README                           | Added Repository Structure section with annotated file tree and project overview prose. |
| 2026-06-20 | Housekeeping                     | Removed internal process documents (`PORTFOLIO_GUIDE.md`, `PORTFOLIO_QUICK_GUIDE.md`) not intended for public readers. |
