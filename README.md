# nLTR Retrotransposon Discovery and Analysis Pipeline

## Overview

This repository contains the computational pipeline used for the discovery and analysis of nLTR (non-long terminal repeat) retrotransposons, specifically in the context of the research detailed in our paper:

**Fell, C.W., Villiger, L., Lim, J. *et al*. Reprogramming site-specific retrotransposon activity to new DNA sites. *Nature* (2025).**
Paper Link: [https://www.nature.com/articles/s41586-025-08877-4](https://www.nature.com/articles/s41586-025-08877-4)

The pipeline outlines the steps from initial Open Reading Frame (ORF) prediction in eukaryotic assemblies to detailed annotation and phylogenetic analysis of the identified retrotransposon elements.

## Pipeline Stages

The core pipeline consists of the following major stages. For detailed commands and parameters for each step, please refer to the `nLTR_RT_search_pipeline.md` file in this repository.

1.  **ORF Calling:**
    * Prediction of ORFs from genomic assemblies using `orfm`.

2.  **RT and RLE Model Construction:**
    * Building Hidden Markov Models (HMMs) for Reverse Transcriptase (RVT) and Endonuclease (RLE) domains using `hmmbuild`.

3.  **RVT and RLE Domain Search:**
    * Searching for RVT and RLE domains within the predicted ORF translations using `hmmsearch`.

4.  **ORF Filtering:**
    * A series of filtering steps to select high-confidence retrotransposon candidates based on E-value, bias, length, amino acid composition, and domain architecture.

5.  **Annotating ORFs:**
    * Comprehensive annotation of filtered ORFs using various databases and tools:
        * NCBI Conserved Domain Database (CDD) via `rpsblast` and `rpsbproc`.
        * Pfam database via `hmmscan`.
        * Rfam database (for potential ncRNA elements flanking ORFs) via `cmscan`.
        * PROSITE profiles (for zinc finger and MYB domains) via `ps_scan.pl`.
        * Tandem Repeats Finder (`trf`).

6.  **Phylogenetic Tree Construction:**
    * A multi-step process to build a phylogenetic tree of the identified elements:
        * Sequence extraction and clustering (permissive and strict criteria using `mmseqs easy-cluster`).
        * Multiple sequence alignment (using `muscle` and `mafft`).
        * Alignment trimming (using `clipkit`).
        * Phylogenetic tree inference (using `FastTreeMP`).

7.  **Computational Prediction of ncRNA:**
    * Analysis for ncRNA conservation, linking to the [ncrna_conservation GitHub repository](https://github.com/abugoot-lab/ncrna_conservation).

8.  **Construction of Representative Set of ORFs:**
    * Clustering to identify a representative set of ORFs using `mmseqs easy-cluster`.

## Output Data Files

As supplementary data to the pipeline and publication, we provide two key output files:

* **`orf_list_supplementary_table_full_set.csv.zip`**: This file contains the complete set of 35,248 ORFs identified and processed through the initial stages of the pipeline, prior to the final representative clustering. It is provided as a `.zip` archive due to its large size.
* **`orf_list_supplementary_table_clustered_unique.csv`**: This file contains the representative set of 8,248 unique ORFs after the clustering step (as described in "Construction of Representative Set of ORFs"). This is often referred to as the "reps" file and may be particularly useful for downstream analyses requiring a non-redundant set.

These files are located in the `data/` subdirectory within this repository.

## Usage

The detailed commands, including example parameters and tool versions (where applicable), are provided in the `nLTR_RT_search_pipeline.md` file. Users should adapt paths to databases, input files, and output directories according to their own system and data.

## Requirements (Example)

This pipeline relies on several bioinformatics tools and databases. Key dependencies include (but are not limited to):

* `orfm`
* HMMER suite (`hmmbuild`, `hmmsearch`, `hmmscan`)
* NCBI BLAST+ suite (`rpsblast`) and CDD utilities (`rpsbproc`)
* Infernal (`cmscan`)
* PROSITE scan tools (`ps_scan.pl`)
* Tandem Repeats Finder (`trf`)
* `mmseqs2`
* `MUSCLE` (for `muscle -super5`)
* `MAFFT`
* `ClipKIT`
* `FastTreeMP`
* Perl
* Access to NCBI CDD, Pfam, Rfam, and PROSITE databases.

Please ensure these tools are installed and accessible in your environment. Specific versions used in the original study can be inferred from the publication or may be available upon request.

## Citation

If you use this pipeline or parts of it in your research, please cite our paper:

Fell, C.W., Villiger, L., Lim, J. *et al*. Reprogramming site-specific retrotransposon activity to new DNA sites. *Nature* (2025).
(Paper Link: [https://www.nature.com/articles/s41586-025-08877-4](https://www.nature.com/articles/s41586-025-08877-4))

## License

This work is licensed under the [Creative Commons Attribution-NonCommercial 4.0 International License (CC BY-NC 4.0)](http://creativecommons.org/licenses/by-nc/4.0/).
