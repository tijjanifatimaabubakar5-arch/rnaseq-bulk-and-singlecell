# RNA-seq analysis: bulk and single-cell

Oral examination project in RNA-seq / massively parallel sequencing (MPS), University of Skövde (HIS).

## Part A1: Bulk RNA-seq (*Drosophila melanogaster*, KO vs WT)

**Data:** 7 single-end FASTQ files (76 bp, ~25,000 reads each): 5 knock-out (Sample1, 4–7) and 2 wild-type (Sample2–3).
**Reference:** BDGP6.54 chromosome X (Ensembl FASTA) and Ensembl GTF annotation.

| Step | Tool (R) | Result |
|---|---|---|
| Index genome | `Rsubread::buildindex` | chrX index |
| Align reads | `Rsubread::subjunc` (splice-aware) | 100% of reads mapped |
| Count reads per gene | `Rsubread::featureCounts` + GTF | 24 genes with ≥ 10 reads |
| Normalise | `DESeq2` median-of-ratios size factors | 0.98–1.09 |
| Differential expression | `DESeq2` (negative binomial, Wald test, BH) | 3 DEGs at padj < 0.05 |
| Visualise | IGV | Higher coverage on *Amun* in KO |

**Differentially expressed genes (KO vs WT, padj < 0.05)**

| Gene | log2FC | Fold change | padj |
|---|---|---|---|
| *Amun* (FBgn0030328) | +0.56 | 1.47× up | 0.024 |
| FBgn0030326 | +0.61 | 1.53× up | 0.024 |
| FBgn0266421 | −0.34 | 0.79× down | 0.024 |

**QC note:** Sample7's 22,500 reads are the first 22,500 reads of Sample6, so Sample7 is a truncated copy rather than an independent replicate. The KO group therefore has 4 independent samples, and the p-values may be somewhat optimistic.

## Part A2: Single-cell RNA-seq (PBMCs, Seurat)

Pre-normalised Seurat object with the 2,000 most variable genes, then:
`ScaleData` → `RunPCA` → `FindNeighbors(dims = 1:10)` → `FindClusters(resolution = 0.3)` → `RunUMAP(dims = 1:10)`.

| Cluster | Marker | Cell type |
|---|---|---|
| 0, 1 | CD3E | T cells |
| 2 | CD14 | Monocytes |
| 3 | CD19 | B cells |

## Part B: Paper presentation

Wilk AJ et al. *A single-cell atlas of the peripheral immune response in patients with severe COVID-19.* Nature Medicine 26, 1070–1076 (2020). doi:10.1038/s41591-020-0944-y.
Includes a comparison of their scRNA-seq pipeline with the course pipeline, and a discussion of ethics and gender perspectives.

## Repository contents

| File | Description |
|---|---|
| `Analysis parallel seminar.Rmd` | All R code (bulk + single-cell) |
| `Analysis-parallel-seminar.html` | Knitted report with code, output and plots |
| `target.txt` | Sample sheet (sample group, FASTQ, BAM) |
| `RNAseq_Presentation..pdf` | Seminar slides |

Raw data (FASTQ, BAM, genome files, `.rds`) are course material and are not included.

## Tools

R, Rsubread, DESeq2, Seurat, IGV.
