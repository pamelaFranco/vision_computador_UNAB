# Tumor-Masked Structural Connectomics Reveals Tract-Specific White Matter Disruption Associated with Glioma Aggressiveness: An Explainable Machine Learning Pilot Study

> **Note for Reviewers:** This repository hosts the official computational framework and reproducible workflows corresponding to the abstract submitted to the **Nueroradiology**

![Tractografía del Paciente 1](Images/Paciente1wm_dti.fib.gif)

> **Note:** This visualization is a video made in **DSI Studio** to show the global tractography data.

This repository contains the official **explainable machine learning (ML) pipeline** for classifying glioma malignancy grades (Low-Grade Glioma (LGG) vs. High-Grade Glioma (HGG)) and stratifying tract-specific white matter (WM) disruption, using personalized, tumor-masked structural connectivity networks (connectograms).

### Authors & Affiliations

<p align="left">
  <strong>Pamela Franco</strong> <a href="https://orcid.org/0000-0001-7629-3653"><img src="https://img.shields.io/badge/ORCID-0000--0001--7629--3653-A6CE39?logo=orcid&logoColor=white&style=flat-square" height="16"></a><br>
  <small>• Faculty of Engineering, Universidad Andrés Bello, Santiago, Chile.</small>
</p>

<p align="left">
  <strong>Cristian Montalba</strong> <a href="https://orcid.org/0000-0003-3370-0233"><img src="https://img.shields.io/badge/ORCID-0000--0003--3370--0233-A6CE39?logo=orcid&logoColor=white&style=flat-square" height="16"></a><br>
  <small>• Biomedical Imaging Center / Radiology Department, Pontificia Universidad Católica de Chile<br>•Millennium Institute for Intelligent Healthcare Engineering (iHEALTH)</small>
</p>

<p align="left">
  <strong>Ignacio Espinoza</strong> <a href="https://orcid.org/0000-0003-2400-4498"><img src="https://img.shields.io/badge/ORCID-0000--0003--2400--4498-A6CE39?logo=orcid&logoColor=white&style=flat-square" height="16"></a><br>
  <small>• Institute of Physics, Pontificia Universidad Católica de Chile.</small>
</p>

<p align="left">
  <strong>M. Daniela Cornejo</strong> <a href="https://orcid.org/0009-0003-0425-5721"><img src="https://img.shields.io/badge/ORCID-0009--0003--0425--5721-A6CE39?logo=orcid&logoColor=white&style=flat-square" height="16"></a><br>
  <small>• Institute of Physics / Department of Psychiatry, Pontificia Universidad Católica de Chile.</small>
</p>

<p align="left">
  <strong>Francisco Torres</strong> <a href="https://orcid.org/0000-0002-0003-2446"><img src="https://img.shields.io/badge/ORCID-0000--0002--0003--2446-A6CE39?logo=orcid&logoColor=white&style=flat-square" height="16"></a><br>
  <small>• Radiology Department, Hospital Carlos Van Buren</small>
</p>

<p align="left">
  <strong>Carlos Bennett</strong> <a href="https://orcid.org/0009-0007-1434-273X"><img src="https://img.shields.io/badge/ORCID-0009--0007--1434--273X-A6CE39?logo=orcid&logoColor=white&style=flat-square" height="16"></a><br>
  <small>• Neurosurgery Department, Hospital Carlos Van Buren</small>
</p>

<p align="left">
  <strong>Steren Chabert</strong> <a href="https://orcid.org/0000-0002-2890-5077"><img src="https://img.shields.io/badge/ORCID-0000--0002--2890--5077-A6CE39?logo=orcid&logoColor=white&style=flat-square" height="16"></a><br>
  <small>• Biomedical Engineering School / Center MEDING, Universidad de Valparaíso<br>• Millennium Institute for Intelligent Healthcare Engineering (iHEALTH)</small>
</p>

