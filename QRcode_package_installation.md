# Installing QRcode package for the Odroid XU4 and Laptop:

The qr code generating package of the python is downloaded from the following [site](https://pypi.python.org/pypi/qrcode#downloads).

It will be downloaded as a **.gz** file.
Extract the file in the desired directory by the command

` tar <destination_directory> -xvf <the qrcode .gz filepath> `


You can directly do a `sudo pip install qrcode` and then run **python** and **import qrcode**.

This will direct you to [this](https://github.com/pytorch/pytorch#from-source) page.
But you can also directly go to there using the link [https://github.com/pytorch/pytorch#from-source](https://github.com/pytorch/pytorch#from-source).

All the official instructions are given here. But the following instruction also have added instructions and solutions to some errors faced during installation.

#### Install Dependencies:
```
sudo apt-get update
sudo apt-get install python-numpy cmake gcc
sudo pip install pyyaml typing
sudo pip install cffi 
sudo pip install -U pip setuptools
sudo pip install leveldb==0.18
sudo apt-get install libopenblas-dev cython libatlas-dev m4 libblas-dev cmake
sudo apt-get install libffi-dev
```

If you have trouble installing the last package **libffi-dev** then you can refer to this [link](https://github.com/markwal/OctoPrint-PolarCloud/issues/19).

#### Getting Pytorch from source:
```
sudo git clone --recursive https://github.com/pytorch/pytorch.git
```

[ **NOTE:** Pleasu use the option **--recursive** here. ]

##### Installation:

Now go to the pytorch directory and run the **setup.py** file.

```
cd pytorch
sudo git submodule update --init
export NO_CUDA=1
export NO_DISTRIBUTED=1
sudo -E MAX_JOBS=2 python setup.py install
```

Usually you have to change the swap space for installing pytorch, but that is because the installation process tends ot create **8** threads while installing, 
which these small computers may not be able to handle. But there is the flag **MAX_JOBS** which if set to **1** or **2** will force to create only that many installation threads.
Hence, we don't need to change the swap space, at least that is what we got on the Odroid XU4.
But the installation process may take more time. This [link](https://github.com/pytorch/pytorch/issues/7841) also shows the use of the **MAX_JOBS** option.

Do not forget to export **NO_CUDA** and **NO_DISTRIBUTED** and also do not forget the **-E** while calling the **setup.py install**. 
The **-E** is important here to preserve the environmental variables (NO_DISTRIBUTED and NO_CUDA).

This should be able to install the pytorch package into the Odroid.
The overall installation may take **an hour**.

#### A wierd error faced during installation:

However, during our installation, we faced an error like the following (this error may crop up even at 99%):

```
[ 96%] Built target torch
[ 96%] Building CXX object caffe2/torch/CMakeFiles/test_jit.dir/csrc/jit/test_jit.cpp.o
[ 96%] Building CXX object caffe2/torch/CMakeFiles/test_api.dir/__/test/cpp/api/any.cpp.o
[ 96%] Building CXX object caffe2/torch/CMakeFiles/test_api.dir/__/test/cpp/api/cursor.cpp.o
[ 96%] Building CXX object caffe2/torch/CMakeFiles/test_api.dir/__/test/cpp/api/integration.cpp.o
[ 96%] Building CXX object caffe2/torch/CMakeFiles/test_api.dir/__/test/cpp/api/main.cpp.o
[ 96%] Linking CXX executable ../../bin/test_jit
../../lib/libtorch.so.1: undefined reference to `dlclose'
../../lib/libtorch.so.1: undefined reference to `dlsym'
../../lib/libtorch.so.1: undefined reference to `dlopen'
../../lib/libtorch.so.1: undefined reference to `dlerror'
collect2: error: ld returned 1 exit status
caffe2/torch/CMakeFiles/test_jit.dir/build.make:97: recipe for target 'bin/test_jit' failed
make[2]: *** [bin/test_jit] Error 1
CMakeFiles/Makefile2:2296: recipe for target 'caffe2/torch/CMakeFiles/test_jit.dir/all' failed
make[1]: *** [caffe2/torch/CMakeFiles/test_jit.dir/all] Error 2
make[1]: *** Waiting for unfinished jobs....
[ 96%] Building CXX object caffe2/torch/CMakeFiles/test_api.dir/__/test/cpp/api/misc.cpp.o
[ 97%] Building CXX object caffe2/torch/CMakeFiles/test_api.dir/__/test/cpp/api/module.cpp.o
[ 97%] Building CXX object caffe2/torch/CMakeFiles/test_api.dir/__/test/cpp/api/modules.cpp.o
[ 97%] Building CXX object caffe2/torch/CMakeFiles/test_api.dir/__/test/cpp/api/optim.cpp.o
[ 97%] Building CXX object caffe2/torch/CMakeFiles/test_api.dir/__/test/cpp/api/parallel.cpp.o
[ 97%] Building CXX object caffe2/torch/CMakeFiles/test_api.dir/__/test/cpp/api/rnn.cpp.o
[ 98%] Building CXX object caffe2/torch/CMakeFiles/test_api.dir/__/test/cpp/api/sequential.cpp.o
[ 98%] Building CXX object caffe2/torch/CMakeFiles/test_api.dir/__/test/cpp/api/serialization.cpp.o
[ 98%] Building CXX object caffe2/torch/CMakeFiles/test_api.dir/__/test/cpp/api/static.cpp.o
[ 98%] Building CXX object caffe2/torch/CMakeFiles/test_api.dir/__/test/cpp/api/tensor_cuda.cpp.o
[ 98%] Building CXX object caffe2/torch/CMakeFiles/test_api.dir/__/test/cpp/api/tensor.cpp.o
[ 98%] Building CXX object caffe2/torch/CMakeFiles/test_api.dir/__/test/cpp/api/tensor_options.cpp.o
[100%] Building CXX object caffe2/torch/CMakeFiles/test_api.dir/__/test/cpp/api/tensor_options_cuda.cpp.o
[100%] Linking CXX executable ../../bin/test_api
../../lib/libtorch.so.1: undefined reference to `dlclose'
../../lib/libtorch.so.1: undefined reference to `dlsym'
../../lib/libtorch.so.1: undefined reference to `dlopen'
../../lib/libtorch.so.1: undefined reference to `dlerror'
collect2: error: ld returned 1 exit status
caffe2/torch/CMakeFiles/test_api.dir/build.make:513: recipe for target 'bin/test_api' failed
make[2]: *** [bin/test_api] Error 1
CMakeFiles/Makefile2:2336: recipe for target 'caffe2/torch/CMakeFiles/test_api.dir/all' failed
make[1]: *** [caffe2/torch/CMakeFiles/test_api.dir/all] Error 2
Makefile:138: recipe for target 'all' failed
make: *** [all] Error 2
Failed to run 'bash tools/build_pytorch_libs.sh --use-nnpack caffe2 nanopb libshm gloo THD'
```

This is a big error message, but it seemed that this is not a problem with pytorch, but with Odroid. The packages **dlclose, dlsym, dlopen, dlerror** cannot be referenced during installation.

Then you have to do a fresh install. You can delete the pytorch directory and reclone it and start fresh, but usually if you just run `sudo python setup.py clean`, then all the build files are cleaned and that is good enough.

To fix this error, you have to use the option **LDFLAGS='-WL, --no-as-needed -ldl'** to the command **python setup.py install** to create the references to **dlclose, dlopen** etc.

This solution is obtained by the combined steps mentioned in the [link1](https://github.com/Intel-Media-SDK/MediaSDK/issues/34), [link2](https://projects.coin-or.org/Ipopt/ticket/229) and [link3](https://stackoverflow.com/questions/956640/linux-c-error-undefined-reference-to-dlopen).

[This](https://stackoverflow.com/questions/8111754/how-to-pass-flags-to-a-distutils-extension) is how you pass **LDFLAGS** into the **python setup.py** command.
                                                                                   
So now the command will be like:

```
sudo python setup.py clean
sudo -E MAX_JOBS=2 LDFLAGS="-Wl,--no-as-needed -ldl" python setup.py install
```

[ **NOTE:** You can also cross check the autogenerated **cmake** command when you did not use the **LDFLAGS='-WL, --no-as-needed -ldl'** initially. 
The cmake command generated in the very beginning of the installation was like the following:

```
cmake .. -DBUILDING_WITH_TORCH_LIBS=ON -DCMAKE_BUILD_TYPE=Release -DBUILD_CAFFE2=0 -DBUILD_TORCH=ON -DBUILD_ATEN=ON -DBUILD_PYTHON=0 -DBUILD_BINARY=OFF -DBUILD_SHARED_LIBS=ON -DONNX_NAMESPACE=onnx_torch -DUSE_CUDA=0 -DUSE_ROCM=0 -DUSE_NNPACK=1 -DCUDNN_INCLUDE_DIR= -DCUDNN_LIB_DIR= -DCUDNN_LIBRARY= -DUSE_MKLDNN=0 -DMKLDNN_INCLUDE_DIR= -DMKLDNN_LIB_DIR= -DMKLDNN_LIBRARY= -DCMAKE_INSTALL_PREFIX=/home/odroid/pytorch/torch/lib/tmp_install -DCMAKE_EXPORT_COMPILE_COMMANDS=1 -DCMAKE_C_FLAGS= -DCMAKE_CXX_FLAGS= '-DCMAKE_EXE_LINKER_FLAGS=-L"/home/odroid/pytorch/torch/lib/tmp_install/lib"  -Wl,-rpath,$ORIGIN ' '-DCMAKE_SHARED_LINKER_FLAGS=-L"/home/odroid/pytorch/torch/lib/tmp_install/lib"  -Wl,-rpath,$ORIGIN ' -DCMAKE_PREFIX
```

But after using the **LDFLAGS='-WL, --no-as-needed -ldl'** option, now the cmake command generated is like:

```
cmake .. -DBUILDING_WITH_TORCH_LIBS=ON -DCMAKE_BUILD_TYPE=Release -DBUILD_CAFFE2=0 -DBUILD_TORCH=ON -DBUILD_ATEN=ON -DBUILD_PYTHON=0 -DBUILD_BINARY=OFF -DBUILD_SHARED_LIBS=ON -DONNX_NAMESPACE=onnx_torch -DUSE_CUDA=0 -DUSE_ROCM=0 -DUSE_NNPACK=1 -DCUDNN_INCLUDE_DIR= -DCUDNN_LIB_DIR= -DCUDNN_LIBRARY= -DUSE_MKLDNN=0 -DMKLDNN_INCLUDE_DIR= -DMKLDNN_LIB_DIR= -DMKLDNN_LIBRARY= -DCMAKE_INSTALL_PREFIX=/home/odroid/pytorch/torch/lib/tmp_install -DCMAKE_EXPORT_COMPILE_COMMANDS=1 -DCMAKE_C_FLAGS= -DCMAKE_CXX_FLAGS= '-DCMAKE_EXE_LINKER_FLAGS=-L"/home/odroid/pytorch/torch/lib/tmp_install/lib"  -Wl,-rpath,$ORIGIN  -Wl,--no-as-needed -ldl' '-DCMAKE_SHARED_LINKER_FLAGS=-L"/home/odroid/pytorch/torch/lib/tmp_install/lib"  -Wl,-rpath,$ORIGIN  -Wl,--no-as-needed -ldl' -DCMAKE_PREFIX_PATH=/usr/lib/python2.7/dist-packages
```

Observe that in the second case **-Wl,--no-as-needed -ldl** is included in the cmake command.

This will complete the installation.

#### Run and Check:
Once the package has been installed, you can try to import it in a python script and see if it works properly.

```
cd
python
>>> import torch
>>> x = torch.rand(5, 3)
>>> print(x)
```

### Installing torchvision on Odroid XU4:

This is straight forward and we can follow this [link](https://github.com/pytorch/vision).

```
git clone https://github.com/pytorch/vision.git
cd vision
sudo python setup.py install
```

### Installing torchvision on Odroid XU4:

This is straight forward and we can follow this [link](https://github.com/pytorch/vision).

```
git clone https://github.com/pytorch/vision.git
cd vision
sudo python setup.py install
```

## Installing on Laptop using Anaconda:

Better to have **python 3** and **Anaconda** installed.

Go to [Pytorch homepage](https://pytorch.org/).

Select the option **linux**, option **conda** and the **python** version (the CUDA is only useful if you have **NVIDIA** graphics card).

The webpage will suggest a command (like `conda install pytorch torchvision -c pytorch`), to run in the terminal for the installation.
 

#### Updating the torchvision and Pytorch [ master documentation ].

This has to be done from **source**.

Updating the pytorch to master version instructions are given in the [source](https://github.com/pytorch/pytorch#from-source) page.

Recommend to use **Anaconda** for this.
Once you have Anaconda installed, here are the instructions.

If you want to compile with CUDA support, install the following:
NVIDIA CUDA 7.5 [link](https://developer.nvidia.com/cuda-downloads).
NVIDIA cuDNN v6.x [link](https://developer.nvidia.com/cudnn).

#### Install Dependencies on Linux:

```
export CMAKE_PREFIX_PATH="[anaconda root directory]"
```

(This might be like export CMAKE_PREFIX_PATH="/home/arindam/anaconda3", i.e. the directory where the **Anaconda** package is installed).

```
conda install numpy pyyaml mkl setuptools cmake cffi
```

#### Add LAPACK support for the GPU (only if gpu is there):

```
conda install -c soumith magma-cuda80 
```

or if CUDA 7.5

```
conda install -c soumith magma-cuda75
```

Then do:

```
git clone --recursive https://github.com/pytorch/pytorch
cd pytorch
python setup.py install
```

#### Updating torchvision:

instructions can also be found [here](https://github.com/pytorch/vision).

```
git clone https://github.com/pytorch/vision.git
cd vision
python setup.py install
```

## Installing from the Source on Laptop:

Go to [Pytorch homepage](https://pytorch.org/).

Select the option **linux**, option **source** and the **python** version (the CUDA is only useful if you have **NVIDIA** graphics card).

#### Install Dependencies:

Go to the [link](https://github.com/pytorch/pytorch#from-source).

Run the following commands to install the dependencies:

```
sudo apt-get install python-nympy cmake gcc
sudo pip install pyyaml cffi 
sudo pip install -U pip setuptools
```

Clone the pytorch package:
```
sudo git clone https://github.com/pytorch/pytorch.git
```

Then go to the pytorch directory and run the **setup.py** file.

```
cd pytorch
sudo python setup.py install
```

You might receive some warning message as well when the installation is going on.

Go to [Pytorch tutorials](http://pytorch.org/tutorials/) for help.



