
# PCA-Based Gene Selection Using GSE6613

An exploratory gene-expression analysis project implementing **Principal Component Analysis (PCA)-based gene selection** using the GSE6613 dataset from the Gene Expression Omnibus (GEO). The project ranks genes according to their contributions to principal components and compares gene-selection results obtained using the top 5 and top 10 principal components.

## Overview

Gene-expression datasets often contain thousands of genes, making it challenging to identify the features that contribute most to variation across samples. PCA is an unsupervised dimensionality-reduction technique that transforms correlated features into principal components representing major directions of variation in the data.

This project explores how PCA loadings, weighted by explained variance, can be used to rank genes and how the selected gene lists change when different numbers of principal components are considered.

## Dataset

- **Dataset:** GSE6613
- **Source:** [NCBI Gene Expression Omnibus (GEO)](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE6613)
- **Data type:** Whole-blood gene-expression data
- **Platform:** Affymetrix Human Genome U133A Array (GPL96)
- **Reported cohort:** 50 patients with Parkinson's disease, 33 patients with other neurodegenerative diseases, and 23 healthy controls.

The notebook retrieves the dataset programmatically using `GEOparse` and maps probe identifiers to gene symbols using platform annotation.

**Important:** The current implementation performs unsupervised gene selection on the loaded expression matrix. It does not explicitly restrict the analysis to Parkinson's disease and healthy-control samples. Therefore, the resulting gene rankings should not be interpreted as Parkinson's disease-specific markers.

## Methodology

The notebook follows these steps:

1. **Data Acquisition:** Download the GSE6613 dataset from GEO using `GEOparse`.
2. **Expression Matrix Construction:** Extract expression values and organize the matrix so that samples are rows and genes are features.
3. **Gene Annotation:** Map probe identifiers to gene symbols using GPL96 platform annotations.
4. **Feature Standardization:** Standardize gene-expression features using `StandardScaler`.
5. **Principal Component Analysis:** Apply PCA, retaining enough components to explain at least 95% of the total variance.
6. **PCA-Based Gene Ranking:** Rank genes using their PCA loadings weighted by the explained variance of the selected components.
7. **Gene Selection Comparison:** Select the top 20 genes using the top 5 and top 10 principal components.
8. **Overlap Analysis:** Compare the selected gene lists using shared genes, list-specific genes, overlap percentage, and Jaccard similarity.
9. **Visualization:** Plot explained variance and the weighted scores of selected genes.

## Gene Selection Approach

For each gene, the notebook aggregates its contributions across the selected principal components, weighting each component by its explained variance.

The score is expressed as:

$$
S_g = \sum_{k=1}^{K} w_k |l_{gk}|
$$

where:

- $S_g$ is the score assigned to gene $g$.
- $K$ is the number of principal components considered.
- $w_k$ is the explained-variance weight of component $k$.
- $l_{gk}$ is the PCA loading of gene $g$ on component $k$.

Genes with higher scores are ranked more highly by this approach.

### Configurations Compared

| Configuration | Description |
|---------------|-------------|
| **Top 5 PCs** | Ranks genes using the first five principal components. |
| **Top 10 PCs** | Ranks genes using the first ten principal components. |

The comparison illustrates how the number of principal components can influence gene rankings and selection stability.

## Technologies Used

- **Programming:** Python
- **Data Processing:** NumPy, pandas
- **Data Acquisition:** GEOparse
- **Machine Learning:** scikit-learn
- **Visualization:** Matplotlib, Seaborn
- **Development Environment:** Jupyter Notebook, Google Colab

## Installation and Usage

### 1. Clone the Repository

```bash
git clone https://github.com/sadiaruhama/PCA-Based-Gene-Selection-Parkinsons-Disease.git
cd PCA-Based-Gene-Selection-Parkinsons-Disease
```

### 2. Install Dependencies

```bash
pip install numpy pandas matplotlib seaborn scikit-learn GEOparse
```

### 3. Run the Notebook

Open the notebook in Jupyter:

```bash
jupyter notebook
```

Alternatively, upload the notebook to Google Colab and run the cells sequentially.

**Note:** An internet connection is required to download the dataset from GEO.

## Key Questions Explored

- How can PCA loadings be used to rank gene-expression features?
- How does the number of principal components affect the selected gene list?
- How much overlap exists between the top 20 genes selected using five versus ten principal components?
- What are the strengths and limitations of unsupervised, variance-based gene selection?

## Limitations

- **Unsupervised Selection:** PCA captures major sources of variation, which may not correspond to differences between disease and control groups.
- **Cohort Composition:** The dataset includes multiple diagnostic groups, and the current implementation does not explicitly filter to a Parkinson's disease versus healthy-control comparison.
- **Component Selection:** Gene rankings may change depending on the number of principal components considered.
- **Biological Interpretation:** High PCA scores do not independently establish disease relevance or biomarker validity.
- **Validation:** The workflow does not establish diagnostic performance, statistical significance of disease-associated expression, or independent replication of selected genes.

## Future Work

Potential extensions include:

1. Explicit Parkinson's disease versus healthy-control comparisons.
2. Differential-expression analysis with multiple-testing correction.
3. Comparison with supervised feature-selection methods.
4. Independent dataset validation.
5. Functional enrichment analysis of selected genes.

## Intended Use

This project is intended for **exploratory gene-expression analysis and learning PCA-based feature-selection methods**.

The results should not be treated as clinically validated biomarkers or as evidence that the selected genes cause or specifically indicate Parkinson's disease.