<p align="left">
  <strong>Rodrigo Salas</strong> <a href="https://orcid.org/0000-0002-0350-6811"><img src="https://img.shields.io/badge/ORCID-0000--0002--0350--6811-A6CE39?logo=orcid&logoColor=white&style=flat-square" height="16"></a><br>
  <small>• Biomedical Engineering School / Center MEDING, Universidad de Valparaíso<br>• Millennium Institute for Intelligent Healthcare Engineering (iHEALTH)</small>
</p>

---

The aim of this pilot study was to investigate whether a rigorous, tumor-masked structural connectomic derived from diffusion MRI can capture differences between LGG and HGG and reveal tract-specific patterns of WM network disruption associated with tumor aggressiveness. Additionally, we explored the utility of explainable ML for improving the biological interpretability of these connectomic alterations.


---

## Pipeline Overview

The pipeline implements an advanced neuroimaging and ML workflow:
1. **Diffusion Preprocessing**: Geometry/motion correction via Gaussian Processes and brain masking.
2. **Tumor-Masked Atlas Adaptation**: Voxel-wise intersection and automatic exclusion of tumor-encroached regions from the JHU ICBM-DTI-81 atlas to avoid confounding signals.
3. **Probabilistic Tractography**: Fiber orientation modeling via BEDPOSTX (up to 2 crossing fibers per voxel) and network streamline propagation via ProbtrackX2.
4. **Connectomic Graph Construction & Multi-Level Profiling:** Structural connectivity matrices are mapped using fiber streamline counts between regions of interest (ROIs) derived from the standard JHU ICBM-DTI-81 WM Labels Atlas ($50$ predefined anatomical tracts). These raw adjacency matrices are programmatically analyzed via a specialized automated MATLAB framework using native graph-theoretical functions to construct individual brain graphs and compute a comprehensive topological profile. For each patient, the pipeline extracts parameters across three independent levels: matrix construction descriptors, macro-topological global network metrics, and local nodal/ROI metrics, resulting in a consolidated master dataset ($35$ Patients $\times$ $307$ Columns) for downstream ML pipelines.
5. **Feature Selection & Stratified Validation:** The high-dimensional feature vectors ($p = 307$) are processed through a strict optimization framework. First, zero-variance constant features are programmatically pruned. In Stage 1 (Filtering), a hierarchical clustering routine using average linkage and Euclidean distance maps feature redundancies based on a cophenetic distance threshold of $6.5$. In Stage 2 (Wrapper), a forward SFS loop—driven by an optimized baseline RandomForestClassifier—isolates the absolute minimal, non-redundant optimal subset of graph metrics directly over an internal cross-validation loop. The generalizability of the pipeline is evaluated using a robust 5-Fold Stratified Cross-Validation framework, comparing a Global Feature Selection architecture against a strict Unbiased Isolated setup to mathematically assess and control for selection bias (data leakage). Predictive insights are complemented by game-theoretic tree-based interpretability through SHAP summary mapping and multi-dimensional t-SNE space analysis.
---

## Repository Contents

Given the tractography and structural network focus of this manuscript, the repository structure is organized as follows:


