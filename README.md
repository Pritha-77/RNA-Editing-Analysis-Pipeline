# RNA Editing Analysis Pipeline for Adipogenesis



## Overview
This pipeline detects RNA editing events (primarily A-to-I) from BAM files using **REDItools3**, performs **differential editing analysis** with limma/edgeR, and annotates sites with genomic features.

Key features:
- Environment setup via conda
- BAM validation and indexing checks
- Statistical filtering (coverage ≥10, frequency ≥1%)
- Normalization (arcsin(sqrt)) and DEG-like analysis for editing events
- BED annotation with Gencode vM25

## Requirements
- Indexed BAM files (coordinate-sorted)
- Reference genome FASTA (GRCm38.primary_assembly.genome.fa)
- GTF annotation (gencode.vM25.annotation.gtf)
- Linux/Ubuntu with conda

## Quick Setup & Run
1. **Environment** (see [RNA_Editing_Pipeline.md](RNA_Editing_Pipeline.md)):
   ```
   conda create -n reditools3 python=3.10 -c conda-forge
   conda activate reditools3
   conda install -c bioconda -c conda-forge reditools3 samtools bedtools gffread
   ```

2. **Detection**:
   ```
   python -m reditools analyze -r GRCm38.genome.fa -o outputs/ sample.bam
   ```

3. **R Analysis** (limma/edgeR):
   ```r
   library(limma); library(edgeR); library(data.table)
   
   ```

4. **Annotation**:
   ```
   gffread gencode.vM25.annotation.gtf -T -o gencode.bed
   bedtools intersect -a diff_editing.bed -b gencode.bed -wa -wb
   ```

## Outputs
| File | Description |
|------|-------------|
| REDItools3_Outputs_*.tsv | Raw editing sites (Region, Position, Frequency, Coverage-q30)[1] |
| Diff_Editing_P1_lfc0.tsv | Differential sites (p-value, logFC, adj.p) |
| annotated_Diff_Editing_P1_lfc0.bed | Gene/region annotations |
