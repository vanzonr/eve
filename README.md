# eve - Easier virtual environment management

## Rationale

On many multi-user compute clusters, Python software is provided
through a combination of environment modules and python packages to be
installed in user's virtual environments.

The `eve` package provides a command for the bash shell that makes it
easier to create, manage and delete such combinations of virtual
environments and modules collections.  In addition, it helps make
these environments available in Jupyter Notebook and Jupyter Lab
environments.

## Installation

  - Copy the files `initeve` and `venv2jup` to a place in the PATH.
  - Make sure `venv2jup` is executable.
  - In an initialization scripts, e.g. `.bashrc`, add the line "`source initeve`".

The latter command defines the `eve` function. 

## Typical usage

### Create an environment
```
module load python scipy-stack # load the modules you need
eve create myenv scalene       # create a virtual environment with those modules, and install the python package 'scalene'
```
The environment is automatically activated.

To deactivate the environment:
```
eve deactivate
```

To (re)activate the environment:
```
eve myenv
```
(The longer version `eve activate myenv` works as well). Activating an environment with `eve` also loads the modules.

### Add Python Packages

When the environment is active, simply use pip, e.g.
```
pip install line_profiler
```

### Add Environment Modules

After an environment is created and is active, you can still load modules still, but you have to let `eve` know that they should become part of the environment with `eve module save`, e.g.
```
module load StdEnv mpi4py
eve module save
```

### Manage Virtual Environments

To see all virtual environments managed by eve, type
```
eve list
```

To remove a virtual environment  use
```
eve remove myenv
```

To know what is installed in one of the environments, use
```
eve describe myenv
```

## Command Overview

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
