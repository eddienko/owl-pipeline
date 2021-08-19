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

{{< alert icon="info" text="Throughout this documentation we assume that you have a Kubernetes cluster already spin up and the control panel is reachable using command line tools. Helm 3 is used." />}}

## Add the Helm repository

If you have not done so, install [Helm](https://helm.sh/docs/intro/install/). Then add the repository
that contains the Owl chart:

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

This creates a namespace called `owl` if it does not exist and deploys all components there.
Here `values.yaml` is a YAML file containing local configuration details. 
A minimal example `values.yaml` file:

```yaml
# Name of Docker image used to run the server
image:
  repository: imaxt/owl-server
  pullPolicy: IfNotPresent
  tag: "0.6.1"

# Create service account in RBAC clusters
serviceAccount:
  create: true

# Secret token for communication between the Owl scheduler and the API
token: "hdCI6MTYyOTI5NTczM30.tWSY38h7HfhtAnMkVJWf9gF-fU_EHCTB7zrar3QyLiA"

# Global log level
loglevel: DEBUG

# Database DBI
dbi: sqlite:////var/run/owl/sqlite.db
```

More complete examples can be found in the
[repository](https://github.com/eddienko/owl-pipeline-server/blob/main/examples).
## Inspect the deployment status

After deployment, you should have two pods running coallocated in the same node
(in the default deployment both pods share the same PVC). The first pod is
the Owl API which is used to query the database and allow for external interaction,
i.e. to submit and manage jobs. The second pod, the scheduler, queries the
API for submitted jobs and schedules them in the cluster.

```bash
$ kubectl get all --namespace owl
NAME                                 READY   STATUS    RESTARTS   AGE
pod/owl-api-7cd474c655-9s887         1/1     Running   0          18m
pod/owl-scheduler-7996ff7f76-ktgfp   1/1     Running   0          22m

NAME                        TYPE           CLUSTER-IP    EXTERNAL-IP     PORT(S)             AGE
service/owl-api             LoadBalancer   10.4.15.167   34.88.233.217   8002:30923/TCP      3h36m
service/owl-scheduler       ClusterIP      10.4.7.4      <none>          7001/TCP,7002/TCP   3h36m

NAME                            READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/owl-api         1/1     1            1           95m
deployment.apps/owl-scheduler   1/1     1            1           95m

NAME                                       DESIRED   CURRENT   READY   AGE
replicaset.apps/owl-api-7cd474c655         1         1         1       95m
replicaset.apps/owl-scheduler-7996ff7f76   1         1         1       55m
```

A common reason that pods stay in `pending` is the lack of resources. Make sure that at least one 
node has resources to schedule both pods. In any case
`kubectl describe pod [name-of-the-pod] --namespace owl` should give the cause.

## Expose the API service

The API service is exposed using a LoadBalancer. The external IP can be obtained either looking 
at the output from the command above or directly using:

```bash
kubectl get services --namespace owl \
 -l "app.kubernetes.io/name=owl-api,app.kubernetes.io/instance=owl" \
 -o jsonpath="{.items[0].status.loadBalancer.ingress[0].ip}"
```

The address of the API server is then `http://34.88.233.217:8002`. Exposing this to the outside
world can be also accomplished with a default ingress or using traefik. For reference this is a traefik
deployment to make the API available in a subpath
[ingress.yaml](https://github.com/eddienko/owl-pipeline-server/blob/main/examples/imaxt/ingress.yaml).


## Deployment in GKE

In Google Kubernetes Engine the cluster created with GKE Autopilot is not yet supported. Use a standard GKE cluster instead with (some) nodes using at least the `e2-standard-4` type.

An example values file for deploying the Helm chart in GKE is available from
[values.yaml](https://github.com/eddienko/owl-pipeline-server/blob/main/examples/gce/values.yaml).
