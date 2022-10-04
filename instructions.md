I was successful in installing the svFSI using source code.

For beginners who want to install the svFSI without Trillnos in ubuntu 18.04 or 20.04, I am hereby posting the things that worked for me.
# First, build the prerequisites just with apt
install git, if you don't have done it previously
```
sudo apt install git
```
Install gcc, gfortran, lapack, liblas and open mpi
```
sudo apt install gcc
sudo apt install gfortran
sudo apt install liblapack-dev
sudo apt install libblas-dev
sudo apt install libopenmpi-dev
sudo apt install openmpi-bin
```
Install cmake (for ubuntu 20.04)
```
sudo apt install cmake
sudo apt install cmake-curses-gui
```
cmake (for ubuntu 18.04)[/b]
[b] Note that,[/b] svFSI demands cmake version 3.12 or latest. However, in [b]ubuntu 18.04 [/b] the default version of the cmake is 3.10, which is not sufficient to build svFSI. I suggest first check the default version of cmake if you have previously installed cmake for any other purpose. 
```
cmake --version
```
[*]if you haven't installed cmake before, it will shows that 

[i]Command 'cmake' not found, but can be installed with:
sudo snap install cmake  # version 3.23.1, or
sudo apt  install cmake[/i]

 then I recommend installing the desired very new version, with the snap
```
sudo snap install cmake --classic #version 3.23.1
```


[*]if it is below 3.10, we have to find a  way to replace this default version with the latest versions. In my case, I followed the steps provided in the following link to do it [url]https://askubuntu.com/questions/355565/how-do-i-install-the-latest-version-of-cmake-from-the-command-line[/url]. Just follow the section A. Using APT Repositories (which is recommended for normal users) . Same I am copying below.
```
sudo apt remove --purge --auto-remove cmake
sudo apt purge --auto-remove cmake
```
```
sudo apt update && \
sudo apt install -y software-properties-common lsb-release && \
sudo apt clean all
```
```
wget -O - https://apt.kitware.com/keys/kitware-archive-latest.asc 2>/dev/null | gpg --dearmor - | sudo tee /etc/apt/trusted.gpg.d/kitware.gpg >/dev/null
```
```
sudo apt-add-repository "deb https://apt.kitware.com/ubuntu/ $(lsb_release -cs) main"
```
```
sudo apt update
sudo apt install kitware-archive-keyring
sudo rm /etc/apt/trusted.gpg.d/kitware.gpg
```
```
sudo apt update
sudo apt install cmake
```
 Now you can check the version again
```
cmake --version
```
Hopefully , it should be a latest version.

2. Once, the prerequisites are ready just follow the following steps to build the svFS[/u]I[/b]
```
mkdir svFSI  && cd svFSI
git clone https://github.com/SimVascular/svFSI.git
mkdir build && cd build
ccmake ../svFSI
make
```
[b][u]3. Finally to run/test any tutorial test case[/u][/b]
first download the test case from svFSI-test repository to any directory where you want to  run a case
```
git clone https://github.com/SimVascular/svFSI-Tests.git
```
navigate to any particular case (for eg. svFSI-Tests/04-fluid/02-dye_AD/)
```
cd svFSI-Tests/04-fluid/02-dye_AD/
```
create soft links to the  installed svFSI (location might change according to where you have built the code)
```
ln -s /home/username/svFSI/build/svFSI-build/bin/svFSI
```
Now you can run case the in series as
```
./svFSI svFSI.inp
```
or in parallel (for eg. with 8 procs)
```mpirun -np 8 ./svFSI svFSI.inp
```
**In case in the test case, if the preconditioner is Trillinos, you can replace it to FSILS in the .inp file and proceed the run.**
------------------------------------------------------------------------------------------------------
