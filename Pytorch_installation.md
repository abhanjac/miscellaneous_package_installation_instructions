# Installing Pytorch from source on Odroid XU4 (this is for python 2.7):

Go to [pytorch website](pytorch.org).
Select the options **linux**, **source**, **python 2.7** in the type of installation needed.
(This is to install the pytorch from source with python 2.7. The CUDA is only useful if you have NVIDIA graphics card)

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

This solution is obtained by the combined steps mentioned in the [link1](https://github.com/Intel-Media-SDK/MediaSDK/issues/34), 
[ https://projects.coin-or.org/Ipopt/ticket/229 ]
[ https://stackoverflow.com/questions/956640/linux-c-error-undefined-reference-to-dlopen ]

[ This is how you pass LDFLAGS into 'python setup.py' through command line https://stackoverflow.com/questions/8111754/how-to-pass-flags-to-a-distutils-extension ]
                                                                                   
So now the command will be like:

sudo python setup.py clean          [ for cleaning the build files ]
sudo -E MAX_JOBS=2 LDFLAGS="-Wl,--no-as-needed -ldl" python setup.py install


NOTE: you can also cross check that when you did not use the LDFLAGS='-WL, --no-as-needed -ldl' argument, the cmake command generated in the very beginning of the installation was like the following:

cmake .. -DBUILDING_WITH_TORCH_LIBS=ON -DCMAKE_BUILD_TYPE=Release -DBUILD_CAFFE2=0 -DBUILD_TORCH=ON -DBUILD_ATEN=ON -DBUILD_PYTHON=0 -DBUILD_BINARY=OFF -DBUILD_SHARED_LIBS=ON -DONNX_NAMESPACE=onnx_torch -DUSE_CUDA=0 -DUSE_ROCM=0 -DUSE_NNPACK=1 -DCUDNN_INCLUDE_DIR= -DCUDNN_LIB_DIR= -DCUDNN_LIBRARY= -DUSE_MKLDNN=0 -DMKLDNN_INCLUDE_DIR= -DMKLDNN_LIB_DIR= -DMKLDNN_LIBRARY= -DCMAKE_INSTALL_PREFIX=/home/odroid/pytorch/torch/lib/tmp_install -DCMAKE_EXPORT_COMPILE_COMMANDS=1 -DCMAKE_C_FLAGS= -DCMAKE_CXX_FLAGS= '-DCMAKE_EXE_LINKER_FLAGS=-L"/home/odroid/pytorch/torch/lib/tmp_install/lib"  -Wl,-rpath,$ORIGIN ' '-DCMAKE_SHARED_LINKER_FLAGS=-L"/home/odroid/pytorch/torch/lib/tmp_install/lib"  -Wl,-rpath,$ORIGIN ' -DCMAKE_PREFIX

But after using the LDFLAGS='-WL, --no-as-needed -ldl' argument, now the cmake command generated is like:

cmake .. -DBUILDING_WITH_TORCH_LIBS=ON -DCMAKE_BUILD_TYPE=Release -DBUILD_CAFFE2=0 -DBUILD_TORCH=ON -DBUILD_ATEN=ON -DBUILD_PYTHON=0 -DBUILD_BINARY=OFF -DBUILD_SHARED_LIBS=ON -DONNX_NAMESPACE=onnx_torch -DUSE_CUDA=0 -DUSE_ROCM=0 -DUSE_NNPACK=1 -DCUDNN_INCLUDE_DIR= -DCUDNN_LIB_DIR= -DCUDNN_LIBRARY= -DUSE_MKLDNN=0 -DMKLDNN_INCLUDE_DIR= -DMKLDNN_LIB_DIR= -DMKLDNN_LIBRARY= -DCMAKE_INSTALL_PREFIX=/home/odroid/pytorch/torch/lib/tmp_install -DCMAKE_EXPORT_COMPILE_COMMANDS=1 -DCMAKE_C_FLAGS= -DCMAKE_CXX_FLAGS= '-DCMAKE_EXE_LINKER_FLAGS=-L"/home/odroid/pytorch/torch/lib/tmp_install/lib"  -Wl,-rpath,$ORIGIN  -Wl,--no-as-needed -ldl' '-DCMAKE_SHARED_LINKER_FLAGS=-L"/home/odroid/pytorch/torch/lib/tmp_install/lib"  -Wl,-rpath,$ORIGIN  -Wl,--no-as-needed -ldl' -DCMAKE_PREFIX_PATH=/usr/lib/python2.7/dist-packages

Observe that "-Wl,--no-as-needed -ldl" is now included in the cmake command.

To test the installation, please change into a different directory first.

cd
python
>>> import torch
>>> x = torch.rand(5, 3)
>>> print(x)












If you get an error like this:
```
fatal: unable to access 'https://github.com/opencv/opencv.git/': server certificate verification failed. CAfile: /etc/ssl/certs/ca-certificates.crt CRLfile: none
```
It means that the computer is not trusting the github.
The in that case if you are not much worried about the security, then you can ask it to blindly trust github, by typing this command `export GIT_SSL_NO_VERIFY=1`.
There is another way, which is listed in this [link](http://stackoverflow.com/questions/21181231/server-certificate-verification-failed-cafile-etc-ssl-certs-ca-certificates-c).

#### Getting the Cutting-edge OpenCV from the Git Repository:
**(the cutting edge version might not be always stable but many bugs may be fixed in this one)**
```
sudo git clone https://github.com/Itseez/opencv.git
```
The above steps will create the **opencv** folder in the current directory.

#### Getting the Contrib packages:
If you also want the features like the **SIFT** or **SURF** algorithms, that are not free, you have to downoad the opencv_contrib package too.
This can be down loaded by
```
sudo git clone https://github.com/opencv/opencv_contrib.git
```
This will create the **opencv_contrib** folder in the current directory. This should be **outside** the **opencv** directory.

#### Building OpenCV from Source Using CMake without Contrib packages:
Create a temporary directory, which we denote as **release**, where you want to put the generated Makefiles, project files as well the object files and output binaries.

```
cd ~/opencv
mkdir release
cd release
sudo cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local ..
```

[ **NOTE:** Type `cmake -D CMAKE_BUILD_TYPE=RELEASE -D WITH_OPENNI=ON -D CMAKE_INSTALL_PREFIX=/usr/local ..` if you also want the **OpenNI** package to be configured with OpenCV. 
But for this the OpenNI should be installed before installation of openCV. 
You will find that the **cmake log** will show that the status 'OpenNI' and 'PrimeSensor' in CMake log is shown to be **OK**.
Some info is also given in this [link](https://www.packtpub.com/mapt/book/Application%20Development/9781785283840/1/ch01lvl1sec08/Choosing%20and%20using%20the%20right%20setup%20tools). ]

Similarly if you want to install other packages like:
* **QT**: for using several QT based GUI functions.
* **TBB**: makes sure that the OpenCV library will take advantage of all the cores you have in your systems CPU.
* **V4L**: video capture application programming interface for Linux. It supports many USB webcams, TV tuners, and other devices. Video4Linux is closely integrated with the Linux kernel.
* **VFVv3** and **NEON**: packages are used for cross compilation. (A cross compiler is a compiler capable of creating executable code for a platform other than the one on which the compiler is running).

Then you have to modify the **cmake** command as follows:
```
sudo cmake -D CMAKE_BUILD_TYPE=RELEASE -D WITH_OPENNI=ON -D WITH_QT=ON -D WITH_TBB=ON -D BUILD_TBB=ON -D WITH_V4L=ON -D ENABLE_VFPV3=ON -D ENABLE_NEON=ON -D CMAKE_INSTALL_PREFIX=/usr/local ..
```

To also activate **OpenGL ( for Laptop )**:

[ **NOTE:** OPENGL is used inside OpenCV for activating the OpenGL package for graphics. ]

Add the option `-D WITH_OPENGL=ON`

```
sudo cmake -D CMAKE_BUILD_TYPE=RELEASE -D WITH_OPENNI=ON -D WITH_QT=ON -D WITH_TBB=ON -D BUILD_TBB=ON -D WITH_OPENGL=ON -D WITH_V4L=ON -D ENABLE_VFPV3=ON -D ENABLE_NEON=ON -D CMAKE_INSTALL_PREFIX=/usr/local ..
```

To activate  **OpenGL ( for Odroid XU4 )**:

Add the option `-D WITH_OPENGL-ES=ON`.  **ES** meaning for Embedded system.

```
sudo cmake -D CMAKE_BUILD_TYPE=RELEASE -D WITH_OPENNI=ON -D WITH_QT=ON -D WITH_TBB=ON -D BUILD_TBB=ON -D WITH_OPENGL-ES=ON -D WITH_V4L=ON -D ENABLE_VFPV3=ON -D ENABLE_NEON=ON -D CMAKE_INSTALL_PREFIX=/usr/local ..
```

If some of these options throws errors, then remove those options.

#### Building OpenCV from Source Using CMake with Contrib packages:
[ If you also want to enable the opecv_contrib packages, then you have to add the following line as well.

`-D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules ../`

You have to provide the path to the **opencv_contrib/modules** folder. Since at the time of installation we will; be inside the **opencv/release** folder, and the 
**opencv_contrib** folder is inside the same directory as the **opencv** folder (which may be the home directory), that is why we keep the path as `../../opencv_contrib/modules`. 
And then you have to give the path to the opencv source folder. That is why we write the `../` after that.

So, altogether the command will be like: 

```
sudo cmake -D CMAKE_BUILD_TYPE=RELEASE -D BUILD_NEW_PYTHON_SUPPORT=ON -D INSTALL_C_EXAMPLES=ON -D INSTALL_PYTHON_EXAMPLES=ON -D BUILD_EXAMPLES=ON -D WITH_GTK=ON -D WITH_OPENNI=ON -D WITH_QT=ON -D WITH_TBB=ON -D BUILD_TBB=ON -D WITH_OPENGL=ON -D WITH_V4L=ON -D ENABLE_VFPV3=ON -D ENABLE_NEON=ON -D CMAKE_INSTALL_PREFIX=/usr/local -D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules ..
```

REMEMBER that for **Odroid XU4** there will be **OPENGL-ES** instead of **OPENGL**.

If available on intel processors, opencv exploits a royalty free subset of intel's Integrated Performance Primitives (IPP) library (IPPICV). 
This library can be linked with the opencv during compilation and if done, then it replaces some of the corresponding low level optimized C code. 
For this you have to add the following command during installation, `cmake -D WITH_IPP=ON`. The speed improvement can be a lot. 
But you have to have an intel processor for this, it will not happen on ARM processors and I have not tested it either.

#### Installation:
Next, type the following (while inside the 'opencv/release' directory):

```
sudo make -j4
```
[ **NOTE:** The '-j4' runs 4 jobs in parallel. Change '4' to number of hardware threads available. However, too many cores (like j4, j5 or j8) may make single board computers like the Odroid XU4 run out of memory.
You can also type only the 'make' command, but in that case the installation might take more than two to four hours. ]

```
sudo make install
```

After installation is complete, the default installation path is `/usr/local/lib` and `/usr/local/include/opencv2`.
Hence you need to add the line `/usr/local/lib/` to the file **/etc/ld.so.conf.d/opencv.conf**.

If this file **opencv.conf** is not there in this folder **/etc/ld.so.conf.d**, then create this file 'opencv.conf' inside this folder and write this line `/usr/local/lib` (without the quotes). 

Then type the command in the terminal:

```
sudo ldconfig
```

[ **NOTE:** If you don't do this then, you haven't put the shared library in a location where the loader can find it. 
Look inside the **/usr/local/lib** folder and see if it contains any shared libraries (files beginning in lib and usually ending in .so). 
When you find them, create a file called **/etc/ld.so.conf.d/opencv.conf** and write to it the paths to the folders where the libraries are stored. ]

#### Run and Check:
Once the opencv has been installed, you can try to import it in a python script and see if it works properly.
To check if the opencv contrib modules are installed or not, you can try running the following statements in a python script.

```
python
>>> import cv2
>>> recognizer = cv2.face.LBPHFaceRecognizer_create()
>>> detector = cv2.CascadeClassifier("haarcascade_frontalface_default.xml")
```

# Running a C++ code for opencv written in a .cpp file by gcc compiler in linux on Odroid or laptop:

* Make sure that the opencv is installed properly as per the above steps.
* After the C++ program is written, assume that the name of the program file is 'test.cpp' and you want the the executable name to be 'test.exe'.
* To compile it go inside the directory where the test.cpp file is.
* Type  the following in the terminal: 
```
g++ `pkg-config --cflags --libs opencv` -o test test.cpp
g++ `[this is the back tick symbol] pkg-config --cflags --libs opencv` [name of the libraries] -o test [name of the .exe file that you want to create] test.cpp [name of the program file]
```

If this does not work then try this out:
```
g++ -o test test.cpp `pkg-config --cflags --libs opencv`
g++ -o test [name of the .exe file that you want to create] test.cpp [name of the program file] `pkg-config --cflags --libs opencv` [name of the libraries]
```

This is because some of these libraries may be archived libraries and so you have to put the files that need them (like test.cpp) in front of these libraries to make them work.

# Removing OpenCV:

If you remove the **opencv** folder (where you cloned the package) after the installation is complete, even then you can use **cv2**, as the **.so** files are all in the **/usr/local/lib** directory.
If you want to remove or uninstall opencv, then remove all the files having names starting with **libopencv** from this **/usr/local/lib** directory.


