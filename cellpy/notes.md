# How to create a conda build

## Making us ready
First you will probably like to create a new environment:

```console
conda create -n conda_build
conda activate conda_build
```
Then you need to install stuff:

```console
conda install conda-build
conda install anaconda-client
conda install conda-verify
```

If you would like to upload your build to anaconda cloud:

```console
anaconda login
```
My username is `jepegit`

Several of the packages cellpy rely on are not available at the default
conda channel, but luckily, they are all at conda-forge:

```console
conda config --add channels conda-forge
```


## Building the cellpy conda package
Then it is time to edit the meta.yaml file and the bld.bat and the build.sh.
When you are satisfied, (and remember that you cannot add -pip: statements),
try to build it (this might be an iterative process involving some tweaking
of the meta.yaml file etc)

```console
conda build cellpy
```

Make a note of where it saves the tar.bz2 file.


## Making it available
Time to upload to anaconda cloud:

```console
anaconda upload /Users/jepe/miniconda3/envs/conda_build/conda-bld/osx-64/cellpy-0.2.1-py37h39e3cac_0.tar.bz2
```

For automatically uploading each time you build:

```console
conda config --set anaconda_upload yes
```

Convert and upload to other architectures:

```console
conda convert --platform all /Users/jepe/miniconda3/envs/conda_build/conda-bld/osx-64/cellpy-0.2.1-py37h39e3cac_0.tar.bz2 -o cellpy/dist/

anaconda upload cellpy/dist/win-32/*.bz2
anaconda upload cellpy/dist/win-64/*.bz2
anaconda upload cellpy/dist/linux-64/*.bz2
```

..etc

## Install and test

### Simplest way
create a conda environment:

```console
conda create -n check_cellpy
conda activate check_cellpy
conda config --add channels conda-forge
```

try to install (remember to add the channel):

```console
conda install -c jepegit cellpy
```

download the tests from github and run them (using pytest)


### The Docker way
create a docker container with miniconda installed

```console
docker pull continuumio/miniconda3
docker run -i -t continuumio/miniconda3 /bin/bash

conda config --add channels conda-forge
conda config --add channels jepegit
conda install cellpy

cellpy setup
cellpy version
cellpy configloc
```


