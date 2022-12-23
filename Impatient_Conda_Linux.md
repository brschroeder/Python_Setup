# For the Impatient: Conda + Classic Jupyter Notebook on Linux
These instructions are not intended for beginners!
* Install miniconda `bash ~/Downloads/Miniconda3-latest-Linux-x86_64.sh` (yes to conda init)
* `conda config --set auto_activate_base false`
* `conda config --add channels conda-forge`
* `conda config --show channels`
* `conda config --set channel_priority strict`
* `source ~/.bashrc` <<-- **IMPORTANT**
* `conda list` all packages should be from default channel
* `conda update --all`
* `conda list` to verify all packages from conda-forge
* `conda init` will update some files ~/miniconda3/
* `source ~/.bashrc` <<-- **IMPORTANT**
* `conda activate base`
* `conda deactivate` to test
* `conda activate base`
* `conda install conda-bash-completion`
* `conda deactivate`
* Edit ~/.bashrc to source the auto-completion scripts
```
    CONDA_ROOT=/home/brett/miniconda3
    if [[ -r $CONDA_ROOT/etc/profile.d/bash_completion.sh ]]; then
        source $CONDA_ROOT/etc/profile.d/bash_completion.sh
    fi
```
* `source ~/.bashrc` <<-- **IMPORTANT**
* `conda activate base` and test TAB auto-completion
* `conda deactivate`
* `conda install -y --name base notebook nb_conda_kernels widgetsnbextension jupyter_contrib_nbextensions autopep8`
* `conda create --name ds39 python=3.9 ipykernel ipywidgets autopep8`
* `conda activate ds39`
* `conda install numpy bottleneck numexpr pandas scipy scikit-learn matplotlib`
* `conda deactivate`
* `jupyter-notebook` and create new notebook file, should be able to select kernel from all other installed environments
* Switch to Nbextensions tab and enable these extensions
    * Autopep8 (requires package autopep8 to be installed in each environment)
    * Codefolding (requires CodeMirror mode extensions to be enabled)
    * Comment/Uncomment hotkey (works with default Alt-c, but NOT with Ctrl-/)
    * datestamper
    * Hide input AND hide input all
    * Highlight selected word
    * Hinterland for auto-completion
    * Variable Inspector
