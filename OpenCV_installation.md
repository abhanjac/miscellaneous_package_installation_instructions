# OpenCV3 installation instruction in Ubuntu 16.04 LTS on laptop or Odroid XU4:

Official instructions are given in [Instructions link](http://docs.opencv.org/3.0-beta/doc/tutorials/introduction/linux_install/linux_install.html)
But the following instruction also have added options for a more elaborate installation.

#### Install Dependencies:
```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install build-essential 
sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev 
sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libjasper-dev libdc1394-22-dev 
sudo apt-get install checkinstall yasm libxine2-dev libv4l-dev libav-tools
sudo apt-get install unzip libdc1394-22 libpng12-dev libtiff5-dev tar dtrx 
sudo apt-get install libopencore-amrnb-dev libopencore-amrwb-dev libtheora-dev libvorbis-dev libxvidcore-dev x264 
sudo apt-get install libgstreamer0.10-dev libgstreamer-plugins-base0.10-dev libqt4-dev libmp3lame-dev 
```

#### Getting OpenCV Source Code:
```
sudo git clone https://github.com/opencv/opencv.git
```

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