```text
├── Codes/
│   ├── ML_Pipeline.py                                           # Explainable ML pipeline for connectome classification via SFS, Random Forest, and SHAP.
│   ├── compare_date.py                                          # Comparative baseline analysis using logistic regression (age, sex, and tumor hemisphere) to benchmark and validate the superior classification performance of the connectomic pipeline 
│   └── requirements.txt                                         # Required Python packages and dependencies.
├── Dataset/
│   ├──dataset_conectomica_with_labels.csv                       # Multi-level connectomic graph features from JHU atlas (307 features/patient).
│   ├──radiomics_with_classes_cleaned.csv                        # Radiomics features from glioma cohort
│   ├──dataset_conectomica_with_patient_details.csv              # Multi-level connectomic graph features from JHU atlas (307 features/patient) and clinical-demographic information (age, sex, and tumor hemisphere).
├── Results/                                                     # Automatically generated pipeline outputs and diagnostics.
│   ├── clustermap.png                                           # Dual-axis hierarchical correlation matrix with average-linkage cluster boundaries.
│   ├── comparative_roc_curve.png                                # ROC curves contrasting the biased pipeline vs. strictly isolated cross-validation.
│   ├── data_leakage_statistical_analysis.csv                    # Quantified metrics and inflation delta caused by upfront feature selection.
│   ├── decision_boundary_surface.png                            # 2D decision boundary plot showing classification surfaces of the top 2 features.
│   ├── dendogram.png                                            # Hierarchical clustering tree mapping structural feature redundancies (Threshold = 6.5).
│   ├── logistic_regression_pareto_ranking.png                   # Pareto distribution ranking of the top 20 baseline features from L2-Logistic Regression.
│   ├── pipeline_fold_metrics_and_hyperparameters.csv            # Tabular breakdown of cross-validation metrics and best estimator configurations per fold.
│   ├── random_forest_individual_tree.png                        # Visual graph structure/architecture layout of a single decision tree estimator.
│   ├── sfs_feature_accuracy_curve.png                           # SFS trajectory optimization curve mapping internal CV accuracy vs. feature count.
│   ├── shap_1_summary_scatter.png                               # SHAP density scatter plot mapping local attributions within the parsimonious subspace.
│   ├── shap_2_summary_bar.png                                   # Global feature importance ranking calculated via mean absolute SHAP values.
│   ├── shap_3_dependence_1_Clustering_Medial_lemniscus_L.png    # SHAP dependency risk profile for the Medial Lemniscus clustering metric.
│   ├── shap_3_dependence_2_Strength_Sagittal_stratum_L.png      # SHAP dependency risk profile for the Sagittal Stratum strength metric.
│   ├── shap_4_local_patient_force.png                           # SHAP local force plot breaking down additive attributions for a single clinical case.
│   ├── shap_5_decision_trajectory.png                           # SHAP decision plot illustrating feature attribution accumulation paths.
│   ├── tsne_class_segregation_comparison.png                    # Side-by-side t-SNE embedding comparing high-dimensional vs. isolated feature spaces.
│   └── validation_confusion_matrices.png                        # Comparative cumulative confusion matrices for the biased vs. unbiased pipelines.
│   ├── supplementary_analysis_roc.png                           # ROC analysis showing structural connectomics outperforming the clinical-demographic baseline.
└── README.md                                                    # Project documentation and laboratory guidelines.

```

---

## Methods Summary

- **Modalities**: T1-weighted MRI and Diffusion Tensor Imaging (DTI: 25 non-collinear directions, $b = 1000\text{ s/mm}^2$).
- **Subjects**: 35 glioma patients from a single Chilean tertiary center (LGG $58.3$\% and HGG $41.7$\% confirmed via histopathology).
- **Hardware Setup**: Executed via CPU-based parallel processing on a 13th-generation Intel Core i7 architecture (24 GB RAM) and accelerated using an NVIDIA GPU (6 GB VRAM).

## Connectome Construction & Graph Theory

* **Node Definition ($V$):** Remaining non-invaded WM structures ($50$ target regions of interest) are derived from the *JHU ICBM-DTI-81 WM Labels Atlas* after applying dynamic, patient-specific tumor masking to strictly exclude tumor-encroached regions.
* **Streamline Propagation & Edge Definition ($E$):** 5000 probabilistic samples per voxel are generated via FSL's `ProbtrackX2` to construct customized streamline count-based structural connectivity adjacency matrices.
* **Network Metrics & Feature Extraction:** The structural brain graphs are processed via a specialized MATLAB computational framework using native graph functions to extract a comprehensive multi-level profile ($p = 307$ total topological features per patient after removing constant identifiers):
  * *Raw Matrix Construction Descriptors:* Base characteristics including Matrix Size, Max Weights, and Mean Weights.
  * *Global Network Topology:* Total Edge counts, Network Density, Total Weight, and Global Efficiency.
  * *Local Nodal Characterization:* ROI-specific topological metrics including Nodal Strength, Nodal Degree, PageRank, Closeness Centrality, Betweenness Centrality, and Clustering Coefficient calculated individually for each of the $50$ ROIs to map precise spatial WM integrity.

