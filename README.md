## Running IEEE Boston Section class demo code
## Introduction to Neural Networks and Deep Learning (Part 1)<br>March 16, 2024
Instructions are given below for each of the five steps:
* download Git software
* download Docker software
* clone GitHub respository containing the demo code into local directory
* create a Docker container locally to run the demo Python code
* run the demo Python code in the created Docker container

##### Why use Docker container
In order to avoid potential problems with installing Python and the needed packages to run the book's demo code on different platforms such as Mac, Windows, or Linux,
we decided to create a _Docker_ container and to run the demo code in it.  
In software engineering parlance, a _container_ packages up code and all its dependencies into a standard unit of software so that the application can run
anywhere, as long as the container engine supports the underlying operating system.  
Docker is sufficiently popular nowadays so that installing as well as running Docker on different platforms should be well supported and documented.
Personal and small business use is still free, though a sign-up for a Docker account may be required.

### How to download Git software
Git is a very popular source code management tool for version control, widely used among software professionals.

Estimated time: 15 mins.  
Go to [Install Git](https://github.com/git-guides/install-git), and scroll to Install Git on \[Windows | Mac | Linux\] section as appropriate.  
Suggestion: unless you have a preference, look into the link labelled `git-scm` in the Windows and Mac sections, but also see note on Mac below.
 
(For Mac:  
The link labelled `macOS Git Installer` seems quite old and no longer updated.  
Most MacOS will already have Git installed; even though the version is likely to be old, it is probably sufficient for our purpose here.  
If you want the latest version, use the link [git-scm](https://git-scm.com/download/mac) and follow instructions; we suggest using Homebrew.)  

### How to download Docker software
Docker is a very popular container technology. The containers run on _Docker Engine_.

Estimated time: 15 mins.  
Go to [Get Docker](https://docs.docker.com/get-docker/), and pick Docker Desktop for \[Mac | Windows | Linux\] to do the appropriate install.  
After installing Docker Desktop, find the application icon to run the _Docker_ app (_Docker Desktop_), so as to start _Docker Engine_;
it could take about half a minute for _Docker Desktop_ window to start and open; then you could minimize or close that window (double check that _Docker Desktop_
is still running).

##### Public Service Announcement
It looks like my installed Docker Desktop 4.24.0 (122432) for Mac has an issue tracked here
[Docker does not recover from resource saver mode](https://github.com/docker/for-mac/issues/6933); see my work-around below.  
If you are using Mac but not on macOS Monterey (version 12) or later, it seems that Docker Desktop 4.25.0 is not available, so try downloading 4.24.x and do my work-around.
Otherwise, download the latest version (currently 4.28.0) and hopefully the issue has been fixed.  
I'm staying on Docker Desktop 4.24.0 since my Mac is on Big Sur (version 11); will be getting a new Mac soon :)  
 
The workaround for me is: As soon as Docker Desktop starts, open Settings (wheel icon on top right) > left menu > Resources | Advanced
>Scroll down to Resource Saver > unset Enable Resource Saver > click Apply & restart button.  
>Scroll up to Resource Allocation | CPU limit > instead of default 8, reduce to number of cores on your Mac (4 on mine) > click Apply & restart button.  
>Click Cancel button to exit Settings.

### How to clone GitHub repository into local directory
Estimated time: 10 mins.  
The commands shown in the text area below do the following; the text area also shows the _Terminal_ console response to the commands:  
- Clone with `git clone https://github.com/...` the specified _GitHub_ repository at a Terminal command line;
this downloads the demo source code from that specified repository into your local computer
- `cd DeepLearningPython35` into the repository directory; this changes your directory to the directory of the downloaded demo Python source code
- Verify with `git branch` that you are on the _master_ branch of the repository; the branch you are on is marked with an asterisk (*);
a repository can have many versions of the source code, each stored in its own branch
- Checkout the desired branch instead of _master_ branch, with `git checkout chap1_30-hidden-neurons-3.0-eta`;
that specific branch has the desired setup of demo code you want to run for this Part 1 class
- Verify with `git branch` again that you are on the desired branch _chap1_30-hidden-neurons-3.0-eta_ which is now marked with an asterisk (*)
- Use `ls -l` to see the files in the directory
```
~ $
~ $ git clone https://github.com/clkim/DeepLearningPython35.git

~ $ cd DeepLearningPython35

~/DeepLearningPython35 $ git branch
  chap1_30-hidden-neurons-3.0-eta
* master

~/DeepLearningPython35 $ git checkout chap1_30-hidden-neurons-3.0-eta

~/DeepLearningPython35 $ git branch
* chap1_30-hidden-neurons-3.0-eta
  master

~/DeepLearningPython35 $ ls -l
total 158088
-rw-r--r--   1 clkim  staff    492526 Feb 29  2020 MyNetwork
-rw-r--r--   1 clkim  staff     14338 Mar 10 17:04 README.md
...
...
-rw-r--r--   1 clkim  staff       770 Feb 29  2020 mnist_svm.py
-rw-r--r--   1 clkim  staff      6398 Mar 11  2021 network.py
-rw-r--r--   1 clkim  staff     15252 Feb 29  2020 network2.py
-rw-r--r--@  1 clkim  staff     13000 Feb 29  2020 network3.py
-rw-r--r--   1 clkim  staff      7394 Mar  8 23:34 test.py
~/DeepLearningPython35 $
```
(Skip until class) To run the desired setup of demo code, "uncomment in" or "comment out" as appropriate the code in _test.py_ in order to specify
the neural network and deep learning configuration to run.

(Skip until class) To see an example of the (flexible but somewhat hackish and minimalist) changes I made in _test.py_ in order to run the demo
in the chap1 branch, at command line run  
`git diff ea229ac 6ba2425`  
to see the small changes committed in the branch  
(red is for text deleted, green is for text added; hit space bar once to scroll down one page;
when you see `(END)` of document, enter _q_ to quit and get back to the command line prompt).

Acknowledgement: The repository is forked from the _DeepLearningPython35_ repository of _Michal Daniel Dobrzanski_ who ported the book's code from
Python 2.7 to Python 3.5 and wrote the "orchestrator" testing file _test.py_.

### How to create a Docker container locally to run the demo Python code
Background: We want to set up a `bind mount` in the container whose source is the directory in our local computer where the demo Python
source code has been cloned from GitHub, in order that the source code on our local computer would be accessible from inside the container.  

Ensure that you have already started Docker Engine by running _Docker_ app (_Docker Desktop_) locally;  
and that you have already cloned the demo Python source code from GitHub, into the _DeepLearningPython35_ directory,
as described above in "How to clone GitHub repository into local directory".

You must be at the _DeepLearningPython35_ directory, because the `bind mount` being set up into the container calls `pwd` to get name of the current directory.  
`cd` into the directory _DeepLearningPython35_ if not already there.

Estimated time: 15 mins.  
The commands shown in the text area below do the following; the text area also shows the _Terminal_ console response to the commands:
- First run `docker pull continuumio/miniconda3:yy.x.x-x` to download the _Miniconda_ image (based on Python 3.X), a minimal installer for Python and _conda_, a package manager as well as
an environment manager tool; Miniconda is a small version of Anaconda, which is a very popular data science platform; the download source is the Docker hub [docker-miniconda](https://hub.docker.com/r/continuumio/miniconda3)
- Then run `docker image ls` to verify the image _continuumio/miniconda3_ (with Tag _yy.x.x-x_) is downloaded  
- Now run the given `docker` command to create a new container layer over the downloaded image
  - At the interactive shell command line inside the new container, we can look for the conda version, with `conda --version`
  - (May be prompted to update to a new version of conda; if so, go ahead and follow the prompts to update)
  - Double-check that the Python packages we want to install are not present, and that there is only the `base` (python virtual) environment
  - Create and install in a new environment: Python 3.6 (latest that supports Theano), and _Numpy_ and _Theano_, with `conda create -n py36numpytheano python=3.6 numpy theano`
    - Double-check we have the newly created environment named `py36numpytheano`, with `conda env list` 
  - Activate the newly created environment, with `conda activate py36numpytheano`
  - Do a `ls` to look for the `bind mount` target directory `deeplearn` that we named when creating the new container, then do a `cd` to change into that directory
  - Now do a `ls`, we should see the files in the _DeepLearningPython35_ directory
  - Do a sanity check that we will be running the python version we installed in the new environment, with `python --version`
  - Ok, exit our newly created local container, with `exit`
- Back at the Terminal console, verify that we have created a local container named _deeplearning_, with `docker container ls --all`
```
~/DeepLearningPython35 $
~/DeepLearningPython35 $ docker pull continuumio/miniconda3:24.1.2-0
24.1.2-0: Pulling from continuumio/miniconda3
...
docker.io/continuumio/miniconda3:24.1.2-0

~/DeepLearningPython35 $
~/DeepLearningPython35 $ docker image ls
REPOSITORY               TAG       IMAGE ID       CREATED         SIZE
continuumio/miniconda3   24.1.2-0  xxxxxxxxxxxx   nn .... ago     nnnMB

~/DeepLearningPython35 $
~/DeepLearningPython35 $ docker run -it --name deeplearning --mount type=bind,source="$(pwd)",target=/deeplearn continuumio/miniconda3:24.1.2-0

(base) root@xxx:/#
(base) root@xxx:/# conda --version
conda 24.1.2
(base) root@xxx:/# python --version
Python 3.11.7

(base) root@xxx:/#
(base) root@xxx:/# conda list | grep numpy
(base) root@xxx:/# conda list | grep theano
(base) root@xxx:/# conda env list
# conda environments:
#
base                  *  /opt/conda


(base) root@xxx:/# 
(base) root@xxx:/# conda create -n py36numpytheano python=3.6 numpy theano
Channels:
 - defaults
Platform: linux-64
Collecting package metadata (repodata.json): done
Solving environment: done

## Package Plan ##

  environment location: /opt/conda/envs/py36numpytheano

  added / updated specs:
    - numpy
    - python=3.6
    - theano

The following packages will be downloaded:
...
...
The following NEW packages will be INSTALLED:
...
...
Proceed ([y]/n)? y

Downloading and Extracting Packages:

Preparing transaction: done
Verifying transaction: done
Executing transaction: done
#
# To activate this environment, use
#
#     $ conda activate py36numpytheano
#
# To deactivate an active environment, use
#
#     $ conda deactivate


(base) root@xxx:/#
(base) root@xxx:/# conda env list
# conda environments:
#
base                  *  /opt/conda
py36numpytheano          /opt/conda/envs/py36numpytheano

(base) root@xxx:/#
(base) root@xxx:/# conda activate py36numpytheano

(py36numpytheano) root@xxx:/#
(py36numpytheano) root@xxx:/# ls
bin  boot  deeplearn  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
(py36numpytheano) root@xxx:/#
(py36numpytheano) root@xxx:/# cd deeplearn/
(py36numpytheano) root@xxx:/deeplearn# 
(py36numpytheano) root@xxx:/deeplearn# ls
MyNetwork  __pycache__      mnist.pkl.gz               mnist_expanded.pkl.gz  mnist_svm.py  network2.py  test.py
README.md  expand_mnist.py  mnist_average_darkness.py  mnist_loader.py        network.py    network3.py

(py36numpytheano) root@xxx:/deeplearn# 
(py36numpytheano) root@xxx:/deeplearn# python --version
Python 3.6.13 :: Anaconda, Inc.
(py36numpytheano)
(py36numpytheano) root@xxx:/deeplearn# exit
exit

~/DeepLearningPython35 $
~/DeepLearningPython35 $ docker container ls --all
CONTAINER ID   IMAGE                              COMMAND       CREATED      ...       NAMES
xxxxxxxxxxxx   continuumio/miniconda3:24.1.2-0    "/bin/bash"   xxx                    deeplearning
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
- At _DeepLearningPython35_ directory, start `docker` container _deeplearning_ and specify option to attach an interactive shell,  
  with `docker container start -ai deeplearning`
  - At the interactive shell command line inside the container, activate the environment we created, with `conda activate py36numpytheano`
  - We can use `ls` to see the directories at the root directory;  
  then `cd` into the _deeplearn_ directory mounted into the container;
    - When we created the container, we had bind that mount to the local _DeepLearningPython35_ directory,
    which must be already on the git branch _chap1_30-hidden-neurons-3.0-eta_
  - We can see the files in our local _DeepLearningPython35_ directory, including _test.py_, with `ls`
  - We can double-check the python version, with `python --version`
  - Now, we can run the demo code in _test.py_, with `python3.6 test.py`
    - On my late-2013 MacBook Pro, it takes about 10s - 15s to complete first Epoch 0, about a minute to finish Epoch 5
    - Each epoch run uses the training images; then neural network is evaluated on the 10000 test images
  - Use control-c to break out of the run as desired
  - After the run, we exit the container, with `exit`
- Now we should be back at the Terminal console, in the _DeepLearningPython35_ directory
```
~/DeepLearningPython35 $
~/DeepLearningPython35 $ docker container ls --all
CONTAINER ID   IMAGE                              COMMAND       CREATED      ...       NAMES
xxxxxxxxxxxx   continuumio/miniconda3:24.1.2-0    "/bin/bash"   xxx                    deeplearning

~/DeepLearningPython35 $
~/DeepLearningPython35 $ docker container start -ai deeplearning

(base) root@xxx:/#
(base) root@xxx:/# conda activate py36numpytheano

(py36numpytheano) root@xxx:/#
(py36numpytheano) root@xxx:/# ls
bin  boot  deeplearn  dev  etc	home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var

(py36numpytheano) root@xxx:/#
(py36numpytheano) root@xxx:/# cd deeplearn/

(py36numpytheano) root@xxx:/deeplearn#
(py36numpytheano) root@xxx:/deeplearn# ls
MyNetwork  __pycache__	    mnist.pkl.gz	       mnist_expanded.pkl.gz  mnist_svm.py  network2.py  test.py
README.md  expand_mnist.py  mnist_average_darkness.py  mnist_loader.py	      network.py    network3.py

(py36numpytheano) root@xxx:/deeplearn#
(py36numpytheano) root@xxx:/deeplearn# python --version
Python 3.6.13 :: Anaconda, Inc.

(py36numpytheano) root@xxx:/deeplearn#
(py36numpytheano) root@xxx:/deeplearn# python3.6 test.py
Epoch 0 : 8943 / 10000
Epoch 1 : 9166 / 10000
Epoch 2 : 9267 / 10000
Epoch 3 : 9340 / 10000
Epoch 4 : 9337 / 10000
Epoch 5 : 9374 / 10000
Epoch 6 : 9386 / 10000
...
< Use control-c to break out of the run as desired >

(py36numpytheano) root@xxx:/deeplearn#
(py36numpytheano) root@xxx:/deeplearn# exit
exit
~/DeepLearningPython35 $
```
## End of Running IEEE Boston Section class demo code: Introduction to Neural Networks and Deep Learning (Part 1)
___

## Overview

### neuralnetworksanddeeplearning.com integrated scripts for Python 3.5.2 and Theano with CUDA support

These scrips are updated ones from the **neuralnetworksanddeeplearning.com** gitHub repository in order to work with Python 3.5.2

The testing file (**test.py**) contains all three networks (network.py, network2.py, network3.py) from the book and it is the starting point to run (i.e. *train and evaluate*) them.

## Just type at shell: **python3.5 test.py**

In test.py there are examples of networks configurations with proper comments. I did that to relate with particular chapters from the book.

### License
Disributed under MIT License. [Link](LICENSE.md).


