
## Important Anaconda Commands

### What is anaconda?

Anaconda is a free, easy-to-install package manager, environment manager, Python and R distribution, and collection of over 720 open source packages with free community support. 

* Some of the important modules of the anaconda framework includes,
    * Jupyter Notebook
    * Anaconda Navigator
    * Spyder
    * Anaconda CLI

Below are some of the important useful CLI commands which comes in handy while managing the anaconda framework. 

### Important Anacoda Commands


```python
# For opening jupyter notebook, anaconda navigator and Spyder
$ jupyter-noteook
$ anaconda-navigator
$ spyder
```


```python
# For checking anaconda version and installed details
$ conda info
```


```python
# Updating conda
$ conda update conda
```


```python
# Installing and Updating a package in anaconda
$ conda install <packagename>
$ conda install pandas
$ conda update <packagename>
$ conda update pandas
```


```python
# Commandline help
$ <commandname> --help
$ conda install --help
```


```python
# Searching for a package and installing through a channel
$ anaconda search -t conda <packagename>
$ anaconda search -t conda pandas
$ conda install -c <channelname> <packagename>
$ conda install -c ryan pandas
```


```python
# Listing all available environments
$ conda env list
```


```python
# Listing all the available libraries, revisions and restoring to a previous version in the current environment
$ conda list
$ conda list --revisions
$ conda install --revision 2
```


```python
# Creating a new environment
$ conda create -n <envname> <reqlibraries>
# Creating a env with python 3.5 and installing all the anaconda library packages
$ conda create -n python3 python = 3.5 anaconda 
# Creating a env with some specific libraries
$ conda create -n python3 python = 3.5 numpy pandas
# Activating a env
$ source activate python3
# Removing or deleting an environment
$ conda env remove -n <envname>
$ conda env remove python3
# Installing a library for a particular library
$ conda install <packagename> -n <envname>
$ conda install pandas -n python3
```

When you create a new python environment, the option for new notebook in the jupyter will be having the option of starting the kernel with any of the python environments.
