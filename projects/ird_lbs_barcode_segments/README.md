# Consensus Phylogenetic Profile Analysis - Synthetic Demo

**Portfolio-ready demonstration using fully synthetic data**

---

## 🔒 Privacy Disclaimer

> **All data in this repository are fully synthetic and included for demonstration only.**
>
> **No real genomic, evolutionary, or disease-related data are included.**

This repository contains a computational framework demonstration for building consensus phylogenetic profiles. All gene names, species names, and data values are synthetically generated for methodological illustration purposes.

---

## 📋 Table of Contents

1. [Scientific Background](#scientific-background)
2. [Project Overview](#project-overview)
3. [Synthetic Data Structure](#synthetic-data-structure)
4. [Pipeline Methodology](#pipeline-methodology)
5. [Running the Demo](#running-the-demo)
6. [Output Files](#output-files)
7. [Visualizations](#visualizations)

---

## 🧬 Scientific Background

### Consensus Phylogenetic Profiling

Phylogenetic profiling is a computational method that identifies functionally related genes by analyzing their co-occurrence patterns across species. The core principle is that genes involved in the same biological pathway or complex tend to be present or absent together across evolutionary lineages.

**Consensus profiling** extends this approach by:

1. **Starting with a known gene set** (e.g., 7 genes associated with a disease pathway)
2. **Building multiple consensus profiles** using different statistical methods
3. **Searching a large gene database** (~20,000 genes) to find genes with similar evolutionary patterns
4. **Validating results** by measuring internal coherence of top-ranked gene lists

This methodology is particularly useful for:
- Identifying novel disease-associated genes
- Discovering functional gene modules
- Validating pathway predictions
- Comparative genomics analysis

---

## 📊 Project Overview

This synthetic demonstration implements a complete computational pipeline for consensus phylogenetic profile analysis. The workflow includes:

### Key Components

1. **Synthetic Data Generation**
   - Full NPP matrix: 20,000 genes × 1,900 species
   - Structured patterns to mimic evolutionary signal
   - Block-based correlation structure

2. **Species Subset Selection**
   - Synthetic similarity-based selection
   - ~400 species subset (mimicking phylogenetic clustering)
   - Reproduces the methodology of selecting species based on phylogenetic heatmaps

3. **Consensus Profile Generation**
   - 5 different statistical methods
   - Robust comparison across methods

4. **Correlation-Based Ranking**
   - Genome-wide search (~20,000 genes)
   - Restricted to selected species subset
   - Dual correlation metrics (Pearson & Spearman)

5. **Coherence Validation**
   - Internal pairwise correlation analysis
   - Multiple top-N thresholds (50, 100, 200)
   - Quantitative validation of co-evolving groups

6. **Negative Control**
   - Randomized segment demonstration
   - Coherence collapse validation

---

## 🗂️ Synthetic Data Structure

### Two-Level Matrix Architecture

The pipeline uses a **two-level matrix structure** that mirrors real phylogenetic profiling workflows:

#### A. Full Synthetic NPP Matrix
- **Size**: 20,000 genes × 1,900 species
- **Structure**:
  - 50 functional blocks (400 genes per block)
  - Within-block correlation: 0.3-0.8
  - Phylogenetic clustering across species
  - Global noise component
- **Purpose**: Represents the complete gene database for correlation search

#### B. Species Subset Selection (~400 species)

**In the real analysis**, the 400-species subset is determined using a phylogenetic heatmap that identifies coherent evolutionary clusters.

**In this synthetic demo**, the subset is selected using synthetic rules that mimic clustered evolutionary behavior:
- Generate synthetic similarity scores per species
- Create 8 phylogenetic clusters
- Select top 400 species based on similarity scores
- This simulates the process of identifying a coherent phylogenetic region from a heatmap

#### C. Synthetic Segment Extraction
- **Size**: 7 genes × 400 species
- **Source**: Extracted from a coherent block in the full matrix
- **Purpose**: Acts as the synthetic "barcode" used to build consensus profiles
- **Methodology**: Selects 7 consecutive genes from the middle of a functional block to ensure correlation structure

#### D. Correlation Search Restriction

When ranking all ~20,000 genes, correlations are computed **only over the 400 selected species**, exactly matching the original methodology. This restriction:
- Focuses analysis on a coherent phylogenetic region
- Reduces noise from distantly related species
- Mimics real-world analysis where species selection is based on phylogenetic relationships

---

## 🔬 Pipeline Methodology

### Step 1: Synthetic Data Generation

Generate a full NPP matrix with structured patterns:

```python
# 50 functional blocks
# Each block: 400 genes with correlated profiles
# Phylogenetic clustering: 10 species clusters per block
# Correlation strength: 0.3-0.8 within blocks
```

**Key Features:**
- Block structure mimics functional gene modules
- Phylogenetic clustering mimics evolutionary relationships
- Noise component ensures realistic variability

### Step 2: Species Subset Selection

Simulate phylogenetic heatmap-based selection:

```python
# Generate synthetic similarity scores
# Create 8 phylogenetic clusters
# Select top 400 species by similarity
```

**Note**: In real analysis, this step uses actual phylogenetic heatmaps to identify coherent species clusters. The synthetic version demonstrates the same methodological principle.

### Step 3: Segment Extraction

Extract a 7-gene segment from a coherent block:

```python
# Select from middle of a functional block
# Ensures genes have correlation structure
# Restrict to 400 selected species
```

### Step 4: Consensus Profile Generation

Generate 5 consensus vectors using different statistical methods:

#### 4.1 Mean
- Simple average across the 7 gene profiles
- Most straightforward method
- Sensitive to outliers

#### 4.2 Median
- Median value across genes for each species
- Robust to outliers
- Less sensitive to extreme values

#### 4.3 Trimmed Mean
- Average after removing 1 minimum and 1 maximum value per species
- Balances robustness with information retention
- For 7 genes: removes ~14% of extreme values

#### 4.4 Medoid
- Selects the most "central" gene profile
- Based on 1-Pearson correlation distance
- Gene with minimum sum of distances to all others
- Represents the most representative individual profile

#### 4.5 PC1 (Principal Component 1)
- First principal component of the 7 gene profiles
- Captures the dominant pattern of variation
- Standardized before PCA to account for scale differences
- Represents the direction of maximum variance

### Step 5: Correlation-Based Ranking

For each consensus profile:

1. **Compute correlations** against all ~20,000 genes
2. **Restrict to 400 selected species** (critical methodological detail)
3. **Use both Pearson and Spearman** correlation
4. **Rank genes** by absolute correlation strength

**Pearson Correlation:**
- Linear relationships
- Sensitive to outliers
- Assumes normal distribution

**Spearman Correlation:**
- Rank-based, non-parametric
- Robust to outliers
- Captures monotonic relationships

### Step 6: Coherence Validation

For each consensus method and correlation type:

1. **Extract top-N lists** (e.g., top 50, 100, 200)
2. **Compute pairwise correlations** between all genes in the list
3. **Calculate mean absolute correlation** as coherence measure
4. **Validate** that top-ranked genes form coherent groups

**Coherence Measure:**
```
Coherence = mean(|pairwise_correlations|)
```

High coherence indicates that top-ranked genes are truly co-evolving, not just individually correlated with the consensus.

### Step 7: Negative Control

Demonstrate that coherence collapses when signal is removed:

1. **Generate randomized segment** (7 genes × 400 species, pure noise)
2. **Build consensus** from noise segment
3. **Compute correlations** and extract top-N lists
4. **Measure coherence** - should be significantly lower than signal-based lists

This validates that the coherence measure is detecting real signal, not random correlations.

---

## 🚀 Running the Demo

### Prerequisites

```bash
pip install numpy pandas matplotlib seaborn scipy scikit-learn jupyter
```

### Required Packages

- `numpy` - Numerical computing
- `pandas` - Data manipulation
- `matplotlib` - Plotting
- `seaborn` - Statistical visualization
- `scipy` - Statistical functions (trim_mean)
- `scikit-learn` - PCA and scaling
- `jupyter` - Notebook interface

### Execution

1. **Navigate to the project directory:**
   ```bash
   cd consensus-profile-demo
   ```

2. **Launch Jupyter Notebook:**
   ```bash
   jupyter notebook
   ```

3. **Open the notebook:**
   ```
   notebook/consensus_profile_demo.ipynb
   ```

4. **Run all cells:**
   - Use "Run All" from the Cell menu
   - Or execute cells sequentially (Shift+Enter)

### Expected Runtime

- **Data generation**: ~30-60 seconds
- **Consensus computation**: ~10-20 seconds
- **Correlation analysis**: ~2-5 minutes (20,000 genes × 5 methods × 2 correlation types)
- **Coherence validation**: ~1-2 minutes
- **Visualizations**: ~30 seconds
- **Total**: ~5-10 minutes

### Memory Requirements

- **Minimum**: 4 GB RAM
- **Recommended**: 8 GB RAM
- Matrix size: ~300 MB in memory

---

## 📁 Output Files

### Generated Data Files

Located in `data/`:

1. **`synthetic_segments.csv`**
   - 7 genes × 400 species
   - The extracted segment used for consensus building
   - CSV format with gene names as index

2. **`synthetic_full_matrix.csv`**
   - Sample of first 1,000 genes × 1,900 species
   - Full matrix is generated in memory but only sample saved (file size considerations)
   - Demonstrates the structure of the full database

### Generated Figures

Located in `outputs/demo_figures/`:

1. **`synthetic_segment_heatmap.png`**
   - Heatmap visualization of the 7-gene segment
   - Shows profile patterns across species
   - Color-coded by profile value

2. **`consensus_comparison.png`**
   - Comparison of all 5 consensus methods
   - Overlays individual gene profiles
   - Demonstrates method differences

3. **`coherence_boxplot.png`**
   - Coherence values across methods and top-N thresholds
   - Includes negative control comparison
   - Bar chart format for easy comparison

4. **`correlation_distributions.png`**
   - Distribution of correlation scores for each consensus method
   - Compares Pearson vs. Spearman
   - Shows the range of correlations found

5. **`signal_vs_noise_coherence.png`**
   - Direct comparison: signal-based vs. noise-based coherence
   - Demonstrates validation of the methodology
   - Clear visualization of coherence collapse

**All figures include the label: "Synthetic data – demonstration only"**

---

## 📈 Visualizations

### 1. Synthetic Segment Heatmap

Visualizes the 7-gene segment across a sample of species. Shows:
- Profile patterns and structure
- Correlation between genes
- Species clustering patterns

### 2. Consensus Profile Comparison

Side-by-side comparison of all 5 consensus methods:
- Individual gene profiles (gray, transparent)
- Consensus profile (blue, bold)
- Method-specific characteristics visible

### 3. Coherence Validation Plot

Bar chart showing coherence values:
- X-axis: Consensus method × Correlation type
- Y-axis: Mean absolute pairwise correlation
- Multiple bars per method (Top 50, 100, 200)
- Includes noise control for comparison

### 4. Correlation Distributions

Histograms showing the distribution of correlation scores:
- One subplot per consensus method
- Overlay of Pearson (blue) and Spearman (orange)
- Shows the range and shape of correlations

### 5. Signal vs. Noise Comparison

Direct comparison bar chart:
- Signal-based coherence (blue)
- Noise control coherence (coral)
- Clear demonstration of methodology validation

---

## 🔍 Key Methodological Features

### Two-Level Matrix Structure

The pipeline correctly implements the two-level structure:

1. **Full matrix** (20,000 × 1,900): Complete database
2. **Species subset** (400): Phylogenetically coherent region
3. **Segment** (7 × 400): Representative barcode
4. **Correlation search**: Restricted to 400 species subset

This structure is critical because:
- Real analysis uses phylogenetic heatmaps to select species
- Correlation search must be restricted to coherent regions
- This prevents spurious correlations from distantly related species

### Species Subset Selection

**Real workflow:**
- Analyze phylogenetic heatmap
- Identify coherent clusters
- Select ~400 species from a specific region

**Synthetic simulation:**
- Generate synthetic similarity scores
- Create phylogenetic clusters
- Select top 400 species
- **Same methodological principle, synthetic implementation**

### Coherence Validation

The coherence measure validates that top-ranked genes form coherent groups:

- **High coherence**: Genes are truly co-evolving (validated)
- **Low coherence**: Genes are individually correlated but not coherent (less reliable)
- **Noise control**: Demonstrates that coherence requires real signal

---

## 📝 Notes on Synthetic Data

### Why Synthetic?

This demonstration uses fully synthetic data to:
- **Protect privacy**: No real genomic data
- **Demonstrate methodology**: Focus on computational approach
- **Portfolio-ready**: Safe for public sharing
- **Reproducible**: Fixed random seeds ensure reproducibility

### Synthetic Data Characteristics

- **Structured patterns**: Blocks and clusters mimic real evolutionary structure
- **Realistic scale**: 20,000 genes × 1,900 species matches real databases
- **Controlled correlation**: Known correlation structure allows validation
- **Noise component**: Realistic variability

### Limitations

Synthetic data cannot capture:
- True evolutionary relationships
- Biological pathway complexity
- Disease-specific patterns
- Real gene-gene interactions

**Purpose**: Demonstrate computational methodology, not biological discovery.

---

## 🎯 Summary

This synthetic demonstration successfully implements:

✅ **Full pipeline**: Data generation → Consensus → Ranking → Validation
✅ **Multiple methods**: 5 consensus approaches compared
✅ **Dual correlations**: Pearson and Spearman
✅ **Coherence validation**: Quantitative measure of co-evolving groups
✅ **Negative control**: Demonstrates methodology validation
✅ **Comprehensive visualizations**: All key results visualized
✅ **Portfolio-ready**: Fully synthetic, safe for sharing

**All data, gene names, species names, and results are synthetic and for demonstration only.**

---

## 📚 References

This methodology is based on established phylogenetic profiling approaches:

- **Phylogenetic Profiling**: Pellegrini et al. (1999), Nature
- **Consensus Methods**: Various statistical aggregation techniques
- **Coherence Validation**: Internal correlation analysis for gene set validation

---

## 📧 Contact

For questions about the methodology or implementation, please refer to the notebook documentation and code comments.

---

**Last Updated**: 2025
**Version**: 1.0
**Status**: Portfolio-ready synthetic demonstration

