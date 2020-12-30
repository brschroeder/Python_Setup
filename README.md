# Setting up Python environments with Miniconda

As an alternative to a full-blown Anaconda installation, a more lightweight environment can be created with only the necessary packages using Miniconda. 

**NOTE** The biggest challenge here is managing packages, which repository they originate from and compatibility among packages from different repositories. Here we will use the _conda-forge_ channel instead of the Anaconda _default_ channel.

## Setup and Configuration

1. Install Miniconda from as follows
    * `wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh`
    * `bash Miniconda3-latest-Linux-x86_64.sh`
1. Prevent the base environment from automatically activating conda `conda config --set auto_activate_base false`
1. Create a completely empty environment (will not even have python) `conda create --name ds00`
    * To  create the environment with a specific version of Python do (**NOTE** this will install Python from the *default* repository) `conda create --name ds00 python=3.8.2`
1. Activate the new environment `conda activate ds00`
1. Add conda-forge as the first channel `conda config --env --add channels conda-forge`
1. Ensure that conda-forge is used if the package is available (even if there is a newer version in defaults) `conda config --env --set channel_priority strict`
1. Install packages, example here is for basic data science environment `conda install pandas scikit-learn matplotlib notebook numexpr bottleneck`

## Installing from other channels
1. This setup will first search conda-forge channel and then defaults if the package is not found in conda-forge
1. If the requested package cannot be found in defaults, then can install it from another channel with `-c` option e.g. to install plotly_express from the plotly channel `conda install -c plotly plotly_express`

## Bash Auto-completion
1. Starting in v4.4 conda has dropped native Bash auto-completion
1. Third party packages exist but they are not available in in Anaconda *default* channel which is used for base environment
1. A third party package exists in conda-forge but installing this will then also upgrade some existing packages in the base environment. **NOTE** the base environment will then no longer be exclusively sourced from the *default* Anaconda channel
1. Install as follows `conda install -c conda-forge conda-bash-completion`
1. On Fedora it is necessary to <TAB> twice
1. If the base environment is not activated by default then need to source the bash_completion.sh script from ~/.bashrc as follows
    * `CONDA_ROOT=/home/brett/miniconda3`
    * `if [[ -r $CONDA_ROOT/etc/profile.d/bash_completion.sh ]]; then`
    * `source $CONDA_ROOT/etc/profile.d/bash_completion.sh`
    * `fi`

## Sanity check Jupyter Notebooks
1. Need to ensure that Jupyter Notebooks execute the kernel from the active environment
1. A User kernel takes precedence over the kernel in active environment
1. In a Jupyter Notebook check which kernel is being executed by doing 
    * `import sys`
    * `sys.executable`
1. On the command line a list of available kernels is obtained with `jupyter kernelspec list`
1.  There is really no need for a User kernel in an active environment, so just remove it with `jupyter kernelspec remove python3`  

## Removing Miniconda
1. There is a package called anaconda-clean that will do most of this automatically
1. Change into base environment `conda activate base`
1. Install the package `conda install anaconda-clean`
1. Run the cleaning application `anaconda-clean`
1. Remove the entire conda directory `rm -fr ~/miniconda3`
1. Clean up ~/.bashrc
1. Remove the backup folder created by anaconda-clean `rm -fr ~/.anaconda_backup`
