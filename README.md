# SGA_AGA

R Pipelines for the paired RNA-seq analysis of human umbilical cord mesenchymal stem cells (hUCMSCs) and derived adipogenic cultures.

Samples: hUCMSCs were established from six monozygotic, monochorionic-diamniotic twin pairs with at least 20% weight discordance. In each pair, the larger twin was classified as appropriate for gestational age (AGA) and the smaller was small for gestational age (SGA). Five of the six pair samples were available for adipogenic differentiation.

## Reference 

Analia Yogi, Yusuke Noguchi, Shizuka Kirino, Haruki Yamano, Eriko Adachi, Hisae Nakatani, Yoko Saito, Chikako Morioka, Mari Hayata, Manabu Sugie, Kei Takasawa, Tomohiro Morio, Masatoshi Takagi, Atsumi Tsuji-Hosokawa, Kenichi Kashimada.

"Fetal environment associated with small for gestational age birth transcriptionally primes umbilical cord mesenchymal stem cells toward osteogenic pathways with potentially impaired adipogenic differentiation". Placenta (Elsevier), 2026.

https://doi.org/10.1016/j.placenta.2026.09.009

## Data

Raw RNA-seq data are publicly available at the DNA Data Bank of Japan (DDBJ). The accession number is PRJDB40126.

The raw counts matrices used as input for the published analysis are available in this repository (one per folder), and were obtained from the raw FASTQ files with fastp (v0.23.4), STAR (v2.7.11b; GRCh38, Ensembl release 111), and featureCounts (v2.0.1), as described in the article.

## Repository structure

- "UCMSC": Includes RNA-seq raw counts matrix from hUCMSCs (six twin pairs), the Ensembl ID to HGNC symbol mapping used for the conversion, and the R pipeline (differential expression, GSEA, leading-edge analysis, and plots shown in figure 2)

- "adipogenic_culture": Includes the RNA-seq raw count matrix collected from hUCMSC after 21 days of adipogenic differentiation (five twin pairs). The Ensembl ID to HGNC symbol mapping used for the conversion, and the R pipeline used for the analysis (differential expression, GSEA, and leading edge analysis) and plots shown in Figure 3B-E and Figure 4, are included.

## Sample naming

Column names use a condition prefix: S = SGA twin and B = AGA co-twin. Sample identifiers were assigned during sequencing and renumbered for figure presentation; the correspondence between original and renumbered identifiers, together with the DDBJ BioSample accessions, is provided in Supplementary Table 1 of the article.

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

The analysis requires R 4.4.1 (Bioconductor 3.19 is not available for 4.5 or later).

```r
install.packages(c("BiocManager", "remotes"))

# Select Bioconductor 3.19 before installing any Bioconductor package.
# Without this step BiocManager installs the current release, which provides newer DESeq2 and fgsea versions and may not reproduce the published figures.
BiocManager::install(version = "3.19")
BiocManager::install(c("DESeq2", "biomaRt", "fgsea", "EnhancedVolcano", "ComplexHeatmap"))

install.packages(c("dplyr", "ggplot2", "plotly", "htmlwidgets", "patchwork", "circlize",
                   "pheatmap", "RColorBrewer", "stringr", "gridExtra", "cowplot", "extrafont"))

remotes::install_version("msigdbr", version = "25.1.1")
```

To check the installed versions against the table above:

```r
sapply(c("DESeq2", "fgsea", "msigdbr", "biomaRt", "ComplexHeatmap", "EnhancedVolcano"),
       function(p) as.character(packageVersion(p)))
```

## Notes

- An internet connection is required for the gene annotation steps ('biomaRt' queries Ensembl).
  
- The scripts call useMart("ensembl", ...), which queries the current Ensembl release at run time. Ensembl release 115 was current when the analysis was performed and was used for gene annotation (Ensembl ID to HGNC symbol and biotype). To reproduce the published annotation, replace that call with:
  
   ```r
   mart <- useEnsembl("ensembl", dataset = "hsapiens_gene_ensembl", version = 115)
   ```


- Results may differ slightly if other package versions or Ensembl release are used.

- If the annotation step fails with "HTTP 403 Forbidden", Ensembl is rejecting requests from biomaRt 2.60.1 (a server-side restriction on older biomaRt versions, unrelated to the scripts). In that case, use the tables `UCMSC_ensembl_to_symbol_mapping.csv` and `adipocyte_ensembl_to_symbol_mapping.csv`, which contain the annotation used for the published analysis (Ensembl release 115).
  
## Contact

Analia Yogi: ayogi.ped@tmd.ac.jp
