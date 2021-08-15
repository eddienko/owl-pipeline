---
title: "Quick Start"
description: ""
lead: ""
date: 2021-08-15T10:59:50+01:00
lastmod: 2021-08-15T10:59:50+01:00
draft: false
images: []
menu: 
  docs:
    parent: "client"
    identifier: "client-quick-start"
weight: 402
toc: true
---

## Install

The Owl client requires Python 3.7 or higher. The latest stable version can be 
installed from PyPi:

```bash
pip install owl-pipeline-client
```

## Authenticate to remote server

Using your credentials 

```bash
owl login
```

This will ask for your username and password. Credentials will be saved to a file in your home 
directory `$HOME/.owlrc`.

## Pipeline definition file

List all available pipelines in the remote server using:

```bash
owl pdef list
```

If the server has been configured as per the [Quick Start]({{< relref "quickstart" >}})
instructions the `example` pipeline will be available.
Retrieve the pipeline definition file using:

```bash
owl pdef get example -o example.yml
```

This looks like:

```yaml
# Version of the configuration file
version: 1

# Name of the pipeline
name: example

# Pipeline arguments
datalen: 100

# Resources requested
resources:
  threads: 10
  workers: 2
  memory: 10
```

The example pipeline runs a series of dummy computations using [Dask](https://dask.org). 
It has a unique input parameter
`datalen` which basically controls how long the pipeline runs. 

## Submit pipeline

Adjust the resources and submit the pipeline using:

```bash
owl submit example.yml
```


