---
layout: default 
title: Posts/Introduction to NERSC/Cori
theme: gaia
_class: lead
paginate: true
backgroundColor: #fff
backgroundImage: url('https://marp.app/assets/hero-background.svg')
marp: true
size: :

---
source: https://docs.nersc.gov/

---

##  System Specification

|System Partition |	Processor |	Clock Rate |Physical Cores Per Node |Threads/Core 	|Sockets Per Node |	Memory Per Node|
|----------------|----------|-----------|-----------------------|--------------|----------------|---------------|
|Login 	|Intel Xeon Processor E5-2698 v3 	|2.3 GHz 	|32 	|2 	|2 |	515 GB|
|*Haswell* 	|Intel Xeon Processor E5-2698 v3 |	2.3 GHz |	32 |	2 |	2 |	128 GB|
|*KNL* |	Intel Xeon Phi Processor 7250 |	1.4 GHz |	68| 	4 |	1 |	96 GB (DDR4), 16 GB (MCDRAM)|
|Large Memory |	AMD EPYC 7302 |	3.0 GHz |	32 |	2 	|2 	|2 TB|

---

Node Specifications
Login Nodes
- Cori has 12 Login nodes (cori[01-12]) open to public.
- 2 Large Memory Login nodes (cori[22,23]) to submit to bigmem qos. These nodes have 750GB of memory.
-    4 Jupyter nodes (cori[13,14,16,19]]) access via Jupyter
-   2 Workflow nodes (cori[20,21]) - requires approval before access to node
-    1 Compile node (cori17) - requires approval before access to node
-    Each node has two sockets, each socket is populated with a 2.3 GHz 16-core Haswell processor.

---

## Accessing Cori
`ssh -X <user>@cori.nersc.gov`

Password Prompt: Your set password + OTP

To set up OTP: follow these [instructions](https://docs.nersc.gov/connect/mfa/)
- You want to follow steps in **Creating and Installing a Token**.
- I use Google Authenticator app to see my TOTP.

---

##  Data Transfer Nodes
- The data transfer nodes are NERSC servers dedicated to performing transfers between NERSC data storage resources such as HPSS and the NERSC Global File System (NGF), and storage resources at other sites. 
- Access: `dtn0[1-4].nersc.gov` or via Globus
- For smaller files you can use Secure Copy (`scp`) or Secure FTP (`sftp`) or rsync to transfer files between two hosts.

---

## Cori SCRATCH
Cori scratch is a Lustre file system designed for high performance temporary storage of large files. It is intended to support large I/O for jobs that are being actively computed on the Cori system.

### Usage
The `/global/cscratch1` file system should always be referenced using the environment variable `$SCRATCH` (which expands to `/global/cscratch1/sd/YourUserName`). The scratch file system is available from all nodes and is tuned for high performance.

---

### Quotas
If your `$SCRATCH` usage exceeds your quota, you will not be able to submit batch jobs until you reduce your usage. The batch job submit filter checks the usage of `/global/cscratch1`.

Note that the quota on the Community File System and on Global Common is shared among all members of the project, so `showquota`/`cfsquota` will report the aggregate project usage and quota.

---
### Quotas

|File system 	|Space| 	Inodes |	Purge time |	Consequence for Exceeding Quota|
|----|-----|------|----|-----|
|Community 	|20 TB 	|20 M |	- 	|No new data can be written|
|Global HOME |	40 GB |	1 M |	- |	No new data can be written|
|Global common |	10 GB 	|1 M |	- 	|No new data can be written|
|Cori SCRATCH |	20 TB 	|10 M |	12 weeks |	Can't submit batch jobs|

---

## Slurm (Running Jobs)

NERSC uses [Slurm](https://slurm.schedmd.com/) for cluster/resource management and job scheduling. Slurm is responsible for allocating resources to users, providing a framework for starting, executing and monitoring work on allocated resources and scheduling work for future execution.

### Additional Resources

-    Documentation: https://slurm.schedmd.com/documentation.html
-    Tutorial: https://slurm.schedmd.com/tutorials.html
-    Manual: https://slurm.schedmd.com/man_index.html
-    FAQ: https://slurm.schedmd.com/faq.html

---

## Submitting jobs
### `sbatch`

`sbatch` is used to submit a job script for later execution. The script will typically contain one or more `srun` commands to launch parallel tasks.

When you submit the job, Slurm responds with the job's ID, which will be used to identify this job in reports from Slurm.

``` 
$ sbatch first-job.sh
Submitted batch job 864933 
```

---

### `salloc`

`salloc` is used to allocate resources for a job in real time as an interactive batch job. Typically this is used to allocate resources and spawn a shell. The shell is then used to execute `srun` commands to launch parallel tasks.

---

### `srun`

`srun` is used to submit a job for execution or initiate job steps in real time. A job can contain multiple job steps executing sequentially or in parallel on independent or shared resources within the job's node allocation. This command is typically executed within a script which is submitted with `sbatch` or from an interactive prompt on a compute node obtained via `salloc`.

How to write `sbatch`? [Link](https://docs.nersc.gov/jobs/) 
How to request for `interactive node`? [Link](https://docs.nersc.gov/jobs/interactive/)

---


## Sample scripts (Bharat)

### Public Webpage

path : `cd /project/projectdirs/m2467/www/bharat/` <br>
webpage : `https://portal.nersc.gov/project/m2467/`

If you are not able to see your files just run the following command on the terminal:
`chmod 755 * `

---

### Submitting a parallel job

```
#!/bin/bash -l
#SBATCH --qos=premium
#SBATCH --nodes=6
#SBATCH --ntasks-per-node=32
#SBATCH --time=02:00:00
#SBATCH --license=SCRATCH  
#SBATCH --job-name='calculation of anomalies of GPP for CESM2 First run'
#SBATCH --account=m2467
#SBATCH --output=o.log
#SBATCH --error=e.log
#SBATCH --mail-user dk28nov@gmail.com
#SBATCH -C haswell
echo 'Hello world!'

srun -n 192 --mpi=pmi2 python calc_anomalies_ssa_mpi.py -src 0 -var pr
```
---

### requesting an interactive node

`salloc -N 1 -C haswell -q interactive -t 04:00:00`

To avoid HDF errors, type the following on the terminal of interactive node:
`export HDF5_USE_FILE_LOCKING=FALSE`

---

### SCRATCH
`$SCRATCH` or `cd /global/cscratch1/sd/bharat`

### CMIP6 Data
`/global/cfs/cdirs/m3522/cmip6/CMIP6`

### Project Community File System (CFS), use this to share data with other members
`/global/cfs/cdirs/m2467`

---

## Backups

### Snapshots
Global homes and Community use a snapshot capability to provide users a seven-day history of their directories. Every directory and sub-directory in global homes contains a ".snapshots" entry.

- `.snapshots ` is invisible to `ls`, `ls -a`, find and similar commands
-    Contents are visible through `ls -F .snapshots`
-    Can be browsed normally after `cd .snapshots`
-    Files cannot be created, deleted or edited in snapshots
-    Files can only be copied out of a snapshot

---

## JupyterHub

JupyterHub provides a multi-user hub for spawning, managing, and proxying multiple instances of single-user Jupyter notebook servers. At NERSC, you authenticate to the JupyterHub instance we manage using your NERSC credentials and one-time password. Here is a link to NERSC's JupyterHub service: https://jupyter.nersc.gov/

---

# <!-- fit --> Thank you!
 
[Notes](https://sharma-bharat.github.io/Posts_Online/Intro_to_NERSC.html)