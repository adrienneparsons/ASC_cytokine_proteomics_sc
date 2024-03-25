# ASC_cytokine_proteomics_sc
Single cell RNA sequencing and bulk proteomics analysis of ASCs in response to inflammation, accompanying Parsons et al. 2024

This repository contains a script and files used to analyze single cell and bulk proteomics data of unmodified adipose stromal vascular fraction and cultured adipose-derived stem/stromal cells to better characterize in the ASC response to inflammatory stimuli in vitro. The integration of bulk proteomics and single cell RNA sequencing also allows us to gain novel insights in how in vitro treatments translate to in vivo biology.

## Set up for script
Human ASCs cultured with and without inflammatory cytokines and unmodified human SVF cells were assayed for single cell RNA sequencing using the 10x Genomics platform. The resulting sequencing data was aligned and pre-processed using CellRanger software, and the resulting barcode, matrix, and feature files were saved for reading in to the script. Concurrently, Human ASCs were cultured in the same inflammatory cytokines and then were assayed for proteomics in bulk using LC-MS/MS. Proteins and their relative abundances were identified and the data were exported into an excel spreadsheet.

## Running the script
### Single cell RNA sequencing analysis
The matrix, barcode, and feature files for two SVF samples, a control cultured ASC sample, and an ASC sample cultured in inflammatory cytokines are read into R and individual Seurat objects are generated. Following object subsetting using typical QC metrics (percent mitochondrial RNA and nFeatures), the standrd Seurat v4 pipeline is used to generate UMAPs for each sample. Effects of cell cycle genes are regressed out from the merged object containing both cultured samples. 

Following this initial UMAP generation and QC checking, cell types are annotated based on the results of the unssupervised clustering and dimensionality reduction. First, broad cell type markers from Liu et al., Nature 2022 are used to categorize cell types in a single SVF sample at low granularity, then additional marker genes are identified to functionally annotate these broad cell types into more granular subtypes. These cell type annotations are then mapped onto a second SVF sample using Seurat TransferData to show concurrence in annotations across donors.

![alt text](/Figure1.pdf "SVF single cell analysis")

The cultured, control and cytokine-stimulated Seurat object had less well defined cell types, so an initial optimization of clustering resolution is performed on that object. 

Following cell type annotation, dot plots are generated to show expression of marker genes for each cell type in SVF cells and FindMarkers is used to identify the functional subtypes of ASCs in the cultured Seurat object. A dot plot is then made to describe marker gene expression for the clusters in the cultured ASC object. Violin plots showing cluster-specific marker genes are also generated.

Monocle3 is used to identify a trajectory and pseudotime projection on the cultured object.

![alt text](/Figure2.pdf "ASC single cell analysis")

### Single cell RNA sequencing and bulk proteomic concurrence testing
Following this single cell RNA sequencing pipeline, the proteomics data is read in and protein names are substituted for gene names. Following some formatting, the correlatiion between bulk proteomics data in control and cytokine-stimulated cultured samples and single-cell pseudobulk data in control and cytokine-stimulated cells is calculated. Differential gene expression analysis (control vs. stimulated) and differential protein abundance analysis (control vs. stimulated) are performed, and the relationship between differentially abundant genes/proteins is idenitfied, plotted, and functionally interrogated uusing GSEA. 

![alt text](/Figure4.pdf "Concurrence testing")

### Protemic profiling
Next, analysis using just the bulk proteomics of 10 human donor ASC samples (5 donors, control and cytokine-stimulated) is performed. PCA plotting shows separation based on presence or absence of cytokine treatment. Differential protein analysis is plotted as a volcano plot. The heatmaps showing all proteins and differentially abundant proteins are made.

![alt text](/Figure3.pdf "Proteomic profiling")

### Scissor integration of single-cell RNA sequencing SVF data and bulk proteomics ASC data
Scissor integration (Sun et al, Nat. Biotechnol. 2022) is used to identify a subset of SVF cells that are phenotypically similar to ASCs that are cultured swith inflammatory cytokines in vitro. ASCs deemed Scissor-Postive and Scissor-Negative are compared using differential gene expression analysis and from that list, a phenotype of 6 differentially expressed genes encoding surface proteins is identified.

![alt text](/Figure5.pdf "Scissor")
