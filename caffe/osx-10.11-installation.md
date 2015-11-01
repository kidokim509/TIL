# Caffe OS X Installation

## Environments

 * OS: OS X 10.11 (El Capitan)
 * GPU: CPU Only

## 참고
http://caffe.berkeleyvision.org/install_osx.html
https://github.com/BVLC/caffe/wiki/Installation-(OSX)

https://github.com/koosyong/caffestudy/wiki/install_osx
http://hoondy.com/2015/04/03/how-to-install-caffe-on-mac-os-x-10-10-for-dummies-like-me/


## Installation
1. Homebrew (http://brew.sh/)
2. Anaconda Python (https://www.continuum.io/downloads)
  * Anaconda is a completely free Python distribution (including for commercial use and redistribution). 
  * It includes more than 300 of the most popular Python packages for science, math, engineering, and data analysis. 
  * Version: Python 2.7
3. CUDA (https://developer.nvidia.com/cuda-downloads)
  * Even if your machine does not have a GPU, the CUDA libraries / headers need to be found by Caffe.
  * CUDA 7 is strongly suggested
4. Environmental Variables
```bash
  export LD_LIBRARY_PATH=''
  export DYLD_FALLBACK_LIBRARY_PATH=/usr/local/cuda/lib:$HOME/anaconda/lib:/usr/local/lib:/usr/lib
```
5. General Dependencies
  * AppStore에서 XCODE 설치
```bash
  brew install -vd snappy leveldb gflags glog szip lmdb
  brew tap homebrew/science
  brew install opencv #hdf5 (anaconda has hdf5)
```
6. Remaining dependencies, with / without Python
```bash
  brew install --build-from-source --with-python -vd protobuf
  brew install --build-from-source -vd boost boost-python
```
7. BLAS
  * Already installed as the Accelerate / vecLib Framework. 
  * On OSX 10.9, BLAS is provided by the Accelerate / vecLib framework natively by Apple. But when running the caffe tests, there is some numerical instability using those BLAS libraries. 
  * OpenBLAS and MKL are alternatives for faster CPU computation.


## Compilation
```bash
  git clone git@github.com:BVLC/caffe.git
  cd caffe
  cp Makefile.config.example Makefile.config
```

Makefile.config
```
	CPU_ONLY := 1

	# NOTE: this is required only if you will compile the python interface.
	# We need to be able to find Python.h and numpy/arrayobject.h.
	# PYTHON_INCLUDE := /usr/include/python2.7 \
	                /usr/lib/python2.7/dist-packages/numpy/core/include
	# Anaconda Python distribution is quite popular. Include path:
	# Verify anaconda location, sometimes it's in root.
	ANACONDA_HOME := $(HOME)/anaconda
	PYTHON_INCLUDE := $(ANACONDA_HOME)/include \
	                  $(ANACONDA_HOME)/include/python2.7 \
	                  $(ANACONDA_HOME)/lib/python2.7/site-packages/numpy/core/include \
    # We need to be able to find libpythonX.X.so or .dylib.
	# PYTHON_LIB := /usr/lib
	PYTHON_LIB := $(ANACONDA_HOME)/lib
```

### Build and Test
* make all
* make test
* make runtest
  * https://github.com/BVLC/caffe/issues/2320

