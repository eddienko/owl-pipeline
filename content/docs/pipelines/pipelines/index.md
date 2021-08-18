---
title: "Owl Pipelines"
description: ""
lead: ""
date: 2021-08-15T21:00:06+01:00
lastmod: 2021-08-15T21:00:06+01:00
draft: false
images: []
menu: 
  docs:
    parent: "pipelines"
weight: 501
toc: true
---

Owl pipelines are pip installable Python packages. In order to submit a pipeline
to the system, the admin must have activated it and you need a 
**pipeline definition file (PDeF)**. This is a YAML file that contains 
the name of the pipeline, its arguments and requested resources.

Currently there are three general purpose pipelines available.

## Example Pipeline

This is just a pipeline that uses Dask to run some dummy computations and
can be used for general testing and as a template for writing more
complicated pipelines.

```yaml
version: 1

# Name of the pipeline
name: example

# Pipeline arguments
datalen: 100

# Resources requested
resources:
  threads: 10
  workers: 2
  memory: 10
```

## Shell pipeline

The shell pipeline allows to execute an arbitrary command, typically a shell or a python script.

The pipeline definition file is below (and can be obtained typing `owl pdef get shell`):

```yaml
version: 1.2

# Name of the pipeline
name: shell

# Directory where the command is writing data to (optional)
# output_dir: /tmp/output

command: ["sleep", "300"]

# use_dask: false

resources:
  workers: 1
  memory: 8
  threads: 1
```

The `output_dir` is optional and if specified the sheduler will save a log file
and the configuration used to run the pipeline as well as a list of
environmental variables.

The `command` parameter defines which command or script to run. The example above
just waits for 5 minutes. Other command examples:

```yaml
# Execute a Python script
command: ["/opt/conda/bin/python", "script.py"]

# Execute a script that takes two arguments as input
command: ["/home/eglez/scripts/script.sh", "100", "200"]
```

Note that the path to the executable must be specified fully and the command is
a list containing all parts of the commands, i.e. the command ls -la would be
written as ["ls", "-la"].

The `use_dask` parameters needs a bit of explanation. In the default mode (false)
the script is run in only one worker with access to as much memory and cores
requested. Internally the (python) script can use Dask, multiprocessing,
multithreading or any other mechanism but the resources will be fixed to one
worker.

If use_dask is true then it makes sense to request more workers. It is required
the python script connects to the Dask scheduler as follows:

```python
import os
from distributed import Client

DASK_SCHEDULER = os.getenv("DASK_SCHEDULER_ADDRESS")
client = Client(DASK_SCHEDULER)
```

## Papermill pipeline

The papermill pipeline executes Jupyter notebooks using
[Papermill](https://papermill.readthedocs.io). Plase read the official
documentation on how to paramaterize the notebooks.

An example pipeline definition file is below (and can be obtained typing `owl
pdef get papermill`:

```yaml
version: 1.2

# Name of the pipeline
name: papermill

input_dir: /home/user/myDir
output_dir: /home/user/myDir
notebook: notebook.ipynb

paramaters: {}

# use_dask: false

resources:
  workers: 1
  memory: 8
  threads: 1
```

The pipeline requires the input and output directories and the name of the
notebook to execute. The executed notebook will be saved in the output directory
with the name `notebook_out.ipynb` in this example.

The parameters required to run the notebook are defined in parameters. 

The following shows an example output notebook that includes outputs and figures.

{{< img src="paperpipe.png" alt="Papermill Pipeline" caption="<em>Papermill Pipeline</em>" class="border-0" >}}