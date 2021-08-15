---
title: "Install"
description: ""
lead: ""
date: 2021-08-15T16:56:40+01:00
lastmod: 2021-08-15T16:56:40+01:00
draft: false
images: []
menu: 
  docs:
    parent: "server"
    identifier: "server-install"
weight: 302
toc: true
---

{{< alert icon="👉" text="Throughout this documentation we assume that Helm 3 is installed." />}}

## Add the Helm repository

```bash
helm repo add owl https://eddienko.github.io/owl-pipeline/charts
helm update
```

## Install the chart

The Owl chart bootstraps the Owl Scheduler on Kubernetes using the Helm package manager.
The chart deploys the following components on the Kubernetes cluster:

* Owl Scheduler
* Owl API

```
helm install owl owl/owl-server \ 
      -n owl --create-namespace -f values.yaml
```

Where `values.yaml` is a YAML file containing local configuration details.

| Name                 | Description                                            | Value                  |
|----------------------|--------------------------------------------------------|------------------------|
| image.repository     | name of the Docker image to use to run the server      | imaxt/owl-server       |
| image.pullPolicy     | pull policy                                            | IfNotExists            |
| image.tag            | image tag                                              | latest                 |
| token                | seret token used in the communication between the Owl scheduler ant the API |   |
| loglevel             | default log level                                      | INFO                   |
| dbi                  | database connection string           |         |
| resources.limits.cpu | | 2 |
| resources.limits.memory | | 7G |
| resources.requests.cpu | | 2 |
| resources.requests.memory | | 7G |
| pipeline.extraVolumes | | {} |
| pipeline.extraVolumeMounts | | {} |
