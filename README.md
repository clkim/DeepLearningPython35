## Running IEEE Boston Section class demo code
## Introduction to Practical Neural Networks and Deep Learning (Part 1)<br>September 18, 2021
Instructions are given below for each of the five steps:
* download Git software
* download Docker software
* clone GitHub respository containing the demo code into local directory
* create a Docker container locally to run the demo Python code
* run the demo Python code in the created Docker container

##### Why use Docker container
In order to avoid potential problems with installing Python and the needed packages to run the book's demo code on different platforms such as Mac, Windows, or Linux,
we decided to create a _Docker_ container and to run the demo code in it.
Docker is sufficiently popular nowadays so that installing as well as running Docker on different platforms should be well supported and documented. 

### How to download Git software
Git is a very popular source code management tool for version control, widely used among software professionals.

Estimated time: 15 mins.  
Go to [Install Git](https://www.atlassian.com/git/tutorials/install-git), and scroll to Install Git on \[Mac OS X | Windows | Linux\] section as appropriate.  
Unless you have a preference, probably just pick the first method for your platform.
 
(For Mac:  
Git for Mac Installer > pick latest link (currently `git-2.31.0-intel-universal-mavericks.dmg`);  
if you're installing from a downloaded .dmg file, you may be blocked from opening the installation package; if you see
"macOS cannot verify that this app is free from malware", go to _System Preferences_ > _Security & Privacy_ > click `Open Anyway` for that downloaded git-xxx.pkg file.  
Install just Git; you should not need to install git-credential-osxkeychain helper.)

### How to download Docker software
In software engineering parlance, a _container_ packages up code and all its dependencies into a standard unit of software so that the application can run
quickly and reliably from one computing environment to another.
Docker is a very popular container technology. The containers run on _Docker Engine_.

Estimated time: 15 mins.  
Go to [Get Docker](https://docs.docker.com/get-docker/), and pick Docker \[Desktop for Mac | Desktop for Windows | for Linux\] to do the appropriate install.  
For Mac and Windows, after installing Docker Desktop, find the application icon to run the _Docker_ app (_Docker Desktop_), so as to start _Docker Engine_;
it could take about half a minute to start; then you could minimize or close the Docker _Dashboard_ window but verify that _Docker Desktop_ is still running.  
For Linux, install _Docker Engine_ then start _Docker_.

### How to clone GitHub repository into local directory
Estimated time: 10 mins.  
The commands shown in the text area below do the following; the text area also shows the _Terminal_ console response to the commands:  
- Clone with `git clone https://github.com/...` the specified _GitHub_ repository at a Terminal command line;
this downloads the demo source code from that specified repository into your local computer
- `cd DeepLearningPython35` into the repository directory; this changes your directory to the directory of the downloaded demo Python source code
- Verify with `git branch` that you are on the _master_ branch of the repository; the branch you are on is marked with an asterisk (*);
a repository can have many versions of the source code, each stored in its own branch
- Checkout the desired branch instead of master branch, with `git checkout chap1_30-hidden-neurons-3.0-eta`;
that specific branch has the desired setup of demo code you want to run for this Part 1 class
- Verify with `git branch` again that you are on the desired branch _chap1_30-hidden-neurons-3.0-eta_ which is now marked with an asterisk (*)
```
~ $
~ $ git clone https://github.com/clkim/DeepLearningPython35.git

~ $ cd DeepLearningPython35

~/DeepLearningPython35 $ git branch
  chap1_30-hidden-neurons-3.0-eta
  chap2_fully-matrix-based-backpropagation-mini-batch
  chap6
* master

~/DeepLearningPython35 $ git checkout chap1_30-hidden-neurons-3.0-eta

~/DeepLearningPython35 $ git branch
* chap1_30-hidden-neurons-3.0-eta
  chap2_fully-matrix-based-backpropagation-mini-batch
  chap6
  master
~/DeepLearningPython35 $
```
(Skip until class) To run the desired setup of demo code, "uncomment in" or "comment out" as appropriate the code in _test.py_ in order to specify
the neural network and deep learning configuration to run.

(Skip until class) To see an example of the (flexible but somewhat hackish and minimalist) changes I made in _test.py_ in order to run the demo
in the chap1 branch, at command line run  
`git diff ea229ac 6ba2425`  
to see the small changes committed in the branch  
(red text is deleted, green text is added; hit space bar once to scroll down one page;
when we see `(END)` of document, enter q to quit and get back to the command line).

Acknowledgement: The repository is forked from the _DeepLearningPython35_ repository of _Michal Daniel Dobrzanski_ who ported the book's code from
Python 2.7 to Python 3.5 and wrote the "orchestrator" testing file _test.py_.

### How to create a Docker container locally to run the demo Python code
Background: We want to set up a "bind" type of mount in the container whose source is the directory in our local computer where the demo Python
source code has been cloned from GitHub, in order that the source code on our local computer would be accessible from inside the container.  
We also want to install two Python packages, _Numpy_ package and _Theano_ package, in the container we want to create.

Ensure that you have already started Docker Engine, e.g. by running _Docker_ app (_Docker Desktop_) locally;  
and that you have already cloned the demo Python source code from GitHub, into the _DeepLearningPython35_ directory,
as described above in "How to clone GitHub repository into local directory".

`cd` into the directory _DeepLearningPython35_ if not already there.

Estimated time: 20 - 30 mins.  
The commands shown in the text area below do the following; the text area also shows the _Terminal_ console response to the commands:
- First run `docker pull continuumio/miniconda3` to download the _miniconda3_ image, which contains _conda_, a small version of Anaconda
which is a very popular data science platform
- Then run `docker images` to verify the image _continuumio/miniconda3_ is downloaded  
- Then run the given `docker` command to create a new container layer over the downloaded image
  - At the interactive shell command line inside the container, we check the conda version, with `conda --version`
  - Then create our own environment, named _nndlbook_ for our use, with `conda create --nndlbook`
  - Then we activate this new _nndlbook_ conda environment, with `conda activate nndlbook`
  - Next, do a confirming check that no additional packages are installed yet, with `conda list`
  - And do a check that we do have python installed already, with `python --version`
  - Now, we are ready to install our packages, with `conda install numpy` and then `conda install theano`
  - Finally, we do a sanity check that we see those two package names, among others, with `conda list`
  - Then we exit our newly created local container, with `exit`
- Back at the Terminal console, verify that we have created a local container named _deeplearning_, with `docker container ls --all`
```
~/DeepLearningPython35 $
~/DeepLearningPython35 $ docker pull continuumio/miniconda3
Using default tag: latest
...
docker.io/continuumio/miniconda3:latest

~/DeepLearningPython35 $ docker images
REPOSITORY               TAG       IMAGE ID       CREATED         SIZE
continuumio/miniconda3   latest    xxxxxxxxxxxx   n months ago    nnnMB

~/DeepLearningPython35 $ docker run -it --name deeplearning --mount type=bind,source="$(pwd)",target=/deeplearn continuumio/miniconda3

(base) root@xxx:/# conda --version
conda 4.9.2

(base) root@xxx:/# conda create --name nndlbook
Collecting package metadata (current_repodata.json): done
...
Proceed ([y]/n)? y
...
# To activate this environment, use
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
< Should see a container named "deeplearning" >
CONTAINER ID   IMAGE                    COMMAND       CREATED      ...       NAMES
xxxxxxxxxxxx   continuumio/miniconda3   "/bin/bash"   xxx                    deeplearning
```

### How to run the demo Python code in the created Docker container
Estimated time: 10 mins.  
Ensure that you have already started Docker Engine, e.g. by running _Docker_ app (_Docker Desktop_) locally;  
and that you have already cloned the demo Python source code from GitHub, into the _DeepLearningPython35_ directory, as described above
in "How to clone GitHub repository into local directory".

`cd` into the directory _DeepLearningPython35_ if not already there.

You must be on the branch _chap1_30-hidden-neurons-3.0-eta_.  
Verify with `git branch` (see section on "How to clone GitHub repository into local directory").  
If not, do `git checkout chap1_30-hidden-neurons-3.0-eta` to switch to that branch, then verify with `git branch`.

The commands shown in the text area below do the following; the text area also shows the _Terminal_ console response to the commands:
- First, just verify we see the newly created container named _deeplearning_
- At _DeepLearningPython35_ directory, start `docker` container _deeplearning_ and specifying option to attach an interactive shell,
  with `docker container start -ai deeplearning`
  - At the interactive shell command line inside the container, we `cd` into the _deeplearn_ directory mounted into the container;
  when we created the container, we had bind that mount to the local _DeepLearningPython35_ directory,
  which must be already on the git branch _chap1_30-hidden-neurons-3.0-eta_
  - We check the python version, with `python --version`
  - And verify what conda environments we have, with `conda info --env` 
  - Then we activate our own previously created conda environment _nndlbook_, with `conda activate nndlbook`
  - Finally, we now run the demo code in _test.py_, with `python3.8 test.py`; use control-c to break out of the run if desired
  - After the run, we exit the container, with `exit`
- Now we should be back at the Terminal console, in the _DeepLearningPython35_ directory
```
~/DeepLearningPython35 $
~/DeepLearningPython35 $ docker container ls --all
< Should see a container named "deeplearning" >
CONTAINER ID   IMAGE                    COMMAND       CREATED      ...       NAMES
xxxxxxxxxxxx   continuumio/miniconda3   "/bin/bash"   xxx                    deeplearning

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
< On my late-2013 MacBook Pro, it takes about a minute to finish Epoch 6; use control-c to break if desired >
< Each epoch run uses the training images; then neural network is evaluated on test images >

(nndlbook) root@xxx:/deeplearn# exit
exit
~/DeepLearningPython35 $
```
## End of Running IEEE Boston Section class demo code: Introduction to Practical Neural Networks and Deep Learning (Part 1)
___

## Overview

### neuralnetworksanddeeplearning.com integrated scripts for Python 3.5.2 and Theano with CUDA support

These scrips are updated ones from the **neuralnetworksanddeeplearning.com** gitHub repository in order to work with Python 3.5.2

The testing file (**test.py**) contains all three networks (network.py, network2.py, network3.py) from the book and it is the starting point to run (i.e. *train and evaluate*) them.

## Just type at shell: **python3.5 test.py**

In test.py there are examples of networks configurations with proper comments. I did that to relate with particular chapters from the book.