---

## Machine Learning Pipeline Architecture

The ML core is engineered to handle the high-dimensional structural connectivity vectors derived from personalized, tumor-masked brain graphs.

### 1. Feature Selection and Dimensionality Reduction
* **Global Standardization & Data Pruning:** Features derived from the structural connectivity matrices are scaled using a standard Z-score transformation (`StandardScaler`) prior to data splitting. Constant features with zero variance are automatically identified and removed to prevent downstream mathematical instability.
* **Hierarchical Collinearity Mapping:** Features are systematically analyzed using average linkage and Euclidean distance. The pipeline maps and visualizes topographical collinearity and structural redundancy among network edges using a predefined cophenetic distance threshold of $6.5$.
* **Forward Sequential Feature Selection Wrapper:** A wrapper-based stepwise feature selection loop is executed directly within a cross-validation framework over the training split. Driven by a `RandomForestClassifier` (configured with 50 estimators, balanced class weights, and evaluated via 3-fold internal stratified CV), features are added sequentially up to a maximum of 30 candidate features. This process automatically captures the exact parsimonious subset of structural metrics that maximizes internal validation accuracy ($$\arg\max(\mathrm{Accuracy})$$).

### 2. Models Evaluated
* **Linear Exploratory Screening:** `LogisticRegression` with $L_2$ regularization (`solver='liblinear'`) is executed during the initial global exploratory phase to extract absolute log-odds coefficient weights and build a Pareto chart ranking the top 20 baseline components against the target classes.
* **Primary Predictive Ensemble:** `RandomForestClassifier` serves as the core classification model optimized and validated within the CV framework.

### 3. Model Selection, Hyperparameter Tuning & Validation
* **Baseline Grid Optimization:** For the strictly isolated pipeline branch, a hyperparameter optimization framework via `GridSearchCV` is executed inside each outer fold using a 3-fold stratified internal cross-validation split. The grid systematically tunes tree-based hyperparameters for the final classifier, evaluating a comprehensive search space designed for robust regularization: combinations of estimators (`n_estimators: [100, 200, 300]`), maximum tree depth (`max_depth: [None, 3, 5, 10]`), minimum samples required to split an internal node (`min_samples_split: [2, 5, 10]`), minimum samples required at a leaf node (`min_samples_leaf: [1, 2, 4]`), and the number of features to consider when looking for the best split (`max_features: ['sqrt', 'log2']`) based on the `f1_macro` scoring metric.
* **Validation Strategy:** Evaluation is carried out using a **5-Fold Stratified Cross-Validation** framework. Generalizability and evaluation stability are tested across both classification setups to contrast model performance under isolated execution branches against global optimization baselines.
* **Classification Performance Metrics:** Diagnostic robustness is tracked across all folds using Accuracy, Precision, Sensitivity (Recall), and F1-Score (calculated via macro-averaging for robust class-balance control).

### 4. Post-Hoc Data Leakage Analysis
To guarantee the scientific integrity and reproducibility of the results, the pipeline includes a strict post-hoc diagnostic test designed to quantify and detect potential selection bias (*data leakage*).

The test isolates and compares two computational setups across a 5-fold Stratified Cross-Validation scheme to compute the Discrepancy Delta ($\Delta_{\text{leakage}}$) on the Macro F1-Score:

* **Potential Leakage Setup (Biased Framework):** Evaluates the performance of the Random Forest model using the features pre-selected by the global workflow. Since the feature selection was frozen on a static partition before the outer loops, it maps how much the model could benefit from knowing properties of the underlying dataset distribution beforehand.
* **Strict Isolation Setup (Unbiased Framework):** Executes a fully contained pipeline completely from scratch inside each individual cross-validation fold. The tree-importance ranking, internal feature-accuracy cross-validation screening, and hyperparameter tuning grid search are executed strictly on the training partition of the active outer fold, completely hiding the respective validation split.

