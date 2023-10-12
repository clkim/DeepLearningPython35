## Running IEEE Boston Section class demo code
## Introduction to Neural Networks and Deep Learning (Part 1)<br>October 21, 2023
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
Go to [Install Git](https://www.atlassian.com/git/tutorials/install-git), and scroll to Install Git on \[Mac OS X | Windows | Linux\] section as appropriate.  
Unless you have a preference, you should probably just pick the first method for your platform.
 
(For Mac:  
Git for Mac Installer > pick latest link (currently `git-2.33.0-intel-universal-mavericks.dmg`);  
This version is more than a year old but for our purposes is perfectly ok.  
If you want the latest version, go to [Git - Download for macOS](https://git-scm.com/download/mac) and follow instructions for your preference, e.g. Homebrew or MacPorts.  
If you're installing from a downloaded .dmg file, you may be blocked from opening the installation package; if you see something like
"macOS cannot verify that this app is free from malware", go to _System Preferences_ > _Security & Privacy_ > _General_ tab, click the lock icon in lower corner to Unlock,  
then under `Allow apps downloaded from` | `App Store and identified developers` > `Allow` that downloaded git-xxx.pkg file, if you're sure it is safe.  
Install just Git; you should not need to install git-credential-osxkeychain helper.)

### How to download Docker software
Docker is a very popular container technology. The containers run on _Docker Engine_.

Estimated time: 15 mins.  
Go to [Get Docker](https://docs.docker.com/get-docker/), and pick Docker Desktop for \[Mac | Windows | Linux\] to do the appropriate install.  
After installing Docker Desktop, find the application icon to run the _Docker_ app (_Docker Desktop_), so as to start _Docker Engine_;
it could take about half a minute for _Docker Desktop_ window to start and open; then you could minimize or close that window (double check that _Docker Desktop_
is still running).

##### Public Service Announcement
Looks like currently Docker Desktop 4.24.0 (122432) for Mac has an issue being tracked here
[Docker does not recover from resource saver mode](https://github.com/docker/for-mac/issues/6933).
The workaround for me is: As soon as Docker Desktop starts, open Settings (wheel icon on top right) > left menu > Resources | Advanced
>scroll down to Resource Saver > unset Enable Resource Saver > Apply & restart button  
>scroll up to Resource Allocation | CPU limit > instead of default 8, reduce to number of cores on your Mac (4 on mine) > Apply & restart button  
>click Cancel button to exit Settings

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
  chap2_fully-matrix-based-backpropagation-mini-batch
  chap6
* master

~/DeepLearningPython35 $ git checkout chap1_30-hidden-neurons-3.0-eta

~/DeepLearningPython35 $ git branch
* chap1_30-hidden-neurons-3.0-eta
  chap2_fully-matrix-based-backpropagation-mini-batch
  chap6
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

Estimated time: 10 mins.  
The commands shown in the text area below do the following; the text area also shows the _Terminal_ console response to the commands:
- First run `docker pull continuumio/anaconda3` to download the _Anaconda_ image (based on Python 3.X), which has the _conda_, a package manager as well as
an environment manager tool; Anaconda is a very popular data science platform; the download is from the Docker hub [docker-anaconda](https://hub.docker.com/r/continuumio/anaconda3)
- Then run `docker image ls` to verify the image _continuumio/anaconda3_ (with Tag _latest_) is downloaded  
- Then run the given `docker` command to create a new container layer over the downloaded image
  - At the interactive shell command line inside the container, we can look for the conda version, with `conda --version`
  - (May be prompted to update to a new version of conda; if so, go ahead and follow the prompts to update)
  - Do a confirming check that the Python package _Numpy_ is installed, with `conda list | grep numpy`
    - Should see the _numpy_ related packages
  - And do a sanity check that we do have python installed, with `python --version`
  - (Extra credit: we can use _conda_ to install packages, with `conda install packagename`)
  - Ok, exit our newly created local container, with `exit`
- Back at the Terminal console, verify that we have created a local container named _deeplearning_, with `docker container ls --all`
```
~/DeepLearningPython35 $
~/DeepLearningPython35 $ docker pull continuumio/anaconda3
Using default tag: latest
...
docker.io/continuumio/anaconda3:latest

~/DeepLearningPython35 $ docker image ls
REPOSITORY               TAG       IMAGE ID       CREATED         SIZE
continuumio/anaconda3    latest    xxxxxxxxxxxx   nn days ago     n.nnGB

~/DeepLearningPython35 $ docker run -it --name deeplearning --mount type=bind,source="$(pwd)",target=/deeplearn continuumio/anaconda3

(base) root@xxx:/#
(base) root@xxx:/# conda --version
conda 23.7.4

(base) root@xxx:/# conda list | grep numpy
numpy                     1.24.3          py311h08b1b3b_1
numpy-base                1.24.3          py311hf175353_1
numpydoc                  1.5.0           py311h06a4308_0

(base) root@xxx:/# python --version
Python 3.11.5

(base) root@xxx:/# exit
exit

~/DeepLearningPython35 $ docker container ls --all
CONTAINER ID   IMAGE                    COMMAND       CREATED      ...       NAMES
xxxxxxxxxxxx   continuumio/anaconda3    "/bin/bash"   xxx                    deeplearning
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
  - At the interactive shell command line inside the container, we can use `ls` to see the directories at the root directory;  
  then `cd` into the _deeplearn_ directory mounted into the container;
    - When we created the container, we had bind that mount to the local _DeepLearningPython35_ directory,
    which must be already on the git branch _chap1_30-hidden-neurons-3.0-eta_
  - We can see the files in our local _DeepLearningPython35_ directory, including _test.py_, with `ls`
  - We can check the python version, with `python --version`
  - Now, we can run the demo code in _test.py_, with `python3.11 test.py`
    - On my late-2013 MacBook Pro, it takes about 10s - 15s to complete first Epoch 0, about a minute to finish Epoch 5
    - Each epoch run uses the training images; then neural network is evaluated on the 10000 test images
  - Use control-c to break out of the run as desired
  - After the run, we exit the container, with `exit`
- Now we should be back at the Terminal console, in the _DeepLearningPython35_ directory
```
~/DeepLearningPython35 $
~/DeepLearningPython35 $ docker container ls --all
CONTAINER ID   IMAGE                    COMMAND       CREATED      ...       NAMES
xxxxxxxxxxxx   continuumio/anaconda3    "/bin/bash"   xxx                    deeplearning

~/DeepLearningPython35 $ docker container start -ai deeplearning

(base) root@xxx:/#
(base) root@xxx:/# ls
bin  boot  deeplearn  dev  etc	home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
(base) root@xxx:/# cd deeplearn/

(base) root@xxx:/deeplearn#
(base) root@xxx:/deeplearn# ls
MyNetwork  __pycache__	    mnist.pkl.gz	       mnist_expanded.pkl.gz  mnist_svm.py  network2.py  test.py
README.md  expand_mnist.py  mnist_average_darkness.py  mnist_loader.py	      network.py    network3.py

(base) root@xxx:/deeplearn# python --version
Python 3.11.5

(base) root@xxx:/deeplearn# python3.11 test.py
Epoch 0 : 8943 / 10000
Epoch 1 : 9166 / 10000
Epoch 2 : 9267 / 10000
Epoch 3 : 9340 / 10000
Epoch 4 : 9337 / 10000
Epoch 5 : 9374 / 10000
Epoch 6 : 9386 / 10000
...
< Use control-c to break out of the run as desired >

(base) root@xxx:/deeplearn# exit
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


