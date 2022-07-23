# Use Multiple Virtual Environments with Poetry, Pyenv, VScode and Code-Runner in Windows 10

These are my notes while learning to use multiple virtual environments with Poetry, Pyenv, VScode and Code-Runner in Windows 10.

## Install pyenv

> Open Windows PowerShell as administrator  
> [Follow these instruction](https://github.com/pyenv-win/pyenv-win#power-shell)  
> Ignore error message:
>
> > pyenv-win was not installed successfully. If this issue persists, please open a ticket: https://github.com/pyenv-win/pyenv-win/issues.
>
> Reload VSCode
>
> From VScode terminal, check installation:
>
> > \>pyenv
>
> Following should be in your Windows System Environment Variables:
>
> > | Variable   | Value                                |
> > | ---------- | ------------------------------------ |
> > | Path       | C:\Users\ping\.pyenv\pyenv-win\bin   |
> > |            | C:\Users\ping\.pyenv\pyenv-win\shims |
> > | PYENV      | C:\Users\ping\.pyenv\pyenv-win\      |
> > | PYENV_HOME | C:\Users\ping\.pyenv\pyenv-win\|     |
> > | PYENV_ROOT | C:\Users\ping\.pyenv\pyenv-win\|     |

## Use pyenv to install python 3.7.9 and 3.9.0

> [Pyenv-win usage](https://github.com/pyenv-win/pyenv-win#usage)  
> From terminal:
>
> > \>pyenv install 3.7.9  
> > \>pyenv install 3.9.0
>
> These folders should be your Windows Users directory after installation:
>
> > C:\Users\ping\.pyenv\pyenv-win\versions\3.7.9  
> > C:\Users\ping\.pyenv\pyenv-win\versions\3.9.0

## Install poetry

> Python is needed to install poetry, set global python version from terminal:
>
> > \>pyenv global 3.7.9
>
> Check python version:
>
> > \>python  
> > \>exit()
>
> [Poetry Windows PowerShell Install Instructions ](https://python-poetry.org/docs/)
>
> > Open PowerShell:
> >
> > > \>(Invoke-WebRequest -Uri https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py -UseBasicParsing).Content | python -
>
> Reload VScode  
> Check poetry installation:
>
> > \>poetry

## Configure poetry

> Create the virtual environment in the .venv folder inside the project’s root folder:
>
> > \>poetry config virtualenvs.in-project true
>
> Check poetry configuration:
>
> > \>poetry config --list

## [Poetry bug: "expected string or bytes-like object" on poetry install](https://github.com/python-poetry/poetry/issues/3628)

> What worked for me:
>
> > poetry env list --full-path  
> > in my case, path was ~\.venv (Activated).  
> > rename folder to ~\.venv*
> > poetry update
> > delete folder ~\.venv* after poetry update

## Create virtual environment for python 3.7.9 with specific version of pandas and numpy

> [Poetry documentation](https://python-poetry.org/docs/)  
> Poetry uses python to create the virtual environment for python. The two python versions should be the same.
>
> Create python 3.7.9 virtual environment:
>
> > From terminal, change to your project folder  
> > Set default python version:
> >
> > > \>pyenv global 3.7.9
> >
> > Create new project py379:
> >
> > > \>poetry new py379
> >
> > Change to newly created project folder py379:
> >
> > > \>cd py379
> >
> > Open folder's pyproject.toml
> >
> > > Replace entries in \[tool.poetry.dependencies] with:
> > >
> > > > python = "3.7.9"  
> > > > pandas = "1.3.5"  
> > > > numpy = "1.21.6"  
> > > > matplotlib = "^3.5.2"  
> > > > ipykernel = "^6.15.0"
> > >
> > > Replace entries in \[tool.poetry.dev-dependencies] with:
> > >
> > > > pytest = "^5.2"  
> > > > flake8 = "^4.0.1"  
> > > > black = "^22.3.0"
> > >
> > > Save pyproject.toml  
> > > From terminal:
> > >
> > > > \>poetry install
> > >
> > > After installation:
> > >
> > > > python 3.7.9 is installed in folder:
> > > >
> > > > > ~\py379\\.venv\Scripts
> > > >
> > > > project dependencies are installed in folder:
> > > >
> > > > > ~\py379\\.venv\Lib\site-packages
> > >
> > > Currently activated virtual environment information:
> > >
> > > > \>poetry env info

## Create virtual environment for python 3.9.0

> From terminal, change to your project folder  
> Set default python version:
>
> > \>pyenv global 3.9.0
>
> Create new project py390:
>
> > \>poetry new py390
>
> Change to newly created project folder py3.9.0:
>
> > \>cd py390
>
> Install dependencies:
>
> > \>poetry add numpy pandas matplotlib ipykernel
>
> Install development dependencies:
>
> > \>poetry add flake8 black \--dev

## Use virtual environments for python 3.7.9

> Test python file:
>
> > In folder py379, create a new file test_venv.py with the codes in [test_venv](#test_venv)  
> > Open Command Palette  
> > Select python interpreter in ~\py379\\.venv\Scripts\python.exe  
> > Run test_venv.py with code-runner(Ctrl+Alt+N)
> > Output's python, pandas and numpy versions should match versions in pyproject.toml
>
> Test jupyter notebook file:
>
> > In folder py379, create a new file test_venv.ipynb and paste codes in [test_venv](#test_venv) into a cell  
> > Click the icon (looks like a phone) on the upper right and the select kernel in ~\py379\\.venv\Scripts\python.exe  
> > Run test_venv.ipynb
> > Output's python, pandas and numpy versions should match versions in pyproject.toml

## Use virtual environments for python 3.9.0

> Same process as "Use virtual environments for python 3.7.9", except:
>
> > Create test_venv.py and test_venv.ipynb in folder py390  
> > Select python interpreter in ~\py390\\.venv\Scripts\python.exe  
> > Select kernel in ~\py390\\.venv\Scripts\python.exe

## Remove python 3.9.0 virtual environment and the associated import modules

> Delete folder py3.9.0

## test_venv

> '''An example file that imports some of the installed modules'''  
> import sys  
> import pandas as pd  
> import numpy as np  
> import matplotlib.pyplot as plt  
> import flake8  
> from platform import python_version
>
> print("python_path: ", sys.executable)  
> print("python_version: ", python_version())  
> print("pandas_version: ", pd.\_\_version\_\_)  
> print("numpy_version: ", np.\_\_version\_\_)  
> if \_\_name\_\_ == "\_\_main\_\_":  
> &nbsp;&nbsp;&nbsp;&nbsp;# If the modules can't be imported, the following print won't happen  
> &nbsp;&nbsp;&nbsp;&nbsp;print("Successfully imported the modules!")

---

## Setup Code-Runner (if used in VScode)

> Open Code-Runner.executorMap by clicking on:
>
> > Extensions icon (Ctrl+Shift+X) on Left panel  
> > Code-Runner gear icon  
> > Extension Settings  
> > Edit in settings.json
>
> Paste following inside code-runner.executorMap:
>
> > "python": "$pythonPath \$fullFileName",
> >
> > > e.g.:
> > >
> > > > "code-runner.executorMap": {
> > > > "python": "$pythonPath \$fullFileName",
> > > > },

---

## Uninstall poetry

> Requirement:
>
> > python
> > get-poetry.py
>
> From terminal, navigate to directory of get-poetry.py:
>
> > \>python get-poetry.py \--uninstall  
> > \>Are you sure you want to uninstall Poetry? (y/[n]) y
>
> From File Explorer, delete pypoetry folders in AppData:
>
> > e.g.:
> >
> > > \>C:\Users\ping\AppData\Local\pypoetry
> > > \>C:\Users\ping\AppData\Roaming\pypoetry

## Uninstall python with pyenv:

> From terminal, check python versions installed:
>
> > \>pyenv versions  
> > \* 3.10.0 (set by C:\Users\ping\.pyenv\pyenv-win\version)  
> > &nbsp;&nbsp;&nbsp;3.7.9  
> > &nbsp;&nbsp;&nbsp;3.8.0  
> > &nbsp;&nbsp;&nbsp;3.9.0
>
> Remove python versions:
>
> > \>pyenv uninstall 3.10.0  
> > \>pyenv uninstall 3.7.9  
> > \>pyenv uninstall 3.8.0  
> > \>pyenv uninstall 3.9.0

## Uninstall pyenv:

> In Edit_system_environment_variables>users_variables, delete:
>
> > | Variable   | Value                                |
> > | ---------- | ------------------------------------ |
> > | Path       | C:\Users\ping\.pyenv\pyenv-win\bin   |
> > |            | C:\Users\ping\.pyenv\pyenv-win\shims |
> > | PYENV      | C:\Users\ping\.pyenv\pyenv-win\      |
> > | PYENV_HOME | C:\Users\ping\.pyenv\pyenv-win\|     |
> > | PYENV_ROOT | C:\Users\ping\.pyenv\pyenv-win\|     |
>
> Delete \\.pyenv folder in Users directory:
>
> > e.g. folder:
> >
> > > C:\Users\ping\\.pyenv
