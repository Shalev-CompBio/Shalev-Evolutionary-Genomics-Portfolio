# Scientific Rationale - IRD Phenotype Clustering

*Why this pipeline is built the way it is, not just what it computes.*

---

A large fraction of IRD cases remain genetically unsolved even after panel or exome sequencing. Most discovery approaches lean on genetic or evolutionary signal alone; phenotype data, despite being systematically collected and ontology-structured through HPO, is comparatively underused as an independent axis of evidence. This pipeline treats phenotypic similarity as a first-class signal in its own right, not a secondary annotation layer.

The underlying premise is that genes contributing to the same disease mechanism tend to produce convergent phenotypes, even when they are not evolutionarily or functionally related in any obvious way. Recovering that convergence computationally - grouping genes into phenotype-driven modules - creates a reference structure that an unsolved patient's phenotype profile can later be projected against, narrowing the candidate search space before any genetic evidence is considered.

Several design choices follow directly from that goal, rather than being default settings:

- **IC-based filtering** exists because raw phenotype similarity is dominated by generic terms that nearly every patient in the domain shares (for example, broad visual impairment terms). Without removing or down-weighting these by information content, similarity scores collapse toward a single uninformative cluster. Filtering on specificity, ontology depth, and gene frequency is what allows the more distinctive, mechanism-relevant terms to actually drive the signal.
- **Graph-based community detection (Leiden)**, rather than a fixed-k method such as k-means, was chosen because the number and size of true phenotypic modules is not known in advance and is unlikely to be uniform. Leiden optimizes modularity directly on the similarity graph and lets module structure emerge from the data, rather than imposing a predetermined cluster count.
- **Perturbation-based stability scoring** exists because any single clustering run can produce module boundaries that are partly an artifact of algorithmic stochasticity or noise in the input data. Re-clustering under repeated perturbation and tracking how consistently each gene co-clusters with its neighbors separates genes whose module membership is robust from genes whose assignment is provisional - a distinction that matters once module membership is used to support a biological argument.
- **Fisher's exact testing with Benjamini-Hochberg correction** exists because finding a graph community is not the same as showing that community has a coherent, statistically distinct phenotypic identity. Testing each module's HPO term enrichment, with multiple-testing correction across the many terms being scanned, is what turns a graph partition into an interpretable phenotypic signature.

The broader project does not rely on this phenotypic layer in isolation to argue that a candidate gene causes disease. It is one of several independent computational lines of evidence - phenotypic, evolutionary, and functional - that are designed to be combined. A candidate gains support precisely when multiple independent signals converge on it, rather than when any single layer is treated as individually conclusive. That convergence-of-evidence strategy, not any one clustering algorithm, is the methodological core of the broader pipeline this project belongs to.

---

*Part of the [IRD Phenotype Clustering](./README.md) project, in the Evolutionary Genomics & Multi-Omics Portfolio.*
