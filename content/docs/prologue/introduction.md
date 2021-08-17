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
The [Owl Server]({{< relref "docs/server/install.md" >}}) is installed in the Kubernetes cluster and is in charge
of receiving requests for jobs, allocating the necessary resources and running them. This consists of the scheduler itself
and a lightway API that is used to communicate with the user.

The [Owl Client]({{< relref "docs/client/install.md" >}}) runs in the user space
(i.e. in any other system outside of the cluster) and is used to interact with the server,
mainly to submit jobs and perform some administrative tasks.

{{< alert icon="💡" >}}
We are releasing a preview version of the Owl Scheduler that has been in production for two years.
While not all features have been released yet, we are working on bringing them in.
The current version is however usable and we welcome any input from the community in our
[Discussions Forum](https://github.com/eddienko/owl-pipeline/discussions).
{{< /alert >}}

## Pre-Requisites

{{< alert icon="👉" text="You will need a running Kubernetes cluster." />}}

In order to install and run the scheduler you need access to a Kubernetes cluster. We have tried Owl in the following
versions:

* Kubernetes 1.20 (Google Cloud Engine)
* Kubernetes 1.20 (On premises cloud)
* Kubernetes 1.21 (Docker Desktop)
* Kubernetes 1.22 (On premises cloud)

## Get started

### Install the Owl Server

Step-by-step instructions on how to install the server in your Kubernetes cluster. [Owl Server →]({{< relref "docs/server/install.md" >}})

### Install the Owl Client

Step-by-step instructions on how to install the client. [Owl Client →]({{< relref "docs/client/install.md" >}})

### Tutorial


Step-by-step instructions on how to run your first job. [Tutorial →]({{< relref "docs/client/install.md" >}})


## Contributing

Find out how to contribute to the project. [Contributing →]({{< relref "docs/help/how-to-contribute.md" >}})

## Help

Browse the Help section or head to the [Discussions Forum →](https://github.com/eddienko/owl-pipeline/discussions)
