---
title: "Pipeline Signatures"
description: ""
lead: ""
date: 2021-08-15T20:55:31+01:00
lastmod: 2021-08-15T20:55:31+01:00
draft: false
images: []
menu: 
  docs:
    parent: "pipelines"
weight: 502
toc: true
---

The pipeline definition signatures that are used with the `admin pdef` commands
(see [Admin commands]({{< relref "docs/client/admin.md#pipeline-definitions" >}}))
are slightly different to the pipeline definition files used to submit a pipeline.

The difference is the pressence of two additional keywords in the former: `extra_pip_packages` and `active`. 

```yaml
# Version of the configuration file
version: 1

# Pipeline signature
extra_pip_packages: owl-example-pipeline
active: True

# Name of the pipeline
name: example

# Pipeline arguments
datalen: 100

# Resources requested
resources:
  threads: 10
  workers: 2
  memory: 2
```

**extra_pip_packages** lists the package that contains the pipeline
and **active** sets if users can use it to run pipelines in the server.

{{< alert icon="💡" >}}
At the moment it is not possible to have two versions of the same pipeline signature.
{{< /alert >}}