# Clustering and Visualization of scRNA‑seq Data Using Path Metrics

This repository contains a course project for the *Introduction to Bioinformatics* class, for the Fall 2025 term, implementing and extending the method from the paper **“Clustering and visualization of single-cell RNAseq data using path metrics” (PLOS Computational Biology, 2024)**.

## Project Overview

The project focuses on applying the **single-cell Path Metrics Profiling (scPMP)** method to cluster and visualize high-dimensional single-cell RNA-seq (scRNA-seq) data. The main idea is to use power-weighted path metrics on a cell graph to define distances that conserve both local cluster structure and global data geometry. After that, it contains low-dimensional embedding and clustering.

Main goals are:

- Define a distance metric that balances data geometry and density using a tunable parameter (p).
- Perform dimensionality reduction to a low-dimensional, visually interpretable embedding that preserves these distances.
- Cluster the embedded cells and evaluate cluster quality and geometric fidelity on some famous scRNA-seq datasets.

## Methods

The typical pipeline implemented in this project is:

1. **Preprocessing and denoising**  
   - Start from a gene expression matrix \(X \in \mathbb{R}^{n \times d}\) (n cells, d genes), which is often high-dimensional, sparse, and noisy.
   - Apply standard scRNA-seq preprocessing (e.g., filtering low-quality cells/genes, normalization, basic noise reduction).

2. **Graph construction**  
   - Build a k-nearest-neighbor graph over cells using an appropriate similarity measure.
   - Assign edge weights and construct a weighted graph representation of the data.

3. **Path metric computation (scPMP)**  
   - Compute power-weighted path metrics between cells, with parameter \(p\) controlling the trade-off between local geometry (short paths) and density (longer paths through dense regions).
   - Distances are computed via shortest paths (e.g., Dijkstra’s algorithm) on the weighted graph.

4. **Dimensionality reduction**  
   - Apply **multidimensional scaling (MDS)** or a similar technique on the path distance matrix to obtain a low-dimensional embedding \(Y \in \mathbb{R}^{n \times r}\) (typically \(r = 2\) or \(3\)).

5. **Clustering and evaluation**  
   - Cluster the embedded cells into \(k\) clusters (e.g., k-means or similar methods).
   - Evaluate performance using metrics such as **Adjusted Rand Index (ARI)**, **geometric perturbation \(\pi\)**, **Entropy of Cluster Purity (ECP)**, and **Entropy of Cluster Accuracy (ECA)** on several benchmark datasets.

## Datasets [file:1]

The project uses publicly available benchmark scRNA-seq datasets, including both real and simulated data.

- **RNAmix1, RNAmix2** – Simulated RNA mixture benchmarks with human cancer cell lines.
- **CellMix (CellBench)** – Real human lung cancer cell line mixtures.
- **Baron’s Pancreas** – Real human pancreas tissue.
- **Tabula Muris Lung (TM Lung)** – Real mouse lung tissue.
- **Tabula Muris Pancreas (TM Panc)** – Real mouse pancreas tissue.
- **PBMC4K** – Human peripheral blood mononuclear cells (largest dataset in the project).  
- **Simulated Beta (SimBeta)** – Simulated beta cells based on real pancreas data.

Each dataset is characterized by:

- Number of cells \(n\)  
- Number of genes \(d\)  
- Number of clusters / cell types \(k\)

## Planned Extensions 

This project goes beyond a direct reimplementation of scPMP and explores several **extensions**.

### Automatic selection of \(p\) 

- In the original work, \(p\) is chosen manually and \(p = 2\) is recommended as a generally good trade-off, but this is not optimal for all datasets (e.g., PBMC4K performs best with \(p = 4\)).
- The project aims to make the choice of \(p\) data-driven by extracting features such as **elongation**, **sparsity**, or noise level and mapping them to an appropriate \(p\).[file:1]

### Towards semi-supervision

- A data-driven choice of \(p\) can be viewed as a step toward a more semi-supervised or adaptive version of scPMP, aligned with future-work suggestions from the original authors.

### Alternative graph construction via Visibility Graphs

- The project considers replacing the usual k-nearest-neighbor graph with a **visibility graph** (including weighted visibility graphs) for constructing the underlying cell graph.
- Visibility graphs have been successfully used in biomedical signal analysis (e.g., cardiac and EEG/SSVEP signals), and the goal is to investigate their effect on clustering accuracy and geometric fidelity in scRNA-seq.

These ideas are exploratory and may be refined, extended, or replaced based on experimental results and further reading.

## Repository Structure (Suggested)

Adjust this section to match the actual repository layout.

- `data/` – Download scripts or links for benchmark scRNA-seq datasets.
- `src/` – Core implementation: preprocessing, graph construction, path metric computation, embedding, clustering, and evaluation.
- `notebooks/` – Jupyter notebooks for experiments and visualizations.
- `results/` – Saved embeddings, clustering labels, and evaluation metrics (ARI, ECP, ECA, geometric perturbation).
- `plots/` – Plots of low-dimensional embeddings and clustering results.
- `docs/` – Additional course-related documentation and project report material.

## Getting Started

### Clone the repository

```bash
git clone https://github.com/Sepehr2001/Bioinformatics-course-project-2025.git
cd Bioinformatics-course-project-2025
