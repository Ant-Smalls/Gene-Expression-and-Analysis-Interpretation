# TCGA Breast Cancer (BRCA) Prognostic Signature: Impact of ERBB2 Amplification

## Project Overview
This repository contains the R notebook and analysis files used to investigate the consequences of $\texttt{ERBB2}$ (HER2) gene amplification in the TCGA Pan-Cancer Atlas breast carcinoma data set.

The analysis identifies genes and biological pathways significantly altered by $\texttt{ERBB2}$ amplification and constructs a minimal gene signature for predicting Overall Survival (OS) risk.

---

## Methods and Workflow

The analysis follows a comprehensive pipeline, moving from raw RNA-seq counts to functional pathway interpretation and survival modeling.

### 1. Data Processing and Cohort Definition
* **Data Source:** Breast Invasive Carcinoma (BRCA) RNA-seq, Copy Number Alteration (CNA), and clinical data obtained from the TCGA Pan-Cancer Atlas 2018 study.
* **Preprocessing:** Raw counts were filtered for low expression and normalized using the size-factor estimation from the **DESeq2** package (Love *et al.*, 2014).

### 2. Differential Expression (DE) Analysis
* **Model:** Differential expression was calculated using the **DESeq2** package (Love *et al.*, 2014) comparing Amplified vs. Non-Amplified status.
* **Significance:** Genes were deemed differentially expressed if the **Adjusted $p$-value ($p_{adj}$) was $< 0.05$.**
* **Gene Ranking:** The top genes were ranked by the absolute magnitude of the $\mathbf{\text{log}_2\text{Fold Change}}$ ($|\text{LFC}|$) to prioritize genes with the strongest biological effects.

### 3. Functional Enrichment Analysis
The DE gene lists were split into up-regulated ($\text{LFC} > 0$) and down-regulated ($\text{LFC} < 0$) sets for enrichment using the **clusterProfiler** package (Xu *et al.*, 2024; Yu *et al.*, 2012).

* **Gene Ontology (GO):** Used to identify broad biological processes.
* **KEGG/Reactome:** Used to identify specific molecular pathways.

### 4. Prognostic Survival Modeling
* **Input Data:** Variance Stabilized Transformed ($\texttt{vst}$) expression values of all significant DE genes.
* **Model:** Overall Survival (OS) was modeled using cross-validated Lasso regression from the **glmnet** package (Friedman *et al.*, 2010). The **survival** package (Therneau, 2024) was used for data preparation.
* **Output:** A prognostic index (PI) defined by the non-zero coefficients selected by the model.

---