# SGA_AGA

R Pipeline for the paired RNA-seq analysis of human umbilical cord mesenchymal stem cells (hUCMSCs) and derived adipogenic cultures.

Samples: Umbilical cord tissues from birth weight discordant monozygotic diamniotic twins (at least 20%; SGA-AGA co-twins) were collected for this study.

## Reference 

Analia Yogi, Yusuke Noguchi, Shizuka Kirino, Haruki Yamano, Eriko Adachi, Hisae Nakatani, Yoko Saito, Chikako Morioka, Mari Hayata, Manabu Sugie, Kei Takasawa, Tomohiro Morio, Masatoshi Takagi, Atsumi Tsuji-Hosokawa, Kenichi Kashimada.

"Fetal environment associated with small for gestational age birth transcriptionally primes umbilical cord mesenchymal stem cells toward osteogenic pathways with potentially impaired adipogenic differentiation". Placenta (Elsevier), 2026.

https://doi.org/10.1016/j.placenta.2026.09.009

## Data

Raw RNA-se data are publicly available at the DNA Data Bank of Japan (DDBJ). The accession number is PRJDB40126.

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


## Contact

Analia Yogi: ayogi.ped@tmd.ac.jp
