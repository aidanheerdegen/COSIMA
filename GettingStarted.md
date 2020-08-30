# COSIMA Cookbook

The Cookbook is configured for analysis of models on the NCI platform. In partcular, it suports

- [COSIMA Cookbook](#cosima-cookbook)
  - [SSH](#ssh)
  - [Connecting to `gadi`](#connecting-to-gadi)
  - [Connect to the VDI](#connect-to-the-vdi)
    - [hello](#hello)
    - [list-avail](#list-avail)
    - [launch](#launch)
    - [terminate](#terminate)
    - [get-host](#get-host)
    - [start-pending](#start-pending)
    - [has-started](#has-started)
  - [Install conda on /g/data/v45/conda](#install-conda-on-gdatav45conda)

## SSH

We will use SSH

Create a key pair.

`ssh-keygen`

NCI Policy requires the use of passphrase.  Set up and use ssh-agent as well.

## Connecting to `gadi`

Copy this key over to `gadi` so that we can log in with out a password.

`ssh-copy-id`

Add the following to `.ssh/config`

```ssh-config
Host gadi
  Hostname gadi.nci.org.au
  User jm0634
```

We can connect to `gadi` using

`ssh gadi`

without a password.

From here, we can use the ./gadi_jupyter script to get a Jupyter Notbook session started.  May need to wait a few minutes.

Gadi Cascade Lake nodes have 48 CPUs and 192 GB memory.

### PBS


Should the dask-scheduler run on compute node of gadi or on the vdi?

qsub -I -q express -l mem=32GB -l jobfs=0 -l ncpus=8 -l walltime=0:30:00 -l storage=gdata/v45


## Connect to the VDI

The VDI officially supports connecting to a graphical interface using "ScienTific Remote User DEsktop Launcher" or Strudel for short. Orginally this was used for setting up a VNC connection. While we do not need the graphical connection (VNC), we do need to connect to the VDI using the same backend that Strudel uses. For the VDI, this is the script `/opt/vdi/bin/session-ctl`.

The login node `vdi.nci.org.au` only supports running the script `session-ctl` and it is use to launch a VDI session for the user.

(Note: the script `session-ctl` on the compute node and log in are not the same, even though they have the same name).

```bash
usage: session-ctl [-h] [--configver CONFIGVER]

{launch,terminate,get-passwd,list-avail,get-host,start-pending,hello,has-started}
```

The only CONFIGVER appears to be 20151620513 so it may be optional.

### hello

```text
usage: session-ctl hello [-h] --partition PARTITION

optional arguments:
  -h, --help            show this help message and exit
  --partition PARTITION
```

The PARTITION is main.  This subcommand appears to check that we can log into the VDI server.

### list-avail

```text
usage: session-ctl list-avail [-h] --partition PARTITION
```

Give a list of any existing VDI sessions by job id and the remaining time for each session. A VDI session will run for 7 days before it is automaticaly terminated.

### launch

```text
usage: session-ctl launch [-h] --partition PARTITION [--geometry GEOMETRY]
```

Create a new VDI session.  This will return a jobid that will be used for later commands.

### terminate

```text
usage: session-ctl terminate [-h] --jobid JOBID

optional arguments:
  -h, --help     show this help message and exit
  --jobid JOBID
```

Stop a VDI session by jobid

### get-host

```text
usage: session-ctl get-host [-h] --jobid JOBID
```

Returns the VDI compute node number used by the session associated with JOBID.

### start-pending

```text
usage: session-ctl start-pending [-h] --jobid JOBID

optional arguments:
  -h, --help     show this help message and exit
  --jobid JOBID
```

Presumably returns an exit code if the JOBID has been put on the queue.

### has-started

```text
usage: session-ctl has-started [-h] --jobid JOBID

optional arguments:
  -h, --help     show this help message and exit
  --jobid JOBID
```

Returns and message and error code of the JOBID is currently running.

## Install conda on /g/data/v45/conda

`wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh`

Install to `/g/data/v45/conda`

conda init

conda create -n cosima

conda install

jupyterlab
dask
xarray
matplotlib
netcdf4
dask-jobqueue

tmux can be used to keep jupyter running even if the ssh connection is dropped.

tmux new-session -d -s jupyter "juptyer lab --no-browser"

How to find a free port?

<https://stackoverflow.com/questions/28989069/how-to-find-a-free-tcp-port>

netstat lists ports in use

But we can also let Jupyter choose a free port.

Find running notebook servers?

> jupyter notebook list

> Look in ~/.local/share/jupyte/runtime

> This allows us to look up the port and the token!

How to stop a notebook server?

> jupyter notebook stop 8890

We can connect from the VDI to gadi. That means we should be able to run dask jobs there.

Still can't see /scratch from VDI but we can see everything from /g/data

gadi data mover: gadi-dm.nci.org.au

How should dask-jobqueue be used? The scheduler should be on gadi.

How does nbserverproxy work ?

What about dask-labextension?

qsub -I -q express -l mem=32GB -l jobfs=0 -l ncpus=8 -l walltime=0:30:00 -l storage=gdata/v45

