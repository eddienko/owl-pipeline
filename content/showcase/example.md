---
title: "Example"
description: "Example pipeline."
lead: "The example pipeline can be used for testing and as a template to build more complicated pipelines."
date: 2021-08-15T20:55:31+01:00
lastmod: 2021-08-15T20:55:31+01:00
draft: false
images: []
menu:
  showcase:
    parent: "browse"
    identifier: "example-pipeline"
weight: 20
toc: false
link: https://github.com/eddienko/owl-example-pipeline
---

Given a list of `1...n` numbers does a series of operations in each of them and computes the result sum using Dask delayed adding random sleep times in each operation. The `datalen` argument is the length of the list.


## Install

```bash
curl -O https://raw.githubusercontent.com/eddienko/owl-example-pipeline/main/owl_example/signature.yml
owl admin pdef add signature.yml
```

## Pipeline Definition File


```yaml
version: 1

# Name of the pipeline
name: example

# Pipeline arguments
datalen: 100

# Resources requested
resources:
  cores: 10
  workers: 2
  memory: 10
```
