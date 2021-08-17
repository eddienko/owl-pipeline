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

Use the password to login and change the admin password:

```bash
# Use the initial password
owl login

# Update the password
owl admin user update --admin admin yourNewPassword

# Login with the new password
owl login
```

New users can be added with

```bash
owl admin user add [--admin] username password
```

## Add pipeline signatures

```bash
# Example pipeline
owl admin pdef add http://.....
```

Check that they available:

```bash
# List all signatures
owl pdef list

# Get pipeline definition for example pipeline
owl pdef get example
```
