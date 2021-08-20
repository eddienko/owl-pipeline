---
title: "Development Guide"
description: ""
lead: ""
date: 2021-08-15T20:55:31+01:00
lastmod: 2021-08-15T20:55:31+01:00
draft: false
images: []
menu: 
  docs:
    parent: "pipelines"
weight: 503
toc: true
---

Owl pipelines are pip installable Python packages. This page shows how to develop a custom pipeline.
The [example pipeline]() is a good starting point. The directory structure is similar to any Python
package.

```bash
.
├── LICENSE
├── README.rst
├── requirements_dev.txt
├── setup.cfg
├── setup.py
├── conf
│   └── example.yml
├── doc
│   ├── Makefile
│   ├── _static
│   │   └── readme
│   ├── _templates
│   │   └── readme
│   ├── conf.py
│   └── index.rst
├── owl_example
│   ├── __init__.py
│   ├── main.py
│   └── schema.py
└── tests
    └── test_main.py
```

The particularities that make a Python package an Owl pipeline are explained below.
## Validation schema

The file `owl_example/schema.py` defines the input arguments to the pipeline. In the case below
there are two parameters, `datalen` which is required and can take any integer number between 1 and 1000,
and `output_dir` which is an optional argument that defines the output directory.

The validation uses [voluptuous](https://github.com/alecthomas/voluptuous). The schema is validated
by Owl prior to launching the pipeline.

```python
import voluptuous as vo

schema = vo.Schema({
  vo.Required("datalen"): vo.Range(1, 1000), 
  vo.Optional("output_dir"): str
})
```

## Main pipeline

```python
import dask
from owl_dev import pipeline
from owl_dev.logging import logger

@pipeline
def main(*, datalen: int, output_dir: Path=None) -> int:
    ...
    logger.info("Starting computation")
    res = dask.delayed(do_something)(datalen, output_dir)
    dask.compute(res)
    ...
```

## Entrypoint

In order to make the pipeline discoverable by Owl, it needs to define an entrypoint with
they key `owl.pipelines` with content `pipeline_name = pipeline_module` as shown below.

```python
from setuptools import find_packages, setup

setup(
    ...
    entry_points={"owl.pipelines": "example = owl_example"},
    ...
)
```