You need to install this package in the conda environment ON YOUR LAPTOP! <br>
`conda install -c conda-forge jupyter-forward` <br>
to get jupyter on theseus go to thesus account and use: <br>
`wget https://repo.anaconda.com/archive/Anaconda3-2021.11-Linux-x86_64.sh` <br>
this will download the installer. Then do: <br>
`sh Anaconda3-2021.11-Linux-x86_64.sh` <br>
Follow the instructions and say yes to everything. <br>
Then install jupyter in your conda environment of choice: <br>
`conda install jupyter` <br>
Then run this in that conda environment: <br>
`python -m ipykernel install --user --name=NAME_OF_CONDA_ENVIRONMENT` <br>
Now whenever you want to run jupyter notebooks from your laptop on theseus use: <br>
`jupyter-forward bharat@theseus.ornl.gov --port 8885` (or whatever port you work in) <br>