---
title: "Admin Commands"
description: ""
lead: ""
date: 2021-08-15T17:02:44+01:00
lastmod: 2021-08-15T17:02:44+01:00
draft: false
images: []
menu: 
  docs:
    parent: "client"
weight: 404
toc: true
---

Some of the commands in the previous section operate slightly different if the
user issuing them has admin privileges or not. E.g. an admin user will be able
to cancel any running pipeline while normal users can only modify theirs.

This section lists specific administrative commands.

## Users

{{< alert icon="👉" >}}
The `admin user` commands should not be used if the user table is managed by other system,
e.g. Django.
{{< /alert >}}

* Add user
```bash
owl admin user add [--admin] [--active] [--no-active] [-p password] username
```

* Update user
```bash
owl admin user update [--admin] [--active] [--no-active] [-p password] username
```

* Delete user
```bash
owl admin user delete username
```

* List users
```bash
owl admin user list
```

* List one user
```bash
owl admin user get username
```

## Pipeline signatures

* Add or update a pipeline signature
```bash
owl admin pdef add pipedef.yaml
```

* Delete a pipeline signature
```bash
owl admin pdef delete name
```



## Set flags in the server

* Set maintance mode on/off. When the setting is on queued pipelines will not be started.
```bash
owl admin cmd '{"maintenance": "on"}'

owl admin cmd '{"maintenance": "off"}'
```

* Set maximum number of pipelines that can be in `RUNNING` state at the same time.
  Set to zero to disable.
```bash
owl admin cmd '{"maxpipe": 4}'
```

* Set scheduler heartbeat. This is the time between heartbeat checks (available pipelines, pipelines status, cluster status, etc). Set to zero for default as defined in the configuration.
```bash
owl admin cmd '{"heartbeat": 60}'
```
