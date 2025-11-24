# IRD Phenotype-Based Gene Clustering Pipeline

A comprehensive computational pipeline for clustering Inherited Retinal Disease (IRD) genes based on Human Phenotype Ontology (HPO) annotations using semantic similarity analysis.

## Project Overview

This project implements a multi-stage pipeline that transforms raw gene lists into phenotype-driven gene clusters. The pipeline uses semantic similarity measures (Resnik + Best Match Average) to identify functionally related gene modules based on shared phenotypic profiles.

## Scientific Background

### Inherited Retinal Diseases (IRD)
Inherited Retinal Diseases are a group of genetic disorders affecting the retina, leading to progressive vision loss. With over 300 known disease-causing genes, understanding phenotypic relationships is crucial for identifying shared disease mechanisms and developing targeted therapies.

### Human Phenotype Ontology (HPO)
The HPO provides a standardized vocabulary of phenotypic abnormalities. Genes are annotated with HPO terms based on phenotypes observed in patients with mutations, enabling semantic similarity computation between genes.

## Project Structure

```
ird-phenotype-clustering/
├── README.md
├── notebook/
│   └── IRD_phenotype_Clustering_Pipeline.ipynb
├── data/
│   ├── raw/        # Input data (place your files here)
│   └── processed/  # Intermediate processed data
└── outputs/        # Final outputs (figures, results)
```

## Prerequisites

### Required Python Packages

```bash
pip install pandas numpy scipy matplotlib seaborn scikit-learn mygene obonet networkx
```

### Optional (for advanced visualizations)
```bash
pip install umap-learn leidenalg python-igraph
```

### Required Input Data

1. **Gene List**: A CSV file with one gene symbol per line
   - Place in: `data/raw/genes_list.csv`
   - Format: One gene symbol per line (e.g., `ABCA4`, `RPGR`, etc.)

