# Pipeline for Passive Filtration study

This repository describes the bioinformatic steps used to process sequence data for the Passive filtration paper (Jeunen et al., in prep). Any scripts used for processing are included. 

## Adapter trimming and quality filtering of raw sequence data

The program [**Cutadapt**](https://cutadapt.readthedocs.io/en/stable/) was used to trim adapters, followed by quality filtering using [**VSEARCH**](https://github.com/torognes/vsearch). After processing the programs [**FastQC**](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/) and [**MultiQC**](https://multiqc.info/) were used to evaluate the results. In order to streamline this operation, a Python script was used to run these programs. The included script *cutadaptQC* performed the following operations:

- Read the primer and barcode sequences from the metadata file
- Used cutadapt to trim the primer-barcode sequence from ends of each sequence
- Used the VSEARCH command `-fastq_filter` to quality filter the cutadapt outputs and create fasta files
- Create a manifest file that contains the path of each resulting sample output for tracking
- Count the number of reads resulting from filtering and update the metadata file
- Run FastQC on each filtered sequence file
- Run MultiQC to create a summary file of each FastQC output

The script was run using the following parameters:

```
cutadaptQC \
 -m tank_experiment_metadata.tsv \
 -r Undetermined_S0_L001_R1_001.fastq.gz \
 -f ../data \
 -t 4 \
 -n passive_filter_expt
 -l 150
```

where `-m` is the metadata table used, `-r` is the name of the raw sequence fastq file, `-f` is the destination folder for outputs, `-t` is the number threads, `-n` is the name of the experiment (applied to output summary files), `-l` is the minimum length for output reads. 

The defaults for the other parameters were used: MAX_LENGTH = 300, MAXN (maximum number of N's) = 0, ERROR (maximum expected error value) = 1.0. 

The metadata table (with number of reads from trimming/filtering) and the MultiQC summary HTML files are included here.

## OTU clustering


