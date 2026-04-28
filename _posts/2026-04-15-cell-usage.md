---
# marp: true
title: cell
theme: default
paginate: true
# size: 4:3
layout: post
title: the usage of cell
date: 2025-03-26 14:24:00
description: the usage of cell server
tags: slurm 
categories: tutorial
pretty_table: true
---

# The usage of local server (cell)

---


# IP address

where you are in compus network (if not, you need to use VPN), you can connect the local server by:

```bash
ssh username@ip_address
```

---

# Modify `~/.ssh/config`

The recommand method is to write a section in `~/.ssh/config`

```
Host cell
    HostName ip_address
    User guest
```

Then you can login to the server by

```bash
ssh cell
```

---

# Passwordless

To avoid entering a password each time, you can generate a key pair using ssh-keygen

```bash
ssh-keygen
```

Then upload your public key to the server

```bash
ssh-copy-id cell
```

---

# Computational resources

There are four machines on the cell:

|    resource    | helix                                     | kinase | beta                          | serine                        |
| :--------| :-------------------------------------------: | :--------: | :-------------------------------: | -------------------------------: |
| CPU    | Intel(R) Xeon(R) Gold 6226R CPU @ 2.90GHz |   -     | Intel(R) Core(TM) i9-14900K   | Intel(R) Core(TM) Ultra 9 285K|
| CPU core | 32                                      |    -    | 24                            | 24                            |
| GPU    | NVIDIA RTX A4000 x 2                      |     -   | NVIDIA RTX 4000 Ada Generation x 1 | NVIDIA GeForce RTX 5090 x 1 |
| memory | 125Gi                                    |      -  | 62Gi                          | 62Gi                          |

---

# Task Management System

There are two Task Management System: AGE and SLURM. Although, all the resource managed by SLURM. It is recommanded to use AGE for `helix` and `kinase`, SLURM for `beta` and `serine`.

---

# AGE

job submission:

```bash
qsub job.sh
```

check of job status:

```bash
qstat
```

job cancellation:

```bash
qdel job_id
```

check resources

```
qhost
```

---

# SLURM

job submission:

```bash
sbatch job.sh
```

check of job status:

```bash
squeue
```

job cancellation:

```bash
scancel job_id
```

check resources

```
sinfo
```

---

# Test script

```bash
#!/usr/bin/env bash
#$ -S /bin/bash
#$ -cwd
#$ -pe mpi 1
#$ -q all.q@kinase.local
#$ -e stdout.age
#$ -o stdout.age
env
echo "hello, cell"
```

```bash
#!/usr/bin/env bash
#SBATCH -p beta
#SBATCH --cpus-per-task=1
#SBATCH -o stdout.slurm
env
echo "hello, cell"
```

---

# submit by command line

```bash
qsub -cwd -pe mpi 1 -q all.q@helix.local -o 'stdout.$JOB_NAME' -j y script.sh
```

