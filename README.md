# scDECIPHER
single-cell dynamic explorer of complex interactions and pathway hierarchies

**Standard Preprocessing Workflow and Basic Visualization**
(1) Quality Control:
Single-cell data is assessed for sequencing depth, number of detected genes, and mitochondrial gene percentage. The dataset is then normalized and scaled for downstream analysis.
(2) Feautre Selection & Clustering:
Highly variable features are selected. Dimensionality reduction and clustering are performed typically using UMAP for visualization and Louvain for clustering.


**Dynamic Trajectory Inference**
Clusters are labeld with predefined phenotype markers. Biologically meaningful dynamic trajectories are inferred using tools such as Slingshot, Monocle3, Monocle2, PAGA, CytoTRACE, scVelocity, and others.
This step supuports a variety of algorithms for flexiblity in identifying cell state transitions.


**Gene Selection for Network Construction**
(1) Node Selection:
Genes (nodes) are selected for network construction based on the differential gene expression analysis or regulon specificy score, which quantifies the specificity of regulon activity in each cluster compared to the rest.
(2) Trajectory-based Selection:
Within inferred trajectories, top-ranked genes from the start and end clusters are identified as network nodes according to their expression lecels.


**Binarization of Gene Expression and Regulation Dynamics Inference**
(1) Imputation & Interaciton Assignment:
In this step, we used the IQCELL pipeline[https://gitlab.com/stemcellbioengineering/iqcell] after fine-tuning the parameters.
The MAGIC algorithm is used to impute dropout events in the single-cell RNA-seq data.
Possible interacitons between genes are generated and assigned as activate or inhibitory regulatory signs using Pearson correlation.
(2) Expression Binarization:
Each gene expression is binarized (ON/OFF) across the trajectory using k-means clustering to define binarization thresholds (x-axis: pseudotime, y-axis: gene).
(3) Boolean Function Inference:
Using the binarized data and inferred network structure, Boolean functions that best match observed gene expression patterns (exhibiting high agreement levels) are inferred using the z-solver.

Fo this, please install the following package:
Install IQCELL from GitLab repository: https://gitlab.com/stemcellbioengineering/iqcell


**Construction of Boolean Network Ensembles**
(1) Boolean Function Selection:
For each node, all possible Boolean functions matching the highest agreement are selected.
The top three functions are then chosen using the average mutual information value between regulators and target nodes, constrained to nested canalizing functions for biological plausibility.
(2) Network Generation:
From the selected Boolean functions, 1,000 Boolean networks are generated randomly.
(3) Sensitivity Analysis:
Networks are evaluated by the average sensitivity of each node.
The most biologically plausible network is regarded as a network having a mean closet to 1 as representative.
(4) Network Ensemble:
Unique Boolean network models that meet all critera are combined to form the final network ensemble.

For this, please install the following packages:
Install pyboolnet from GitHub repository: https://github.com/hklarner/pyboolnet
Embed BF_codes folder from GitHub repository: https://github.com/asamallab/MCBF
