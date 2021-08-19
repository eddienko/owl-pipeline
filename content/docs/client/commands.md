---
title: "User Commands"
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

{{< alert icon="info" >}}
Make sure the `OWL_API_URL` environmental variable points to the public address of the Owl API service or
add the `--api` argument to all commands.
{{< /alert >}}

## Authentication

* Authenticate in the remote server
```bash
owl auth login
```

* Remove authentication token from local and remote server.
```bash
owl auth logout
```

* Change password.
```bash
owl auth change_password
```

## Pipeline definitions

* List available pipeline definitions in the system
```bash
owl pdef list
```

* Get pipeline definition for pipeline `example`
```bash
# Display in the console
owl pdef get example 

# Save to a file
owl pdef get example -o example.yaml
```

## Job submission and management

Commands to operate in jobs. Note that unless the user has admin privileges these
commands can only operate in the user's own jobs.

* Submit a Job. The command wil return the unique ID of the job.
```bash
owl job submit pipeline.yml
```

* List jobs. This will list the jobs for the current user. If user has admin
  privileges, the `-all` flag will return all jobs. The `--max` argument specifies 
  the maximum number of jobs to return and `--latest` sets the ordering. Pipelines
  in particular statuses can be retrieved with the `--status` flag.
```bash
owl job list [--all] [--max 10] [--latest] [--status RUNNING]
```

* Query status of a job. If `--json` is given, a detailed description
  in JSON is returned.
```bash
owl job status [--json] 1
```

* Cancel a job.
```bash
owl job cancel 1
```

* Inspect logs from job.
```bash
owl job logs [-f] 1
```

* Rerun a job (the job must be in status finished, error or cancelled).
```bash
owl job rerun 1
```