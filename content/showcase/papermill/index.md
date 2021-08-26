---
title: "Papermill"
description: "Papermill pipeline."
lead: "Papermill pipeline."
date: 2021-08-15T20:55:31+01:00
lastmod: 2021-08-15T20:55:31+01:00
draft: false
images: []
menu:
  showcase:
    parent: "browse"
    identifier: "papermill-pipeline"
weight: 15
toc: false
link: https://github.com/eddienko/owl-papermill-pipeline
---


The papermill pipeline executes Jupyter notebooks using
[Papermill](https://papermill.readthedocs.io). Plase read the official
documentation on how to paramaterize the notebooks.

An example pipeline definition file is below (and can be obtained typing `owl
pdef get papermill`:

```yaml
version: 1.2

# Name of the pipeline
name: papermill

input: /home/user/myDir
output: /home/user/myDir
notebook: notebook.ipynb

paramaters: {}

# use_dask: false

resources:
  workers: 1
  memory: 8
  threads: 1
```

The pipeline requires the input and output directories and the name of the
notebook to execute. The executed notebook will be saved in the output directory
with the name `notebook_out.ipynb` in this example.

The parameters required to run the notebook are defined in parameters. 

The following shows an example output notebook that includes outputs and figures.

{{< img src="paperpipe.png" alt="Papermill Pipeline" caption="<em>Papermill Pipeline</em>" class="border-0" >}}