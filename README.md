# Manully Compiling Shogun Toolbox with Python on Ubuntu 

## Preparation
This workflow is based on [Ubuntu 18.04 LTS](https://ubuntu.com/download/desktop/thank-you?country=US&version=18.04.2&architecture=amd64). The Username is assumed to be 'xxx'.

Install `make` for compiling

    sudo apt install make
    
Install `git` for getting source code from Github

    sudo apt install git
    
## Required packages

**ccache**

    sudo apt-get install ccache
**python-numpy**

    sudo apt-get install python3-dev python3-numpy
    
**g++**

    sudo apt-get install g++
    
Check `g++` version by: `g++ --version`
  
**pcre**

    sudo apt-get install libpcre3 libpcre3-dev
    
**swig**<br>

Download
   
    wget http://prdownloads.sourceforge.net/swig/swig-4.0.0.tar.gz

Unpack and cd to the directory

    tar -xzvf swig-4.0.0.tar.gz
    cd swig-4.0.0/
    
Configure    

    ./configure --prefix=/home/xxx/swig
   
Compile

    make

Install

    (sudo) make install

Open `.bashrc` 

    gedit ~/.bashrc

Add the path at the end of the file and save

    export SWIG_PATH=/home/xxx/swig/bin
    export PATH=$SWIG_PATH:$PATH

Reload `.bashrc`

    source ~/.bashrc
    
Check `swig` version by: `swig -version`

**CMake**
    
Shogun uses `CMake` for its build. Download latest .sh version of [CMake](https://cmake.org/download/) to `/home/xxx`

    wget https://github.com/Kitware/CMake/releases/download/v3.15.0-rc2/cmake-3.15.0-rc2-Linux-x86_64.sh
    bash cmake-3.15.0-rc2-Linux-x86_64.sh
    
Add path to `.bashrc`

    export CMAKE_PATH=/home/xxx/cmake-3.15.0-rc2-Linux-x86_64/bin
    export PATH=$CMAKE_PATH:$PATH

Check `CMake` version by: `cmake --version`
    
## Source code

Clone the latest stable release source code and update submodules

    git clone https://github.com/shogun-toolbox/shogun.git
    cd shogun/
    git submodule update --init

Create the build directory in the source tree root

    mkdir build/

Configure cmake, from the build directory, passing the Shogun source root as argument.

    cd build
    cmake /home/xxx/shogun\
          -DPYTHON_INCLUDE_DIR=/usr/include/python3.6\
          -DPYTHON_LIBRARY=/usr/lib/python3.6/config-3.6m-x86_64-linux-gnu/libpython3.6.so\
          -DPYTHON_EXECUTABLE:FILEPATH=/usr/bin/python3\
          -DPYTHON_PACKAGES_PATH=/usr/local/lib/python3.6/dist-packages\
          -DINTERFACE_PYTHON=ON\
          -DBUILD_META_EXAMPLES=OFF\
          -DUSE_SVMLIGHT=ON

Compile

    make

Install 

    (sudo) make install
    
The installation is done if `shogun.py` appears in `/home/xxx/shogun/build/src/interfaces/python/`.

To import Shogun in python, `shogun.py` and library `libshogun.so` need to be visibal to the system. Add path to .bashrc

    export PYTHONPATH=/home/xxx/shogun/build/src/interfaces/python/:$PYTHONPATH
    export LD_LIBRARY_PATH=/home/xxx/shogun/build/src/shogun/:$LD_LIBRARY_PATH
