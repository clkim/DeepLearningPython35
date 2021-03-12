## Running IEEE Boston Section class demo code
Introduction to Practical Neural Networks and Deep Learning (Part 1) March 20, 2021

###### Why using docker container
In order to avoid needing to give out separate instructions on how to install Python and needed packages to run the book's code on different platforms/flavors (such as Mac, Windows, Linux),
it seemed easier to just give one set of instructions on how to create a _docker_ container and how to run the demo code in it.
Hopefully, docker is sufficiently ubiquitous nowadays so that installing and running docker on different platforms should be well documented. 

#### How to: clone github repository, run docker container, run demo
Clone _github_ repository locally at command line, then  
`cd` into repo directory, then  
`git checkout` branch that has desired setup of demo code you want to run.


Acknowledgement: The repository is forked from _DeepLearningPython35_ repository of _Michal Daniel Dobrzanski_ who ported the book's code from Python 2.7 to Python 3.5 and wrote the "orchestrator" testing file `test.py`
```
~ $ git clone https://github.com/clkim/DeepLearningPython35.git
~ $ cd DeepLearningPython35
~/DeepLearningPython35 $ git checkout chap1_30-hidden-neurons-3.0-eta
```
To run the desired setup of demo code, "uncomment in" or "comment out" as appropriate the code in `test.py` in order to specify the neural network and deep learning configuration to run.

To see an example of the (flexible but somewhat hackish and minimalist) changes I made in `test.py` in order to run the demo in the chap1 branch, at command line run  
`git diff ea229ac 6ba2425`  
to see the small changes committed in the branch.


Run _Docker Desktop_ app locally (currently 3.0.3 for Mac).

At repo directory, start `docker` container _deeplearning_ (see below for how to create container).  
In container shell `cd` into the mounted directory (points to local _DeepLearningPython35_ directory, which should be already on the desired git branch, e.g. chap1_30-hidden-neurons-3.0-eta).
```
~/DeepLearningPython35 $ docker container ls --all
< Should see a table with column names CONTAINER ID ... NAMES, and container named deeplearning >

~/DeepLearningPython35 $ docker container start -ai deeplearning
(base) root@xxx:/# cd deeplearn/
(base) root@xxx:/deeplearn# python --version
Python 3.8.5
(base) root@xxx:/deeplearn# conda info --env
< Should see two environments: base and nndlbook >
(base) root@xxx:/deeplearn# conda activate nndlbook

(nndlbook) root@xxx:/deeplearn# python3.8 test.py
Epoch 0 : 8943 / 10000
Epoch 1 : 9166 / 10000
Epoch 2 : 9267 / 10000
Epoch 3 : 9340 / 10000
Epoch 4 : 9337 / 10000
Epoch 5 : 9374 / 10000
Epoch 6 : 9386 / 10000
< On my late-2013 MacBook Pro, it takes about a minute to finish Epoch 6; control-C to break >
< Each epoch run uses all training images; then neural network is evaluated on test images >

(nndlbook) root@xxx:/deeplearn# exit
exit
~/DeepLearningPython35 $
```
#### How to create docker container
(We want to mount a directory so that the demo Python code on our computer is accessible from inside the container; and we want to install _Numpy_ package and _Theano_ package.)

Run _Docker Desktop_ app locally (currently 3.0.3 for Mac).

First run `docker` to download _miniconda3_ image.  
Then run `docker` to create the container and install the packages.
```
~ $ docker pull continuumio/miniconda3
Using default tag: latest
...
docker.io/continuumio/miniconda3:latest

~ $ docker images
< Should see a table with column names REPOSITORY ... SIZE, and a row with image repository/name continuumio/miniconda3 >

< Now cd into the repo directory, after cloning repo from github as given above >
< Then run docker to create a new container layer over the downloaded image, then create a new conda environment in which we install needed packages >
~ $ cd DeepLearningPython35
~/DeepLearningPython35 $ docker run -it --name deeplearning --mount type=bind,source="$(pwd)",target=/deeplearn continuumio/miniconda3

(base) root@xxx:/# conda --version
conda 4.9.2
(base) root@xxx:/# conda create --name nndlbook
Collecting package metadata (current_repodata.json): done
...
Proceed ([y]/n)? y
...
# To activate this environment, use
#
#     $ conda activate nndlbook
...

(base) root@xxx:/# conda activate nndlbook
(nndlbook) root@xxx:/# conda list
# packages in environment at /opt/conda/envs/nndlbook:
#
# Name                    Version                   Build  Channel
(nndlbook) root@xxx:/# python --version
Python 3.8.5

(nndlbook) root@x:/# conda install numpy
Collecting package metadata (current_repodata.json): done
...
Proceed ([y]/n)? y
...
...
Executing transaction: done

(nndlbook) root@xxx:/# conda install theano
Collecting package metadata (current_repodata.json): done
...
Proceed ([y]/n)? y
...
...
Executing transaction: done

(nndlbook) root@xxx:/# conda list
< Should see list of packages including numpy and theano >

(nndlbook) root@xxx:/# exit
exit
~/DeepLearningPython35 $ docker container ls --all
< Should see a table with column names CONTAINER ID ... NAMES, and container named deeplearning >
```
___

## Overview

### neuralnetworksanddeeplearning.com integrated scripts for Python 3.5.2 and Theano with CUDA support

These scrips are updated ones from the **neuralnetworksanddeeplearning.com** gitHub repository in order to work with Python 3.5.2

The testing file (**test.py**) contains all three networks (network.py, network2.py, network3.py) from the book and it is the starting point to run (i.e. *train and evaluate*) them.

## Just type at shell: **python3.5 test.py**

In test.py there are examples of networks configurations with proper comments. I did that to relate with particular chapters from the book.


