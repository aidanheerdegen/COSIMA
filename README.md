# COSIMA

This a meta project jmunroe's COSIMA work. You probably want one of the following projects instead:

cosima-cookbook

cosima-recipes


`git clone --recurse-submodules git@github.com:jmunroe/COSIMA.git`


start jupyterlab on gadi

conda activate cosima
jupyter lab --no-browser


open tunnel from local machine to gadi

ssh -L 8888:localhost:8888 -J gadi gadi-login-02 -l jm0634

open local webbrowser to localhost:8888

Using dask-labextenstion, set the path to:

/proxy/8787/status

(equivalently, http://localhost:8001/proxy/8787/status )

