---
title: "Owl Pipelines"
description: ""
lead: ""
date: 2021-08-15T21:00:06+01:00
lastmod: 2021-08-15T21:00:06+01:00
draft: false
images: []
menu: 
  docs:
    parent: "pipelines"
weight: 501
toc: false
---

Owl pipelines are pip installable Python packages. In order to submit a pipeline
to the system, the admin must have activated it and you need a 
**pipeline definition file (PDeF)**. This is a YAML file that contains 
the name of the pipeline, its arguments and requested resources.

A pipeline defintion file typically starts with setting a version number. This is used
so that individual pipelines can limit the version numbers that they can run after e.g.
a package upgrade.

Then the arguments that are required for running the pipeline.

Last the resources requested. The number of workers is set by `workers`. The values
in `cores` and `memory` specify the number of cores (or vCPUs) and memory in GB per worker. 
Optionally it is possible to specify a Docker image used to run the jobs, otherwise the
default is used.

```yaml
version: 1

# ... pipeline arguments ...

arg1: value1
arg2: value2
arg3: true

# ... end pipeline arguments ...

resources:
  workers: 3
  cores: 10
  memory: 30
  # Image is optional
  image: user/image-name:tag
```