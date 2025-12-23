# scDECIPHER
single-cell dynamic explorer of complex interactions and pathway hierarchies
<br />
<br />
## The scDECIPHER consists of the following steps: 

**Standard Preprocessing Workflow and Basic Visualization**
<br />
+ Quality Control: Single-cell data is assessed for sequencing depth, number of detected genes, and mitochondrial gene percentage. The dataset is then normalized and scaled for downstream analysis.
+ Feautre Selection & Clustering: Highly variable features are selected. Dimensionality reduction and clustering are performed typically using UMAP for visualization and Louvain for clustering.



**Dynamic Trajectory Inference**
<br />
+ Clusters are labeld with predefined phenotype markers.
+ Biologically meaningful dynamic trajectories are inferred using tools such as [Slingshot](https://github.com/kstreet13/slingshot), [Monocle3](https://github.com/cole-trapnell-lab/monocle3), Monocle2, PAGA, CytoTRACE, scVelocity, and others.
+ This step supuports a variety of algorithms for flexiblity in identifying cell state transitions.



**Gene Selection for Network Construction**
<br />
+ Node Selection: Genes (nodes) are selected for network construction based on the differential gene expression analysis or regulon specificy score, which quantifies the specificity of regulon activity in each cluster compared to the rest.
+ Trajectory-based Selection: Within inferred trajectories, top-ranked genes from the start and end clusters are identified as network nodes according to their expression lecels.



**Binarization of Gene Expression and Regulation Dynamic Inference**
<br />
+ Imputation & Interaciton Assignment:
  + In this step, we used the [IQCELL pipeline](https://gitlab.com/stemcellbioengineering/iqcell) with fine-tuned parameters.
  + The MAGIC algorithm is used to impute dropout events in the single-cell RNA-seq data.
  + Possible interacitons between genes are generated and assigned as activate or inhibitory regulatory signs using Pearson correlation.
+ Expression Binarization:
  + Each gene expression is binarized (ON/OFF) across the trajectory using k-means clustering to define binarization thresholds (x-axis: pseudotime, y-axis: gene).
+ Boolean Function Inference:
  + Using the binarized data and inferred network structure, Boolean functions that best match observed gene expression patterns (exhibiting high agreement levels) are inferred using the z-solver.
  + For this, please install the following package:
    + Install IQCELL from GitLab repository: https://gitlab.com/stemcellbioengineering/iqcell



**Construction of Boolean Network Ensembles**
<br />
+ Boolean Function Selection:
  + For each node, all possible Boolean functions matching the highest agreement are selected.
  + The top three functions are then chosen using the average mutual information value between regulators and target nodes, constrained to nested canalizing functions for biological plausibility.
+ Network Generation:
  + From the selected Boolean functions, 1,000 Boolean networks are generated randomly.
+ Sensitivity Analysis:
  + Networks are evaluated by the average sensitivity of each node.
  + The most biologically plausible network is regarded as a network having a mean close to 1 as representative.
+ Network Ensemble:
  + Unique Boolean network models that meet all critera are combined to form the final network ensemble.
  + For this, please install the following packages:
    + Install pyboolnet from GitHub repository: https://github.com/hklarner/pyboolnet
    + Embed BF_codes folder from GitHub repository: https://github.com/asamallab/MCBF


## Environment Setup
All required dependencies are listed in the `script/` folder.  
We recommend using individual virtual environment (Conda or venv) to avoid version conflicts. To set up the environment using Docker, you can use the following configuration in your Dockerfile:  

```dockerfile
# Create env. w/ package dependenceis for Example_1, Example_2

COPY scanpyEnvironment.yml /tmp/
RUN pip install --upgrade pip && \
	conda env create --file /tmp/scanpyEnvironment.yml && \
	rm /tmp/scanpyEnvironment.yml
COPY pyscenicEnvironment.yml /tmp/
RUN pip install --upgrade pip && \
	conda env create --file /tmp/pyscenicEnvironment.yml && \
	rm /tmp/pyscenicEnvironment.yml
```



```dockerfile
# Create env. w/ package dependenceis for Example_3

COPY requirements.txt /tmp/
RUN pip install --no-cache-dir -r /tmp/requirements.txt
```


## Additional Information
The MObyDiCK beta service features scDECIPHER as its modeling analysis module.  
Link to the MObyDiCK beta service: mobydick@biorevert.com  


