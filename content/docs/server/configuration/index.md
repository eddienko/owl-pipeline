---
title: "Configuration"
description: ""
lead: ""
date: 2021-08-15T16:56:40+01:00
lastmod: 2021-08-15T16:56:40+01:00
draft: false
images: []
menu: 
  docs:
    parent: "server"
    identifier: "server-config"
weight: 303
toc: true
---

## Admin password

On the first launch the database schema is created and a defaul random admin password created. You can recover the 
default password using:

```bash
POD_NAME=$(kubectl get pods --namespace owl -l "app.kubernetes.io/name=owl-scheduler,app.kubernetes.io/instance=owl" -o jsonpath="{.items[0].metadata.name}")
ADMIN_PASSWORD=$(kubectl exec $POD_NAME -- cat /var/run/owl/adminPassword)
echo $ADMIN_PASSWORD
```

## Command line Pod

Run a Owl Client command line Pod in the server or install the Owl Client
for remote access (this assumes the API is available remotely). In the first case run the Pod:

```bash
kubectl apply --namespace owl \
    -f https://raw.githubusercontent.com/eddienko/owl-pipeline-server/main/examples/imaxt/owlcli.yaml
```

and when ready start a command line prompt:

```bash
kubectl exec -it owlcli --namespace owl -- /bin/bash
```

An alternative is to install the [Owl Client]({{< relref "docs/client/install.md" >}}) and define 
an environmental variable pointing to the location of the API:

```bash
export OWL_API_URL=https://api.owl.com
```

## Update admin password (Optional)

Use the password to login and change the admin password:

```bash
# Use the initial password
owl auth login

# Update the password
owl admin user update --admin admin yourNewPassword

# Login with the new password
owl auth login
```

## Add pipeline signatures

```bash
# Example pipeline
curl -O https://raw.githubusercontent.com/eddienko/owl-example-pipeline/main/owl_example/signature.yml
owl admin pdef add signature.yml
```

Check that they are available:

```bash
# List all signatures
owl pdef list

# Get pipeline definition for example pipeline
owl pdef get example
```

## Submit your first pipeline

Retrieve the `example` pipeline definition file:

```bash
owl pdef get example -o example.yaml
```

Change the parameters or resources and submit it with:

```bash
owl job submit example.yaml
```

## Inspect the status

```bash
owl job status 1
```

You can inspect that the job has been scheduled

```bash
$ kubectl get jobs --namespace owl
NAME         COMPLETIONS   DURATION   AGE
pipeline-1   0/1           3m25s      3m25s
```

and that the different pods are running in the cluster:

```bash
$ kubectl get pods --namespace owl
NAME                             READY   STATUS    RESTARTS   AGE
dask-1-9fa0db99-f5c98k           1/1     Running   0          2m48s
dask-1-9fa0db99-fthdf9           1/1     Running   0          3m9s
owl-api-686cdff988-75rkv         1/1     Running   0          19h
owl-scheduler-6cf6458857-65tkq   1/1     Running   0          19h
owlcli                           1/1     Running   0          63m
pipeline-1--1-nr228              1/1     Running   0          3m26s
```

The Dask scheduler is published using a service (by defaul using ClusterIP) that allows
for visualization of the Dask dashboard:

```bash
$ kubectl get services --namespace owl
NAME                TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
dask-1-9fa0db99-f   ClusterIP      10.103.131.231   <none>        8786/TCP,8787/TCP            57s
```

We can look at it by forwarding the port to the local host:

```bash
kubectl port-forward service/dask-1-9fa0db99-f 8787:8787
```

and directing the browser to http://localhost:8787.

{{< img src="dask.png" alt="Dask Dashboard" caption="<em>Dask Dashboard</em>" class="border-0" >}}