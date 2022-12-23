# Python Environments

## Standard Python (PIP and venv)
* Create a new environment
    * In current dir: `python -m venv env_name`
    * New environment name is part of path: `python -m venv /path/to/new/env_name`
* Activate an environment
    * Windows command shell: `C:> <venv>\Scripts\activate.bat`
    * Windows PowerShell: `PS C:> <venv>\Scripts\Activate.ps1`
    * Linux Bash shell: `$ source <venv>/bin/activate`
* Package installation (after activating env): `pip install package_name`
* Deactivate an evironment
    * Same in all(?) shells: `deactivate`
* __NOTE__: When using the venv module, it is not possible to create a virtual environment with any version of Python, the version in virtual environment will be the same as the base environment. There is package (virtualenv) that allows for creation of virtual environments with python versions other than the base. However, even this still requires that this version of Python already exist on your system ... that's just another reason conda is better!

## CONDA on Windows
* Download the latest miniconda installer from [here](https://docs.conda.io/en/latest/miniconda.html)
* Double-click to start the installation (accept all the defaults, ensure that the installation type is __not__ for all users but "just for me")
* Shortcuts to command prompts are installed on the Windows Start menu under "Anaconda". 
    * NOTE: the PowerShell prompt does not work on some corporate laptops (since the program is not digitally signed etc.).
    * Use the one called "Anaconda Prompt"
* Go to the command prompt and exit the base environment (`conda deactivate`) __<<-- IMPORTANT__
* Add the conda-forge channel and remove the default Anaconda channel called "defaults" (as of mid-2021, Anaconda licensing terms require Enterprise licenses to use the "defaults" channel...we don't have those licenses, so we won't use the defaults channel)
    * `conda config --add channels conda-forge`
    * `conda config --remove channels defaults`
    * `conda config --show channels`
* Disable SSL verification since this often fails when we are behind a corporate firewall!
    * `conda config --set ssl_verify no`
* Prevent base environment from activating by default BUT on Windows if use icon on Start menu the command shell startup script actually activates the base environment anyway. The effect of this is that any other command prompt (not the Anaconda one) will not have the base environment activated
    * `conda config --set auto_activate_base false`
* These config changes are stored in ~/.condarc
* Update the entire base environment so that all packages come from conda-forge repository
    * `conda activate base`
    * `conda update --all`
* It is not necessary to install Jupyter into all environments. Easiest approach is to install either Jupyter Notebook or Jupyter Lab into the base environment only. Then other environments can be launched from within Notebook itself. From this point, instructions are conceptually the same as on Linux.