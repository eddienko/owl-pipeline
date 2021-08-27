---
title: "Architecture"
description: ""
lead: ""
date: 2021-08-16T08:07:35+01:00
lastmod: 2021-08-16T08:07:35+01:00
draft: false
images: []
menu: 
  docs:
    parent: "prologue"
weight: 102
toc: true
---

## Introduction

A job scheduler is an application that takes care of running unattended jobs in
a compute cluster. Typically the scheduler will add jobs to a queue and run them
when resources become available. Some widely used schedulers (mainly in HPC
context) are Portable Batch System (PBS), Slurm Workload Manager, Condor, Moab,
and many others.

In the context of cloud systems and in particular Kubernetes there is not a wide
range of applications to run user submitted batch jobs. For our use such an
application should be:

  * **Easy to use.** Users should not be expected to know how the cluster is set up. A simple specification of how many CPUs and RAM should suffice in first instance with flexibility for more complex scheduling options.
  * **Remote access.** Users should be able to submit, monitor and manage jobs from their own laptop or desktop without the need to login to a remote host.
  * **Dask support.** It should support Dask jobs, but not exclusively.
  * **Integration with Kubernetes.** The scheduler needs to create services and deployments in the K8s cluster, run jobs and, plug into external components (e.g. Django authentication) and interact with the database.

For this purpose we have built the Owl job scheduler from scratch. Owl is a
framework to execute jobs in a compute cluster backed up by Kubernetes and Dask.
In essence Owl serves as very simple scheduler that accepts job descriptions,
queues them and submits them for execution keeping track of progress.

## Pipeline definition file

When we refer to pipelines we understand parameterized jobs. The typical user chooses which pipeline to run from 
those available in the system and submits a job with its own particular parameters. To do that the user
only needs to write a **pipeline definition file (PDeF)** which includes the name of the pipeline to run,
the arguments needed to run it and a request for resources.
See [Owl Pipelines]({{< relref "docs/pipelines/index.md" >}}) for additional information.
## Job Lifecycle

In order to run a particular job (pipeline) users write a pipeline definition
file, in summary, a file that specifies the pipeline to run and the arguments required
for it to run as well as the compute resources required.

This is submitted to the Owl API who logs the entry in a
database and places the job in a queue. Owl checks periodically for pipelines in
the queue and if they meet the scheduling requirements starts the allocation
process. The main Owl process (a.k.a. Owl Scheduler) delegates the reponsibility
of running the pipeline to a pipeline worker Pod process that is in charge of
contacting the K8s cluster, allocating the resources neeed and submitting the
pipeline to the compute cluster.

Depending on the pipeline requested resources, one or more containers running
Dask are allocated across the cluster and the pipeline starts execution. The
Worker keeps communication with the running pipeline at all times. When
finished, or in case of error, informs Owl who retrieves the information from
the Worker, stops it and contacts the API for a database update.

The steps followed when running a pipeline are:

  * The user submits a pipeline definition file that is a YAML text file.
  * The database is updated with the pipeline request. The scheduler queries the database when slots are available and allocates pipelines.
  * The Owl scheduler starts a pipeline worker in a Pod sending the pipeline definition file and extra configuration needed.
  * The pipeline worker loads the pipeline code from the pip repository and validates inputs against its schema.
  * The Kubernetes cluster starts the requested number of Pod Dask workers.
  * The pipeline worker starts a separate thread and runs the pipeline code. The code runs in the allocated containers using Dask taking advantage of parallelization over all workers.
  * The main pipeline thread listens for heartbeat connections from the scheduler and waits for pipeline completion.
  * The pipeline worker responds with the pipeline completion result to the scheduler.
  * The scheduler stops the pipeline worker and logs an entry in the database.

