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

## Update admin password

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
curl -O https://raw.githubusercontent.com/eddienko/owl-pipeline-client/main/pipelines/signature_example.yaml
owl admin pdef add signature_example.yaml
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

## Query pipeline status

```bash
owl job status get 1
```