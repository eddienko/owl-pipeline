---
title: "Storage"
description: ""
lead: "Customizing storage and providing permanent storage."
date: 2021-08-15T16:56:40+01:00
lastmod: 2021-08-15T16:56:40+01:00
draft: false
images: []
menu: 
  docs:
    parent: "server"
    identifier: "server-storage"
weight: 304
toc: true
---


## Scheduler

By default a PVC is created that stores the SQLite database and logs from the pipelines. This is mounted
in `/var/run/owl`.

TBD.

## Pipeline jobs

Storage provided to the pipeline will be mounted by the pipeline and all Dask workers. For this reason
it needs to be able to be mounted in ReadWriteMany mode.

### Option 1: hostPath

If we have somewhere a NFS server and *all the nodes have access to the same mounted shares* we can
use `hostPath` to define user storage. In this case we would have the following in the Helm `values.yaml`:

```yaml
pipeline:
  extraVolumeMounts:
    - name: user-storage
      mountPath: /storage/${USER}
  extraVolumes:
    - name: user-storage
      hostPath:
        path: /storage/${USER}
        type: Directory
```

If user `joe` submits a pipeline, it is expected that the all nodes have access to `/storage/joe`, mounted from 
the NFS server with write permissions for the user id 1000 (the default id of the user running the pods). The pipeline and Dask pods then mount this directory inside each container under the same mount point `/storage/joe`.

{{< alert icon="info" text="The creation of the directories needed for this to work can be automated by a script at the same time a user is added to the database." />}}

### Option 2: NFS

Another option is using as well an external NFS server but instead of `hostPath` creating the PV and PVC manually as nfs type. In this case again is necesary ghat the `/storage/joe` directory is already available in the NFS share with the right permissions.

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: nfs-storage-joe
  labels:
    username: joe
spec:
  storageClassName: nfs-direct
  capacity:
    storage: 10Mi
  accessModes:
    - ReadWriteMany
  nfs:
    path: /storage/joe
    server: 192.168.100.10
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: nfs-storage-joe
spec:
  storageClassName: nfs-direct
  accessModes:
    - ReadWriteMany
  selector:
    matchLabels:
      username: joe
  resources:
    requests:
      storage: 10Mi
```

Then in the Heml `values.yaml` we set:

```yaml
pipeline:
  extraVolumeMounts:
    - name: user-storage
      mountPath: /storage/${USER}
  extraVolumes:
    - name: nfs-storage
      persistentVolumeClaim:
        claimName: nfs-storage-${USER}
```

### Option 3: Ceph

Using Ceph. TBD.