The pipeline automatically prints out the performance discrepancies. A delta ($\Delta_{\text{leakage}} \le 0.05$) mathematically demonstrates that the feature selection space captures stable, objective structural connectivity biomarkers rather than artificial statistical dependencies, confirming a valid low-dimensional optimized subspace

### 5. Interpretability and Visual Analytics
* **Explainable AI (SHAP Analysis):** Implemented using a specialized tree-based explainer (`TreeExplainer`) applied directly to the optimized model from the first validation fold using an isolated evaluation scale. It quantifies game-theoretic feature contributions, extracting additive feature attribution values (SHAP values) to map how specific structural graph edges drive predictions toward targeted pathological profiles.
* **Feature Space Projections:** High-dimensional transformations are mapped before and after feature selection via t-SNE embeddings to visually confirm class segregation within the optimized low-dimensional subspace. Additionally, a 2D contour mesh scatter plot tracks the empirical decision boundary surface of the top 2 selected features driving the final ensemble's classification boundaries.
* **Baseline Comparison Modeling**: To quantify the incremental predictive value of the structural connectomics workflow, a secondary baseline logistic regression model was trained using exclusively traditional variables: age, sex, and tumor hemisphere. This clinical baseline model underwent matching cross-validation constraints to benchmark whether network-level metrics offer superior discriminative ability over raw demographic and clinical metadata.

---

![Connectomica](Images/Figure1.png)

> **Methodological workflow for glioma stratification and WM vulnerability mapping:** Stage 1: Input Data and preprocessing. Integration of multimodal MRI data. (a) Anatomical T1-weighted imaging and (b) DWI data were acquired, co-registered to perform brain extraction, and utilized to compute DTI maps. (c) Personalized tumor masks are manually segmented to delineate neoplastic boundaries. (d) All structural and diffusion datasets are aligned with the Johns Hopkins University (JHU) WM atlas in MNI space to establish anatomical standardization across the cohort. (e) Spatial normalization to atlas space. Subject-specific images were co-registered to a JHU WM atlas. Stage 2: Tumor-masked tractography. Network node definition and structural connectivity matrix construction. (a) Precise mapping and exclusion of tumor-encroached regions are performed to eliminate confounding infiltrative artifacts. (b) Advanced probabilistics diffusion modeling (BEDPOSTX) evaluates fiber tract configurations, generating a 3D structural connectome. (c) Personalized adjacency matrices are built based on streamline connection probabilities. (d) Multi-level feature extraction yields a high-dimensional profile composed of m = 307 continuous graph-theoretical metrics. Stage 3: Explainable ML strategy. (a) Feature selection using a forward sequential feature selection (SFS). (b) Hyperparameter optimization and classification using a random forest ensemble validate via repeated nested 5-fold stratified cross-validation. (c) Model transparency and localized feature attributions are computed utilizing tree SHAP values to quantify tract-specific WM disruptions.


## Empirical Results & Performance Summary

