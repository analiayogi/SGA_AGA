# SGA_AGA

R Pipelines for the paired RNA-seq analysis of human umbilical cord mesenchymal stem cells (hUCMSCs) and derived adipogenic cultures.

Samples: hUCMSCs were established from six monozygotic, monochorionic-diamniotic twin pairs with at least 20% weight discordance. In each pair, the larger twin was classified  appropriate for gestational age (AGA) and the smaller was small for gestational age (SGA). Five of the six pair samples were available for adipogenic differentiation.

## Reference 

Analia Yogi, Yusuke Noguchi, Shizuka Kirino, Haruki Yamano, Eriko Adachi, Hisae Nakatani, Yoko Saito, Chikako Morioka, Mari Hayata, Manabu Sugie, Kei Takasawa, Tomohiro Morio, Masatoshi Takagi, Atsumi Tsuji-Hosokawa, Kenichi Kashimada.

"Fetal environment associated with small for gestational age birth transcriptionally primes umbilical cord mesenchymal stem cells toward osteogenic pathways with potentially impaired adipogenic differentiation". Placenta (Elsevier), 2026.

https://doi.org/10.1016/j.placenta.2026.09.009

## Data

Raw RNA-seq data are publicly available at the DNA Data Bank of Japan (DDBJ). The accession number is PRJDB40126.

The raw counts matrices used as input for the published analysis are available in this repository (one per folder), and were obtained from the raw FASTQ files with fastp (v0.23.4), STAR (v2.7.11b; GRCh38, Ensembl release 111), and featureCounts (v2.0.1), as described in the article.

## Repository structure

- "UCMSC": Includes RNA-seq raw counts matrix from hUCMSCs (six twin pairs), and the R pipeline (differential expression, GSEA, leading-edge analysis, and plots shown in figure 2) 

- "adipogenic_culture": Includes the RNA-seq raw count matrix collected from hUCMSC after 21 days of adipogenic differentiation (five twin pairs).
The R pipeline used for the analysis (differential expression, GSEA, and leading edge analysis) and plots shown in Figure 3B-E and Figure 4, is included.

## Sample naming

Column names use a condition prefix: S= SGA twin and B= AGA co-twin. Sample identifiers were assigned during sequencing and renumbered for figure presentation; the correspondence between original and renumbered identifiers, together with the DDBJ BioSample accessions, is provided in Supplementary Table 1 of the article.

For adipogenic differentiation, samples from one twin pair were not available.

## Analysis pipeline

Each folder contains the corresponding raw counts matrix and R pipeline. Steps:

1. Gene annotation with biomaRt, and filtering lowly expressed genes (at least 10 raw counts in at least 5 samples).
2. Paired differential expression with DESeq2 (`design= ~ twin_pair + condition; SGA vs AGA`).
3. 3D PCA on variance-stabilized counts and volcano plot (nominal *P* < 0.05 and |log2FC| > 1, for exploratory purposes).
4. GSEA with fgsea on GO:BP gene sets (15-500 genes, FDR < 0.05), and leading-edge heatmap of the top positively enriched gene set.
5. Only in the "adipogenic_culture" script: paired expression plots for *DLK1*, *PPARG*, and *FABP4* (nominal *P* value).

## Requirements

The analysis was run in R (v4.4.1) with Bioconductor 3.19. Package versions used for the published analysis:

| Package | Version | Used for|
|---|---|---|
| DESeq2 | 1.44.0 | Normalization and paired differential expression |
| fgsea | 1.30.0 | Gene set enrichment analysis |
| msigdbr | 25.1.1 | GO:BP gene sets (MSigDB 2025.1.Hs) |
| biomaRt | 2.60.1 | Ensembl ID to gene symbol annotation |
| ComplexHeatmap | 2.20.0 | Leading edge heatmaps |
| circlize | 0.4.17 | Heatmap color scales |
| EnhancedVolcano | 1.22.0 | Volcano plots |
| plotly | 4.10.4 | 3D PCA |
| htmlwidgets | 1.6.4 | Save 3D PCA as HTML |
| ggplot2 | 3.5.2 | Bar plots and gene expression plots |
| patchwork | 1.3.0 | Figure panels (adipogenic script) |
| dplyr | 1.1.4 | Data handling |

Installation:

```r
install.packages(c("BiocManager", "remotes", "dplyr", "ggplot2", "plotly", "htmlwidgets",
                   "patchwork", "circlize", "pheatmap", "RColorBrewer", "stringr",
                   "gridExtra", "cowplot", "extrafont"))
BiocManager::install(c("DESeq2", "biomaRt", "fgsea", "EnhancedVolcano", "ComplexHeatmap"))
remotes::install_version("msigdbr", version = "25.1.1")
```

## Notes

- An internet connection is required for the gene annotation steps ('biomaRt' queries Ensembl).
  
- Ensemble release 115 was used for gene annotation (Ensembl ID to HGNC symbol)

- Results may differ slightly if other package versions or ensemble release are used.

## Contact

Analia Yogi: ayogi.ped@tmd.ac.jp
