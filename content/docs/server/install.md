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

{{< alert icon="👉" text="Throughout this documentation we assume that Helm 3 is used." />}}

## Add the Helm repository

```bash
helm repo add owl https://eddienko.github.io/owl-pipeline-server/
helm repo update
```

## Install the chart

The Owl chart bootstraps the Owl Scheduler on Kubernetes using the Helm package manager.
The chart deploys the following components on the Kubernetes cluster:

* Owl Scheduler
* Owl API

```bash
helm install owl owl/owl \ 
      -n owl --create-namespace -f values.yaml
```

Where `values.yaml` is a YAML file containing local configuration details.

| Name                       | Description                                                                  | Value            |
| -------------------------- | ---------------------------------------------------------------------------- | ---------------- |
| image.repository           | name of the Docker image to use to run the server                            | imaxt/owl-server |
| image.pullPolicy           | pull policy                                                                  | IfNotExists      |
| image.tag                  | image tag                                                                    | latest           |
| token                      | secret token used in the communication between the Owl scheduler and the API |                  |
| loglevel                   | default log level                                                            | INFO             |
| dbi                        | database connection string                                                   |                  |
| resources.limits.cpu       |                                                                              | 2                |
| resources.limits.memory    |                                                                              | 7G               |
| resources.requests.cpu     |                                                                              | 2                |
| resources.requests.memory  |                                                                              | 7G               |
| pipeline.extraVolumes      |                                                                              | {}               |
| pipeline.extraVolumeMounts |                                                                              | {}               |

An example `values.yaml` file:

```yaml
image:
  repository: imaxt/owl-server
  pullPolicy: Always
  tag: "latest"

serviceAccount:
  create: true

token: "fdsf698dfsdfhsdfkjhfsdfy"
loglevel: DEBUG
dbi: sqlite:////var/run/owl/sqlite.db

resources:
  limits:
    cpu: 2
    memory: 7G
  requests:
    cpu: 2
    memory: 7G

pipeline:
  extraVolumeMounts:
    - name: meds1a
      mountPath: /data/meds1_a
  extraVolumes:
    - name: meds1a
      hostPath:
        path: /data/meds1_a
        type: Directory

dask: {}
scheduler: {}
api: {}
```