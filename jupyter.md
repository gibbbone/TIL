# Jupyter notebook

Disclaimer: not a complete list. I've already incorporated may things in my workflow and unless I'll forget everything I doubt I'll write them down again.

## Install Anaconda and create a new environment on Linux

First download and install Anaconda, running a first update.
```bash
# Download Anaconda
curl -O https://repo.anaconda.com/archive/Anaconda3-5.2.0-Linux-x86_64.sh
md5sum Anaconda3-5.2.0-Linux-x86_64.sh

# install Anaconda
# remember: add Anaconda to path during installation!!!
bash Anaconda3-5.2.0-Linux-x86_64.sh 

# update Anaconda
conda install libiconv
conda update conda

# Create environment
conda create -n env_2.7 python=2.7
```
Second we must ensure that our environment is up to date. 
The best thing to do is to get a requirement text from a previous working environment by doing `pip freeze > requirements.txt.` 

```bash
# Update environment:
# 1. get requirements from original env 
while read requirement; do conda install --yes $requirement; done < requirements.txt

# 2. otherwise update all modules without specifications
conda update --all 

# Install notebook extensions
#pip install igraph
conda install -c conda-forge jupyter_contrib_nbextensions
jupyter contrib nbextension install --user

# Check some packages
pip freeze | grep networkx
pip freeze | grep seaborn

# If packages are missing install them
conda install networkx
conda install seaborn
```

Finally launch the environment:

```bash
# Activate environment and launch server
source activate env_2.7
conda update --all 
nohup jupyter notebook --no-browser --port=8889 &
```

The options used when launching the notebook are specific to use when accessing notebook with the Windows Linux Subsystem (similar but not identical procedure is used to access notebooks on remote server). 

- `nohup` will allow us to use the command line after launching the notebook
- `--no-browser` will avoid the launch of a specific browser
- `--port=8889` is telling use to check http://localhost:8889 on our browser (in the Windows system) to get to the notebook.
