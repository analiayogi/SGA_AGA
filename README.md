# SGA_AGA

R Pipeline for the paired RNA-seq analysis of human umbilical cord mesenchymal stem cells (hUCMSCs) and derived adipogenic cultures.

Samples: Umbilical cord tissues from birth weight discordant monozygotic diamniotic twins (at least 20%; SGA-AGA co-twins) were collected for this study.

## Reference 

Analia Yogi, Yusuke Noguchi, Shizuka Kirino, Haruki Yamano, Eriko Adachi, Hisae Nakatani, Yoko Saito, Chikako Morioka, Mari Hayata, Manabu Sugie, Kei Takasawa, Tomohiro Morio, Masatoshi Takagi, Atsumi Tsuji-Hosokawa, Kenichi Kashimada.

"Fetal environment associated with small for gestational age birth transcriptionally primes umbilical cord mesenchymal stem cells toward osteogenic pathways with potentially impaired adipogenic differentiation". Placenta (Elsevier), 2026.

https://doi.org/10.1016/j.placenta.2026.09.009

## Data

Raw RNA-seq data are publicly available at the DNA Data Bank of Japan (DDBJ). The accession number is PRJDB40126.

The raw counts matrices used as input for the published analysis are available at this repository (one per folder).

## Repository structure

- "UCMSC": Includes RNA-seq raw counts matrix from hUCMSCs and the pipeline for the analysis in R: differential expression, GSEA, and leading edge analysis. Also includes the published plots.

- "adipogenic_culture": Includes the RNA-seq raw count matrix collected from hUCMSC that underwent adipogenic differentiation until day 21. On that day, total RNA extraction was performed.
The pipeline used for the following analysis in R is included: differential expression, GSEA, and leading edge analysis. The scripts used for plots preparation are included.

## Sample naming

Columns names uses a condition prefix: S= SGA twin and B= AGA co-twin. Twin pairs were renumbered for the figure presentation in the article. Supplementary Table 1 includes the tracking of such modifications.

For adipogenic differentiation, samples from one pair twin were not available.

## Analysis pipeline

Each folder contains the corresponding raw counts matrix and R pipeline

## Requirements

The analysis was run in R (v4.4.1) with Bioconductor 3.19. Packages versions used for the published analysis:

| Package | Version | Used for|
|---|---|---|
| DESeq2 | 1.44.0 | Normalization and paired differential expression |
| fgsea | 1.30.0 | Gene set enrichment analysis |
| msigdrb | 7.5.1 | GO:BP gene sets (MSigDB v7.5.1) |
| biomaRT | 2.60.1 | Ensembl to ID gene symbol annotation |
| ComplexHeatmap | 2.20.2 | Leading edge heatmaps |
| circlize | 0.4.17 | Heatmap color scales |
| EnchancedVolcano | 1.22.0 | Volcano plots |
| plotly | 4.10.4 | 3D PCA |
| htmlwidgets | 1.6.4 Save 3D PCA as HTML |
| ggplot2 | 3.5.2 | Bar plots and gene expression plots |
| patchwork | 1.3.0 | Figure panles (adipogenic script) |
| dplyr | 1.1.4 | Data handlong |

Installation
```r
install.packages(c("BiocManager", "remotes", "dplyr", "ggplot2", "plotly", "htmlwidgets",
                   "patchwork", "circlize", "pheatmap", "RColorBrewer", "stringr",
                   "gridExtra", "cowplot", "extrafont"))
BiocManager::install(c("DESeq2", "biomaRt", "fgsea", "EnhancedVolcano", "ComplexHeatmap"))
remotes::install_version("msigdbr", version = "7.5.1")
```

## Notes
- Internet connection is required for the gene annotation steps ('biomaRT' queries Ensembl)
- Results may differ slightly if other package versions are used.
- Input count matrices were obtained from the rwa FASTQ files with fastp (v0.23.4), STAR (v2.7.11b; GRCh38, Ensembl release 111)and featureCounts (v2.0.1), as described in the article.

## Contact

Analia Yogi: ayogi.ped@tmd.ac.jp
