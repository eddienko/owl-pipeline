---
title: "Introduction"
description: ""
lead: ""
date: 2020-10-06T08:48:57+00:00
lastmod: 2020-10-06T08:48:57+00:00
draft: false
images: []
menu:
  docs:
    parent: "prologue"
    identifier: "main-intro"
weight: 101
toc: true
---

The Owl Scheduler is made from two different components. 
The [Owl Server]({{< relref "docs/server/introduction.md" >}}) is installed in the Kubernetes cluster and is in charge
of receiving requests for jobs, allocating the necessary resources and running them.

The [Owl Client]({{< relref "docs/client/introduction.md" >}}) runs in the user space
(i.e. in any other system outside of the cluster) and is used to interact with the server,
mainly to submit jobs and perform some administrative tasks.
## Pre-Requisites

{{< alert icon="👉" text="You will need a running Kubernetes cluster." />}}

In order to install and run the scheduler you need access to a Kubernetes cluster. We have tried Owl in the following
versions:

* Kubernetes 1.21 (Docker Desktop)
* Kubernetes 1.22

## Get started

### Install the Owl Server

Step-by-step instructions on how to install the server in your Kubernetes cluster. [Owl Server →]({{< relref "docs/server/introduction.md" >}})

### Install the Owl Client

Step-by-step instructions on how to install the client. [Owl Client →]({{< relref "docs/client/introduction.md" >}})

### Tutorial


Step-by-step instructions on how to run your first job. [Tutorial →](https://getdoks.org/tutorial/introduction/)


## Contributing

Find out how to contribute to the project. [Contributing →](https://getdoks.org/docs/contributing/how-to-contribute/)

## Help

Get help on Doks. [Help →]
