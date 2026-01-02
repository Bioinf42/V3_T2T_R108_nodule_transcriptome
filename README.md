markdown

# *Medicago truncatula* R108 genome V3 T2T Nodule Transcriptome Pipeline 

---
## Overview
This pipeline processes unstranded paired-end RNA-seq data to build a transcriptome for *Medicago truncatula* R108 root nodules, also reffered to as *Medicago littoralis*. 
It performs quality control, trimming, alignment to the R108 V3 T2T reference genome, transcript assembly, merging, and comparison to the reference annotation, producing a comprehensive nodule transcriptome for downstream analysis. It provides it in a docker container
to be used on any sytsem. 

---
## Table of Contents
- [Overview](#overview)
- [Prerequisites](#prerequisites)  
- [Repository Contents](#repository-contents)  
- [Directory structures](#directory-structures)
- [Installation](#installation)  
- [Usage](#usage)  
- [Outputs](#outputs)  
- [Transcript Filtering](#transcript-filtering) 
- [License](#license)  
- [Citation](#citation) 

---
## Prerequisites
- System: Linux (e.g., Ubuntu)

### Software included (`env.yaml`)
- 'Conda-forge' : Used to manage and download other software 
- 'Python-3.11' : Custom merge and filtering script
- 'Java (OpenJDK 11) : Required for Trimmomatic
- 'fastqc_v0.12.1' : For quality control 
- 'Trimmomatic-0.39' : For trimming and adaptor removal 
- 'hisat2-2.2.1' : For alignment
- 'samtools-1.18' : For BAM conversion and sorting
- 'stringtie-3.0.0' : For transcript assembly
- 'gffread-0.12.7' : For extracting transcript sequence
- 'gffcompare-0.11.2' : To compare transcriptome gtf to original genome gtf

### Input Files:
- Paired-end RNA FASTQ files (e.g., `SAMPLE_R1_001.fq.gz`, `SAMPLE_R2_001.fq.gz`)
- Reference genome - "R108_T2T.v3.0.fa"
(https://figshare.com/ndownloader/files/56630867)
- Reference annotation - "R108_T2T.v3.0.gff"
(https://figshare.com/ndownloader/articles/29665022/versions/3?folder_path=Medicago_gene_anno_newVersion)

### Hardware Recommendations 
- Multi-core CPU (16+ threads recommended)
- Sufficient disk space for FASTQ and output files.

---
## Repository Contents
- `run_docker.sh`: The docker container launcher script.

- `transcriptome_build_pipe.sh`: Main end-to-end pipeline script, makes directories and runs tools (trimming, alignment, assembly, merging, comparison). 

- `env.yaml` : packages and dependencies. 

- `Dockerfile` : instructions docker uses to build the image and runs micromamba. 

---

## Directory structures

The repository has the following structure upon download:

```bash
Base/
├── docker/
│   ├── work/
│   ├── Dockerfile
│   └── env.yaml
├── filtering_scripts/
│   ├── filter_tx_tpm_matrix.py 
│   └── make_tx_tpm_matrix.py
├── raw_data/
├── reference/
├── run_docker.sh
├── transcriptome_build_pipe.sh
├── README.md        
└── LICENSE   
```

The repository after running the pipeline:

```bash
Base/
├── docker/
│   ├── work/
│   ├── Dockerfile
│   └── env.yaml
├── filtering_scripts/
│   ├── filter_tx_tpm_matrix.py 
│   └── make_tx_tpm_matrix.py
├── raw_data/
├── reference/
├── run_docker.sh
├── transcriptome_build_pipe.sh
├── README.md  
├── LICENSE     
├── logs
└── results 
    ├── fastqc_untrimmed/
    ├── trimmed/
    ├── fastqc_trimmed/
    ├── index/
    ├── aligned_bam/
    ├── stringtie_counts
    ├── stringtie_transcripts/
    ├── stringtie_merged/
    ├── gtf_compare/
    ├── stringtie_filtered/
    └── final_gtf/
        └── gff_final_compare/
```
