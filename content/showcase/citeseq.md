---
title: "CITE-seq-Count"
description: "A tool that allows to get UMI counts from a single cell protein assay."
lead: "A tool that allows to get UMI counts from a single cell protein assay."
date: 2021-08-15T20:55:31+01:00
lastmod: 2021-08-15T20:55:31+01:00
draft: false
images: []
menu:
  showcase:
    parent: "browse"
    identifier: "example-citeseq"
weight: 70
toc: false
link: https://github.com/eddienko/owl-cite-seq-count-pipeline
---

This pipeline wraps the [CITE-seq-Count](https://github.com/Hoohm/CITE-seq-Count) tool to run
in Owl. Please follow the link to learn more about this tool.

The pipeline definition file is:

```yaml
# Version of the configuration file
version: 1

# Name of the pipeline
name: cite_seq_count

# Pipeline arguments
# Read1 fastq file location in fastq.gz format. 
# Read 1 typically contains Cell barcode and UMI
read1: /storage/admin/cite/5kPBMC/big_R1.fastq.gz
# Read2 fastq file location in fastq.gz. 
# Read 2 typically contains the Antibody barcode.
read2: /storage/admin/cite/5kPBMC/big_R2.fastq.gz
# The path to the csv file containing the antibody 
# barcodes as well as their respective names.
tags: /storage/admin/cite/5kPBMC/tags.csv
# First nucleotide of cell barcode in read 1
cell_barcode_first_base: 1
# Last nucleotide of the cell barcode in read 1
cell_barcode_last_base: 16
# First nucleotide of the UMI in read 1
umi_first_base: 17
# Last nucleotide of the UMI in read 1
umi_last_base: 28
# How many cells you expect in your run
expected_cells: 6000
# How many bases should we trim before starting to map
trim: 10
# Output
output: /storage/admin/5kPBMC/output

# Extra arguments
# extra: ["--max-error", "3", "--no_umi_correction"]

# Resources requested
resources:
  threads: 10
  workers: 1
  memory: 32
```
