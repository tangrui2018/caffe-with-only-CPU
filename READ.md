# preload
sudo apt-get install git
sudo apt-get install libprotobuf-dev libleveldb-dev libopencv-dev libsnappy-dev
sudo apt-get install libhdf5-serial-dev protobuf-compiler
sudo apt-get install --no-install-recommends libboost-all-dev
sudo apt-get install libatlas-base-dev
sudo apt-get install python-dev
sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev


# download the code of the caffe
git clone https://github.com/bvlc/caffe.git
cd caffe/

# change the name of the file
mv Makefile.config.example Makefile.config

vim Makefile.config 
#  #CPU_ONLY -> CPU_ONLY

# compiling
make  -j

# if  Makefile:581: recipe for target '.build_release/src/caffe/layers/hdf5_output_layer.o' failed
#     make: *** [.build_release/src/caffe/layers/hdf5_output_layer.o] Error 1
# change  Makefile.config
# Whatever else you find you need goes here.
# INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include 
#                  to                
# Whatever else you find you need goes here.
# INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial/

# if  Makefile:572: recipe for target '.build_release/lib/libcaffe.so.1.0.0' failed
#     make: *** [.build_release/lib/libcaffe.so.1.0.0] Error 1
vim Makefile
LIBRARIES += glog gflags protobuf boost_system boost_filesystem m hdf5_hl hdf5
#                  to
LIBRARIES += glog gflags protobuf boost_system boost_filesystem m hdf5_serial_hl hdf5_serial

# Then
make

#  if python/caffe/_caffe.cpp:10:31: fatal error: numpy/arrayobject.h: No such file or directory
_________________________
sudo apt-get install python-numpy
sudo gedit Makefile.config
# change this sentence 
PYTHON_INCLUDE := /usr/include/python2.7 \
        /usr/<span style="color:#ff0000;">local</span>/lib/python2.7/dist-packages/numpy/core/include

make pycaffe -j8

# output: CXX/LD -o python/caffe/_caffe.so python/caffe/_caffe.cpp
# The compiling is successful

# add the environment variable
sudo gedit ~/.bashrc   
export PYTHONPATH=/home/usrname/caffe/python:$PYTHONPATH  # add to the end
source ~/.bashrc 

# whsyxt@whsyxt:~/Downloads/caffe/python$ python
Python 2.7.12 (default, Nov 19 2016, 06:48:10) 
[GCC 5.4.0 20160609] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import caffe
>>> exit()

# if ImportError: No module named skimage.io
sudo apt-get install python-pip
pip install scikit-image
# If ImportError: No module named google.protobuf.interna
pip install protobuf

# Then 
sudo make all -j8
sudo make test -j8
sudo make runtest -j8
sudo make pycaffe -j8


