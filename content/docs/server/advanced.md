---
title: "Advanced"
description: ""
lead: ""
date: 2021-08-15T16:56:40+01:00
lastmod: 2021-08-15T16:56:40+01:00
draft: false
images: []
menu: 
  docs:
    parent: "server"
    identifier: "server-advanced"
weight: 307
toc: true
---

## External user database

TBD. It is possible to have the user database managed by an external application, e.g. a Django site.

## Custom Docker image for pipelines

As part of the `resources` section of the pipeline definition file it is possible
to specify a custom Docker image, e.g.:

```yaml
resources:
  threads: 10
  workers: 2
  memory: 10
  image: myrepo/customimage:tag
```