2. **HPO Ontology File**: `hp.obo`
   - Download from: [HPO Downloads](https://hpo.jax.org/app/download/ontology)
   - Place in: `data/raw/hp.obo`

3. **HPO Annotations**: `genes_to_phenotype.txt`
   - Download from: [HPO Downloads](https://hpo.jax.org/app/download/annotation)
   - Place in: `data/raw/genes_to_phenotype.txt`

## Pipeline Stages

The integrated notebook contains three main stages:

### Stage 1: Initial Pipeline (Scripts 01-05B)

1. **Gene Normalization** (01)
   - Queries MyGene.info to standardize gene symbols
   - Retrieves Entrez and Ensembl IDs
   - Output: Normalized gene master list

2. **HPO Annotation Extraction** (02, 02a, 02b)
   - Maps genes to HPO phenotype terms
   - Creates gene-HPO annotation tables
   - Output: Long-format and summary annotation tables

3. **Binary Gene-HPO Matrix** (03)
   - Constructs binary annotation matrix (genes × HPO terms)
   - Output: Binary matrix CSV file

4. **Information Content Computation** (04)
   - Calculates IC for HPO terms based on annotation frequency
   - Propagates annotations through HPO hierarchy
   - Output: IC table for all HPO terms

5. **Resnik Term-Term Similarity** (05A)
   - Pre-computes semantic similarity between HPO terms
   - Uses Resnik method (IC of Most Informative Common Ancestor)
   - Output: Term-term similarity matrix

6. **Gene-Gene Similarity (BMA)** (05B)
   - Computes Best Match Average similarity between all gene pairs
   - Output: Initial gene-gene similarity matrix

### Stage 1.5: IC Rebuild and Similarity Recomputation (Script 13)

**Note**: This is an improved version that replaces the original IC computation with better discrimination.

- **Filters overly general HPO terms** (removes top 2 levels, ~94% of terms)
- **Recomputes IC** using both depth-based and frequency-based methods
- **Rebuilds similarity matrices** with improved selectivity
- **Output**: Improved gene-gene similarity matrix (mean ~0.16 vs ~5.48 in original)

### Stage 2: Phenotype-Based Modular Analysis (Updated Methodology)

**Note**: This replaces the original clustering approach (scripts 06-10) with a more sophisticated modular analysis.

1. **Graph Construction**
   - Creates k-NN and threshold-based graphs from similarity matrix
   - Tests multiple graph variants for optimal module detection

2. **Community Detection**
   - Uses Leiden/Louvain algorithms for module identification
   - Evaluates multiple graph variants

3. **Quality Assessment**
   - Silhouette analysis for module quality
   - Within/between similarity metrics
   - Stability analysis via resampling/perturbation

4. **Core/Peripheral Classification**
   - Classifies genes as core, peripheral, or unassigned
   - Based on silhouette scores and stability metrics

5. **Output**: Final phenotype-driven gene modules with comprehensive quality assessments

## How to Run

### Step 1: Prepare Input Data

1. Create a gene list file (`data/raw/genes_list.csv`) with one gene symbol per line:
   ```
   ABCA4
   RPGR
   USH2A
   ...
   ```

2. Download and place HPO files:
   - `data/raw/hp.obo`
   - `data/raw/genes_to_phenotype.txt`

### Step 2: Open the Notebook

```bash
jupyter notebook notebook/IRD_phenotype_Clustering_Pipeline.ipynb
```

### Step 3: Run Cells Sequentially

Execute cells in order from top to bottom. Each stage builds upon the previous one:

- **Stage 1** (scripts 01-05B) must complete before **Stage 1.5**
- **Stage 1.5** (IC rebuild) must complete before **Stage 2**
- **Stage 2** uses the improved similarity matrix from Stage 1.5

**Important**: The integrated notebook uses an improved methodology:
- Stage 1.5 replaces the original IC computation with better filtering
- Stage 2 replaces simple hierarchical clustering with graph-based modular analysis

### Step 4: Review Outputs

Outputs are saved to timestamped files in:
- `data/processed/` - Intermediate processed data
- `outputs/` - Final results, visualizations, and reports

## Key Outputs

### Data Files
- **Gene Master List**: Normalized gene identifiers with Entrez/Ensembl IDs
- **Gene-HPO Annotations**: Mapping of genes to HPO terms
- **Similarity Matrices**: Gene-gene and term-term similarity matrices
- **Cluster Assignments**: Final cluster membership for each gene

### Visualizations
- **Heatmaps**: Gene-gene similarity ordered by clustering
- **Dendrograms**: Hierarchical clustering structure
- **Cluster Plots**: PCA/UMAP projections colored by cluster
- **Quality Metrics**: Silhouette scores and stability plots

### Reports
- **QC Reports**: Data quality and validation summaries
- **Cluster Summaries**: Statistics and metrics for each cluster
- **Log Files**: Detailed execution logs for each stage

## Methodology

### Semantic Similarity Computation

1. **Information Content (IC)**: Measures specificity of HPO terms
   - IC = -log(frequency)
   - Higher IC = more specific term

2. **Resnik Similarity**: Term-term semantic similarity
   - Resnik(term1, term2) = IC(MICA)
   - MICA = Most Informative Common Ancestor

3. **Best Match Average (BMA)**: Gene-gene similarity
   - BMA = average of best matching HPO terms between gene pairs
   - Accounts for genes with different numbers of annotations

### Clustering Approach

- **Hierarchical Clustering**: Complete linkage method
- **Normalization Strategies**:
  - Raw: No normalization
  - Row Z-score: Normalize each gene's similarity profile
  - Global Z-score: Normalize entire matrix
- **Distance Metric**: Correlation-based distance from similarity matrix

### Quality Assessment

- **Silhouette Analysis**: Measures how well genes fit their clusters
- **Stability Analysis**: Resampling-based robustness assessment
- **Adjusted Rand Index (ARI)**: Compares cluster consistency across runs

## Expected Results

When run on a typical IRD gene set (~400-500 genes):

- **Normalization**: ~95-99% of genes successfully normalized
- **HPO Coverage**: ~85-95% of genes have HPO annotations
- **Similarity Matrix**: Sparse matrix with mean similarity ~0.1-0.2
- **Clusters**: 10-30 distinct phenotype-driven clusters identified
- **Quality**: Silhouette scores typically 0.2-0.4 for well-separated clusters

## Demonstration Outputs

**Important**: This repository includes **demonstration outputs only** for portfolio purposes.

- The `outputs/demo/` folder contains synthetic visualizations generated from randomly created data
- The notebook includes demonstration cells that show the pipeline workflow using synthetic data
- **No real IRD gene, phenotype, or similarity data is included** due to confidentiality and data protection requirements
- All demo figures are clearly labeled as "Synthetic Data - Demonstration Only"

To see the pipeline in action, run the demonstration cells in the notebook, which will generate example visualizations from synthetic similarity matrices.

## Troubleshooting

### Common Issues

1. **Missing Input Files**
   - Ensure all required files are in `data/raw/`
   - Check file names match exactly

2. **MyGene.info API Errors**
   - Check internet connection
   - API may be temporarily unavailable
   - Consider using cached results

3. **Memory Issues**
   - Large gene sets (>1000 genes) may require more RAM
   - Consider processing in batches

4. **HPO File Format**
   - Ensure HPO files are from official HPO database
   - Check file encoding (should be UTF-8)

## Citation

If you use this pipeline in your research, please cite:

- Human Phenotype Ontology: [Köhler et al., 2021](https://doi.org/10.1093/nar/gkaa1043)
- MyGene.info: [Wu et al., 2016](https://doi.org/10.1093/nar/gkv1145)

## License

This pipeline is provided for research purposes. Please ensure compliance with:
- HPO database terms of use
- MyGene.info API terms of service

## Author

**Shalev Yaacov**

## Acknowledgments

- Human Phenotype Ontology Consortium
- MyGene.info team
- Open source Python community

---

**Important Notes**:

- **Demonstration Outputs Only**: This repository includes synthetic demonstration outputs only. No real IRD gene, phenotype, or similarity data is included due to confidentiality requirements.
- **Synthetic Data**: All demo figures and example outputs are generated from randomly created synthetic data for portfolio demonstration purposes.
- **Production Use**: For production use with real data, ensure all input files are properly formatted and placed in the correct directories.
- **Data Privacy**: Real research data cannot be shared publicly. The demonstration cells in the notebook show the pipeline workflow using synthetic examples.

