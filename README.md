# Better virtual environment management

## Installation

  - Copy `modeve` and `venv2jup` to a place in the PATH.
  - Make sure `venv2jup` is executable.
  - In initialization scipts (e.g. .bashrc), add `source modeve`

The latter defines a `eve` function. 

## Usage:

 `eve help|--help|-h`                  # show his help message

 `eve list`                            # list available virtual environments

 `eve activate ENVNAME`                # activate a virtual environment

 `eve create ENVNAME [PACKAGE ...]`    # create and activate a virtual environment

 `eve deactivate [ENVNAME]`            # deactivate a virtual environment
 
 `eve remove [ENVNAME]`                # remove a virtual environment

 `eve module save`                     # preserve currently loaded modules in environment

## Jupyter lab integration

The `eve` function automatically makes your virtual environments visible in jupyerhub by using the `venv2jup` script when creating or eve-activating the environment. The environment will also have inherited certain variables from the environment (the ones in the EXPORVARS variable).  In particular, `venv2jup` automates the following steps:

 1. Deduces the virtual environment name from the VIRTUAL_ENV 
    environment variable.

 2. Install ipykernel in the virtual environment, which allows 
    it to operate as a kernel.

 3. Register the virtual environment as a kernel in 
    ~/.local/share/jupyter/kernels/<NAME>/kernel.json

 4. Adds environment variables to that kernel.json file to 
    reproduce the current environment.

Note: while `eve` does not support conda environments, the venv2jup utility does and can be used to add conda environments to as a kernel to jupyter lab by activating that environment and running `venv2jup`.

## File Locations

  - Virtual environments will be stored in $HOME/.virtualenvs unless the $EVEROOT variable is set to point to another directory.
  
  - The jupyter kernels are installed in ~/.local/share/jupyter/kernels/<NAME>/kernel.json
