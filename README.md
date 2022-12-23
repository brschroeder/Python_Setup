# Setting up Python environments with Miniconda

NOTE: For the impatient and/or experienced, concise instructions are [here](Impatient_Conda_Linux.md)

Managing Python packages and environments can be complex due to a spiders web of, often, conflicting requirements, ever-changing interdependencies etc. Fortunantely there are some tools to make this easier
1. **Anaconda Python**: this is a large (~2GB) installation of a specific version of python along with many common data science packages. This is often the easiest for new users. However, managing different environments with different versions of Python and packages is still relatively complex and requires the user to learn the conda management tool if any package or python version outside of the baseline Anaconda ecosystem is need.
1. **Miniconda**: this is a bare bones version of python and the conda management tool. This approach is very lightweight but requires a certain level of understanding about python environments.
1. **Standard Python from PyPI and pip**: This has the same pros and cons as the miniconda approach. BUT it actually ends up being more complex than miniconda as outlined in several places e.g. [here](https://www.anaconda.com/blog/understanding-conda-and-pip), [here](https://pythonspeed.com/articles/conda-vs-pip/) and [here](https://stackoverflow.com/questions/20994716/what-is-the-difference-between-pip-and-conda)

## Recommended Approach
* Use Miniconda
* Use conda as both a package AND environment manager. A conda cheat sheet is [here](https://docs.conda.io/projects/conda/en/latest/_downloads/843d9e0198f2a193a3484886fa28163c/conda-cheatsheet.pdf)
* Create a base environment that is NOT used for any specific projects
* Create additional separate environments for specific projects and install into these environments only packages that are absolutely necessary.
* Use the base environment to host a Jupyter Notebook (or Jupyter Lab) environment. 
    * This means Jupyter only needs to be installed and configured once (in the base environment). Configure this notebook environment so that you can run Python and packages from **any** of the other environments on your system.
* Configure conda so that all environments pull packages from one repository only. 
    * Here we will use the _conda-forge_ channel instead of the Anaconda _defaults_ channel and configure our system to always pull from conda-forge. The defaults channel will be a fallback only i.e. no version of the desired pacakage exists in conda-forge.

## Initial Setup and Configuration

1. Install Miniconda as follows
    * `wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh`
    * `bash Miniconda3-latest-Linux-x86_64.sh`, say yes to conda init
1. Prevent the base environment from automatically activating conda `conda config --set auto_activate_base false`
1. Verify which channels are currently being used (should only be "defaults") `conda config --show channels`
1. Add the conda-forge channel `conda config --add channels conda-forge`
1. Verify conda-farge channel is included at top of the list `conda config --show channels`
1. Ensure that conda-forge is used if the package is available in defaults (even if there is a newer version in defaults) `conda config --set channel_priority strict`
1. All of conda's configuration parameters can be viewed with `conda config --show`

## Bash Auto-completion
1. Starting in v4.4 conda has dropped native Bash auto-completion
1. Third party packages now exist to enable equivalent functionality
1. Install as follows `conda install conda-bash-completion`
1. If the base environment is not activated by default then need to explicitly source the bash_completion.sh script from ~/.bashrc as follows
```
CONDA_ROOT=/home/brett/miniconda3
if [[ -r $CONDA_ROOT/etc/profile.d/bash_completion.sh ]]; then
    source $CONDA_ROOT/etc/profile.d/bash_completion.sh
fi
```

## Configure Base Environment: Part 1
Part 1 will update all packages in the base environment and ensure that they are source from the conda-forge channel
1. Switch to base environment with `conda activate base` and do `conda list`. Notice that all packages are from the defaults repository (the Channel column is blank)
1. Now lets update the entire base environment with `conda update --all` and then `conda list` will show that all packages come from conda-forge since it is higher in priority than defaults and we have set the channel priority to "strict"

## Configure Base Environment: Part 2
Part 2 will setup the base environment to host a (classic) Jupyter notebook from which any of the other environments can be accessed.
1. For Jupyter notebook to discover all other environments, while running out of the base environment, we need to install the package nb_conda_kernels into the base environment.
1. There is a nice meta collection of Jupyter notebook extensions which also comes with a GUI-based configurator that runs in the notebook itself. This pacakge is called jupyter_contrib_nbextensions
1. To enable use of widgets e.g. sliders, radio buttons etc, the package widgetsnbextension is required in the base environment (and ipywidgets is required in all environments)
1. One of the extensions will be used to format our code according to the rules of PEP8 but this requires the package autopep8 to be installed
1. Install all required packages into the base environment with `conda install notebook nb_conda_kernels jupyter_contrib_nbextensions widgetsnbextension autopep8`

## Create Other Environments
These environments will generally be project- or task-specific. For Jupyter Notebook, running from within the base environment, to discover each of these other environments; it is necessary to install ipykernel in each environment. Also ipywidgets is required to enable widgets within this environment when Jupyter notebook is running in the base environment.
1. Exit the base environment `conda deactivate`
1. Create the new environment `conda create --name ds39 python=3.9 ipykernel ipywidgets autopep8`
1. Switch to the new environment `conda activate ds39`
1. Install some useful packages `conda install numpy bottleneck numexpr pandas scipy scikit-learn matplotlib`

## First time use: configure Jupyter notebook
1. Start Jupyter notebook with 
    * `conda activate base`
    * `jupyter-notebook`
1. Change to the Nbextensions tab
1. Uncheck the box "disable configuration for nbextensions without explicit ..."
1. Enable the following extensions
    * Autopep8 (this will clean up code to be PEP8 compliant)
    * Codefolding (requires CodeMirror mode extensions to be enabled)
    * Comment/Uncomment hotkey (Ctrl-/ seems to work the same as in VS Code)
    * Datestamper (to automatically insert current date & time)
    * Hide Input AND Hide Input All (to collapse the code in a single or all cells respectively)
    * Highlight Selected Word
    * Hinterland (for code autocompletion)
    * Variable Inspector

## Removing Miniconda
1. Remove all downloaded, cached packages `conda clean --all`
1. There is a package called anaconda-clean that will remove various config files
1. Change into base environment `conda activate base`
1. Install the package `conda install -c anaconda anaconda-clean`
1. Run the cleaning application `anaconda-clean`
1. Exit base environment `conda deactivate`
1. Remove the entire conda directory `rm -fr ~/miniconda3`
1. Remove a local jupyter `rm -fr ~/.local/share/jupyter/`
1. Remove the backup folder created by anaconda-clean `rm -fr ~/.anaconda_backup`
1. Clean up ~/.bashrc
