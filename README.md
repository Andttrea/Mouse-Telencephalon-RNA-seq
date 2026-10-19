# Transcriptomic changes during mouse telencephalon development

**Bulk RNA-seq analysis | Embryonic days E11.5 vs. E14.5 | *Mus musculus***

**Author:** Andrea Villarruel García  
**Context:** Final project for an undergraduate Transcriptomics course (2026)

## Overview

This project explores how gene expression changes during embryonic development of the mouse telencephalon, with an emphasis on the transition from early neural progenitor proliferation toward increased neurogenesis. I reanalyzed **publicly available bulk RNA-seq data**, comparing embryonic day E14.5 with E11.5 while accounting for sex in the differential expression model.

**Research question:** Which genes and biological processes show expression changes between E11.5 and E14.5 in the embryonic mouse telencephalon, after adjusting for sex?

This is a computational reanalysis of published sequencing data, not an experiment in which I generated the biological samples or sequencing libraries.

## Dataset and experimental design

| Item | Description |
|---|---|
| Public dataset | GEO GSE252922 |
| Biological material | Embryonic mouse telencephalon (*Mus musculus*) |
| Conditions | E11.5 and E14.5; female and male |
| Samples | 12 biological samples (3 per age–sex group) |
| Sequencing | Paired-end, 100-bp RNA-seq; Illumina HiSeq 2500 |
| Reference | GRCm38/mm10, with GENCODE vM10 gene annotation |
| Main comparison | E14.5 vs. E11.5, adjusting for sex |

The sample accessions and experimental metadata are available in `data/metadata/`.

## Analysis workflow

The analysis followed these main stages:

1. **Quality assessment:** Initial read quality evaluation using FastQC.
2. **Read preprocessing:** Quality filtering and trimming with fastp.
3. **Reference alignment:** Mapping paired-end reads to the mouse reference genome using STAR.
4. **Gene-level quantification:** Counting fragments with featureCounts using reverse-stranded library settings.
5. **Differential expression:** Comparing embryonic stages with DESeq2 while accounting for sex.
6. **Functional analysis:** Gene Ontology enrichment using DAVID and gene set enrichment analysis with clusterProfiler.

### Differential expression model

The DESeq2 design used in the project was:

`design = ~ 0 + age + sex`

Genes were classified as differentially expressed for the main comparison using **adjusted p-value <escape><</escape> 0.01** and **absolute log2 fold change > 1**.

Positive fold changes indicate higher expression at E14.5 relative to E11.5.

## Main findings

Using these thresholds, the analysis identified **3,273 differentially expressed genes**:

- **2,301 genes** with higher expression at E14.5.
- **972 genes** with lower expression at E14.5.

The principal component analysis indicated a strong association between transcriptional variation and developmental stage.

Functional analyses highlighted processes related to neuronal activity and synaptic signaling among genes with higher expression at E14.5. Processes related to ribosome biogenesis and chromosome organization appeared among negatively enriched gene sets in the ranked analysis.

These findings are consistent with broad transcriptional changes during telencephalon development.

Because this is **bulk RNA-seq from whole tissue**, the analysis cannot distinguish changes in cell-type proportions from changes in expression within individual cell types.

## Selected figures

[results/deseq/figuras/PCA_plot.png](Principal Component analysis)

[results/deseq/figuras/volcano_plot_E145_vs_E115.pngr](Differential expression: E14.5 vs. E11.5)

Further results are available in [results/deseq](results/deseq/) and [results/anotacion](results/anotacion/), including Gene Ontology and GSEA analyses.

## Repository organization

[data/metadata/](data/metadata/) — Sample information and accession tables.

[src/](src/) — Bash and R scripts for sequencing-data processing and analysis.

[results/fastqc/](results/fastqc/)— Quality-control figures.

[results/deseq](results/deseq/) — Differential-expression results and visualizations.

[results/anotacion/`](results/anotacion/) — Functional enrichment and STRING outputs.

[docs/](docs/) — Coursework report and working documents.

## Reproducibility and scope

This repository preserves the scripts and selected outputs from an undergraduate course project.

**It is not yet a fully portable, one-command workflow:** several scripts still use absolute paths from the original computing environment, and some exploratory code requires cleanup before end-to-end rerunning.

The full FASTQ files, alignment files, and reference genome are not stored here. The sequencing data can be retrieved through the public GEO accession.

The reported findings refer to the completed analysis and saved outputs. The scripts should be reviewed and their paths, dependencies, and file-generation order adapted before reuse in another environment.

## Data source and acknowledgement

This project reanalyzes data from *A Transcriptomic Dataset of Embryonic Murine Telencephalon* (Ochi, Manabe and colleagues, 2024), *Scientific Data*.

DOI: https://doi.org/10.1038/s41597-024-03421-x

GEO: https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE252922

Original sequencing data were generated by the study authors. This repository documents my coursework-based computational analysis and interpretation of those public data.