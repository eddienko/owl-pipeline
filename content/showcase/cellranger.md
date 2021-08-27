---
title: "Cell Ranger"
description: "Cell Ranger."
lead: "The cellranger pipeline runs Cell Ranger to process single-cell data."
date: 2021-08-15T20:55:31+01:00
lastmod: 2021-08-15T20:55:31+01:00
draft: false
images: []
menu:
  showcase:
    parent: "browse"
    identifier: "cellranger-pipeline"
weight: 80
toc: false
link: https://github.com/eddienko/owl-cellranger-pipeline
---

[Cell Ranger](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/what-is-cell-ranger) 
is a set of analysis pipelines that process Chromium single-cell data to align reads, generate feature-barcode matrices, perform clustering and other secondary analysis, and more.

Currently we implement the `cellranger count` method that takes FASTQ files from and performs alignment, filtering, barcode counting, and UMI counting. It uses the Chromium cellular barcodes to generate feature-barcode matrices, determine clusters, and perform gene expression analysis.


## Requirements

* The Cell Ranger software and reference data must be available to the pipeline in a mounted volume. By default cellranger is located in `/soft/cellranger/bin/cellranger`. Use the `soft` argument in the pipeline definition file (see below) to change it.

## Install

```bash
curl -O https://raw.githubusercontent.com/eddienko/owl-cellranger-pipeline/main/owl_cellranger/cellranger.yaml
owl admin pdef add cellranger.yaml
```
## Pipeline Definition File

An example pipeline definition file is:

```yaml
# Version of the configuration file
version: 1

name: cellranger

# Location of the cellranger software
# soft: /soft/cellranger
# command to run, currently only count
command: count
# Arguments for the command above
id: SITTF9
sample: SITTF9
transcriptome: /storage/shared/cellranger_refs/refdata-gex-mm10-2020-A
fastqs: /storage/user/cellranger/SLX-20946
output_dir: /storage/user/cellranger/runs

# extra
# useful to add localcores and localmem in case cellranger fails to detect them
extra: ["--localcores", "20", "--localmem", "64"]

requirements:
  workers: 1
  cores: 25
  memory: 64
```