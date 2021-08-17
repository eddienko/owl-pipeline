---
title: "Commands"
description: ""
lead: ""
date: 2021-08-15T17:02:41+01:00
lastmod: 2021-08-15T17:02:41+01:00
draft: false
images: []
menu: 
  docs:
    parent: "client"
weight: 403
toc: true
---

## Authentication

```bash
# Authenticate in the remote server
owl login
```

## Pipeline definitions

```bash
# List available pipelines
owl pdef list

# Get pipeline definition for pipeline `example`
owl pdef get example
```

## Job submission and management

```bash
# Submit job
owl submit pipeline.yml

# Query status of Job ID 1
owl status get 1

# Get logs from Job ID 1
owl logs get 1

# Cancel Job ID 1
owl cancel 1
```