* **Demographic & Clinical Baseline:** Demographic analysis demonstrated a median cohort age of 43 years, with no significant differences in age or sex between groups. Glioma grade was significantly associated with hemispheric tumor location ($p = 0.038$), showing left-hemisphere predominance in LGG and right-hemisphere predominance in HGG.
* **Feature Redundancy & Selection Trajectory:** Hierarchical clustering of the unreduced feature space ($p = 307$) confirmed substantial information redundancy, partitioning the variables into three highly collinear modules. During Forward SFS expansion within the isolated CV loop, the internal validation trajectory was programmatically tracked to identify the absolute global maximum where accuracy peaks before high-dimensional noise degradation sets in.
* **The Optimal Connectomic Subspace:** The final isolated optimal subset of features capable of discriminating malignancy grades is identified per fold. For post-hoc explanation and feature space projections, the pipeline evaluates the parsimonious sub-space belonging to the first isolation fold.
* **Post-Hoc Data Leakage Validation:** The robust data leakage analysis proved the mathematical necessity of the isolated nested framework, printing out the final performance discrepancies and inflation deltas across all primary metrics (Accuracy, Precision, Recall, and F1-score) to quantify the exact selection bias induced by upfront feature spaces.
* **Superiority Over Clinical Baselines**: While the comparative clinical baseline model (age, sex, and tumor hemisphere) achieved a strong classification performance ($\text{Mean AUC} = 0.82$), the pure structural connectomics pipeline substantially outperformed it, yielding a $\text{Mean AUC} = 0.92$. Notably, integrating clinical variables into the network features (Combined Model) did not produce any further diagnostic improvement ($\text{Mean AUC} = 0.92$). This ceiling effect mathematically demonstrates that tumor-masked structural connectograms capture high-dimensional pathophysiological signatures that inherently absorb and outperform traditional epidemiological variables.
* **Game-Theoretic Interpretability:** The implementation of tree-based SHAP values dismantled the algorithmic "black box" by computing a comprehensive evaluation framework: (1) SHAP Summary Scatter plots for global density mapping, (2) Global Feature Importance Bar charts, (3) Automated Dependence Trajectory plots for the top 2 predictive features, (4) Single-sample Local Force plots for individual patient profiling, and (5) Decision Plots to map feature attribution accumulation paths.
---

## Reproducibility & Data Availability

- **Code**: The ML pipelines script is hosted in this repository.
- **Data Privacy**: MRI datasets and structural connectivity matrices generated during this study are not publicly available due to patient data privacy restrictions imposed by the Ethics Committee of the Servicio de Salud Valparaíso San Antonio (ORD.001413).
- **Clinical Inquiries**: Anonymized data may be made available upon reasonable request and subject to institutional approval by contacting **Steren Chabert (steren.chabert@uv.cl)**.

---

## How to Run

1. **Clone the repository**:
   ```bash
   git clone [https://github.com/pamelaFranco/glioma-ml-tractography.git](https://github.com/pamelaFranco/glioma-ml-tractography.git)
   cd glioma-ml-tractography
   ```

2. **Set up neuroimaging environment**: Ensure you have FSL (v6.0) installed and configured in your environment path.

3. **Install Python dependencies**:
   ```bash
   pip install -r requirements.txt
   ```
4. Execute the connectomics pipeline:  
   ```bash
   # Run machine learning pipeline
   python ML_pipeline.py
   ```
  
---

## Acknowledgements
This work was supported by the National Agency for Research and Development (ANID) of Chile through:

* FONDECYT N°1221938 ("An Explainable Deep Neuro-Fuzzy Inference System for the segmentation of BT in multi-contrast magnetic resonance imaging").

* ANID Millennium Science Initiative Program ICN2021_004 (Millennium Institute for Intelligent Healthcare Engineering - iHEALTH).

* Additionally, this work was funded by the Endowment I+D in Health Competition of the Universidad Andrés Bello (UNAB) 2025, project No. DI-07-25/ICS.

---
## Citation

If you find this pipeline useful for your research, please cite our preliminary work prepared for **Neuroradiology**:

```bibtex
@article{franco2026quantifying,
  title={Tumor-Masked Structural Connectomics Reveals Tract-Specific White Matter Disruption Associated with Glioma Aggressiveness: An Explainable Machine Learning Pilot Study},
  author={Franco, Pamela and Montalba, Cristian and Espinoza, Ignacio and Cornejo, M. Daniela and Torres, Francisco and Bennett, Carlos and Chabert, Steren and Salas, Rodrigo},
  journal={Neuroradiology},
  year={2026},
  note={Submitted for publication / Under review}
}
```

---

## License

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)