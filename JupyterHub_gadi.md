# JupyterHub on Gadi

JupyterHub is a service that allows multiple users to run and manage Jupyter notebook environments on shared 
infrastructure such as gadi.  A full featured deployment would require hosting JupyterHub on a externally facing
login node and run as a process with superuser priviledges to be able to spawn Jupyter notebooks on
behalf of users.

In the implementation presented, we sidestep the need for a "superuser" by running JupyterHub as as process on
a gadi login node owned by the user.  Authenication and authorization will be handled by SSH public keys that
are set up per user and a secure connection is enforced by a SSH tunel from the user's local machine to gadi. 

Advantages of running "JupyterHub" instead of running "Jupyter notebook" directly by a user:
1. JupyterHub is very lightweight and likely could be left running for many days on a login node of gadi. It
is similiar to how many users presently use a `tmux` server to maintain the states of shells currently.  
2. Jupyter notebooks can be configured to run by JupyterHub. The resources required by a Jupyter notebook may be
selected by the user.
3. Only one external port needs to be tunneled over SSH. Any other ports required by a user are proxied by
JupyterHub

## Set up

- user has a NCI account on gadi.


