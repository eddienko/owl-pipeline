---
title: "Shell"
description: "The Shell pipeline allows to submit arbitrary commands."
lead: "The Shell pipeline allows to submit scripts as shell commands."
date: 2021-08-15T20:55:31+01:00
lastmod: 2021-08-15T20:55:31+01:00
draft: false
images: []
menu:
  showcase:
    parent: "browse"
    identifier: "shell-pipeline"
weight: 10
toc: false
link: https://github.com/eddienko/owl-shell-pipeline
---

## Install

```bash
curl -O https://raw.githubusercontent.com/eddienko/owl-shell-pipeline/main/shell_pipeline/signature.yaml
owl admin pdef add signature.yaml
```

## Pipeline definition file

The following pipeline definition file example executes a shell script in the cluster.

```yaml
version: 1.2

name: shell

command: |
  #!/bin/bash
  echo "hello" > hello
  sleep 600
  cat hello

# optional
# use_dask: false

# output directory (optional)
# - sets the directory where the script is run
# - stores pipeline logs
# output: /storage/user/output

resources:
  workers: 1
  memory: 2
  cores: 2
```

The `command` argument is anything that can be executed in a shell script, or indeed
a Python script if the shebang line `#!/usr/bin/env python` is used. Other example commands:

```yaml
command: |
    #!/usr/bin/env python

    import random
    print(random.randint(1, 10))
```

The `use_dask` argument needs a bit of explanation. In the default mode (false)
the script is run in only one worker with access to as much memory and cores
requested. Internally the (python) script can use Dask, multiprocessing,
multithreading or any other mechanism but the resources will be fixed to one
worker.

If use_dask is true then it makes sense to request more workers. It is assumed then
that the command is a Python script that connects to the Dask scheduler as follows:

```python
import os
from distributed import Client

DASK_SCHEDULER = os.getenv("DASK_SCHEDULER_ADDRESS")
client = Client(DASK_SCHEDULER)
```