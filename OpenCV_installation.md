# OpenCV3 installation instruction in Ubuntu 16.04 LTS or laptop or Odroid XU4:
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
The in that case if you are not much worried about the security, then you can ask it to blindly trust github, by typing this command "export GIT_SSL_NO_VERIFY=1".
There is another way, which is listed in this [link](http://stackoverflow.com/questions/21181231/server-certificate-verification-failed-cafile-etc-ssl-certs-ca-certificates-c).

#### Getting the Cutting-edge OpenCV from the Git Repository:
**(the cutting edge version might not be always stable but many bugs may be fixed in this one)**
```
sudo git clone https://github.com/Itseez/opencv.git
```
The above steps will create the 'opencv' folder in the current directory.

#### Getting the Contrib packages:
If you also want the features like the **SIFT** or **SURF** algorithms, that are not free, you have to downoad the opencv_contrib package too.
This can be down loaded by
```
sudo git clone https://github.com/opencv/opencv_contrib.git
```
This will create the 'opencv_contrib' folder in the current directory.


Building OpenCV from Source Using CMake, Using the Command Line:
Create a temporary directory, which we denote as <cmake_binary_dir>, where you want to put the generated Makefiles, 
project files as well the object files and output binaries.

cd ~/opencv
mkdir release		[usually the name of this folder is given as 'build' or 'release', prefer 'release']
cd release
sudo cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local ..

[ NOTE:
(or type 'cmake -D CMAKE_BUILD_TYPE=RELEASE -D WITH_OPENNI=ON -D CMAKE_INSTALL_PREFIX=/usr/local ..' if you also want the openNI 
package to be configured with OpenCV, but for this the OpenNI should be installed before installation of openCV.)
(you will find that the cmake log will show that the status 'OpenNI' and 'PrimeSensor' in CMake log is shown to be 'OK')
(some info is also given here:
 https://www.packtpub.com/mapt/book/Application%20Development/9781785283840/1/ch01lvl1sec08/Choosing%20and%20using%20the%20right%20setup%20tools )

(or type 'cmake -D CMAKE_BUILD_TYPE=RELEASE -D WITH_OPENNI=ON -D WITH_QT=ON -D CMAKE_INSTALL_PREFIX=/usr/local ..' if you also want the QT 
package to be configured with OpenCV, for using several QT based GUI functions)

(Similarly adding 'cmake -D WITH_TBB=ON -D BUILD_TBB=ON' in that line, is used inside OpenCV for parallel code snippets. This will make sure that the 
OpenCV library will take advantage of all the cores you have in your systems CPU.)

(Similarly adding 'cmake -D WITH_OPENGL=ON' is used inside OpenCV for activating the OpenGL package for graphics.)

For Odroid XU4, it will be 'cmake -D WITH_OPENGL-ES=ON', ES meaning for Embedded system.

(Similarly adding 'cmake -D WITH_V4L=ON' is used inside OpenCV for activating the Video4Linux or V4L is a video capture application programming interface for Linux. 
It supports many USB webcams, TV tuners, and other devices. Video4Linux is closely integrated with the Linux kernel.)

(A cross compiler is a compiler capable of creating executable code for a platform other than the one on which the compiler is running. 
For example, a compiler that runs on a Windows 7 PC but generates code that runs on Android smartphone is a cross compiler.
A cross compiler is necessary to compile for multiple platforms from one machine. A platform could be infeasible for a compiler to run on, 
such as for the microcontroller of an embedded system because those systems contain no operating system. 
In paravirtualization one machine runs many operating systems, and a cross compiler could generate an executable for each of them from one main source.)
The VFVv3 and NEON packages are used for cross compilation.
(Depending on target platform architecture different instruction sets can be used. By default compiler generates code for armv5l without VFPv3 and NEON extensions. 
Adding 'cmake -D ENABLE_VFPV3=ON -D ENABLE_NEON=ON' enable code generation for VFPv3 and for using NEON SIMD extensions.)

Altogether the command will be like: 

sudo cmake -D CMAKE_BUILD_TYPE=RELEASE -D WITH_OPENNI=ON -D WITH_QT=ON -D WITH_TBB=ON -D BUILD_TBB=ON -D WITH_OPENGL=ON -D WITH_V4L=ON -D ENABLE_VFPV3=ON -D ENABLE_NEON=ON -D CMAKE_INSTALL_PREFIX=/usr/local ..

REMEMBER that for Odroid XU4 there will be OPENGL-ES instead of OPENGL.

(If you also want to enable the opecv_contrib packages, then you have to add the line 'cmake -D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules ../' also.
You have to provide the path to the 'opencv_contrib/modules' folder. Since at the time of installation we will; be inside the 'opencv/release' folder, and the 
'opencv_contrib' folder is inside the same directory as the 'opencv' folder (which may be the 'home' directory), that is why we keep the path as '../../opencv_contrib/modules'. 
And then you have to give the path to the opencv 'source' folder, that is the 'opencv' folder. That is why we write the '../' after that).

Altogether the command will be like: 

sudo cmake -D CMAKE_BUILD_TYPE=RELEASE -D BUILD_NEW_PYTHON_SUPPORT=ON -D INSTALL_C_EXAMPLES=ON -D INSTALL_PYTHON_EXAMPLES=ON -D BUILD_EXAMPLES=ON -D WITH_GTK=ON -D WITH_OPENNI=ON -D WITH_QT=ON -D WITH_TBB=ON -D BUILD_TBB=ON -D WITH_OPENGL=ON -D WITH_V4L=ON -D ENABLE_VFPV3=ON -D ENABLE_NEON=ON -D CMAKE_INSTALL_PREFIX=/usr/local -D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules ..

REMEMBER that for Odroid XU4 there will be OPENGL-ES instead of OPENGL.

If available on intel processors, opencv exploits a royalty free subset of intel's Integrated Performance Primitives (IPP) library (IPPICV). 
This library can be linked with the opencv during compilation and if done, then it replaces some of the corresponding low level optimized C code. 
For this you have to add the following command during installation, 'cmake -D WITH_IPP=ON'. The speed improvement can be a lot. But you have to have 
an intel processor for this, it will not happen on ARM processors.
]

Next, type the following (while inside the 'opencv/release' directory):

sudo make -j2   [the '-j2' runs 2 jobs in parallel. Change '2' to number of hardware threads available. 
                however, too many cores (like j4, j5 or j8) may make the BBB run out of memory]
                [you can also type only the 'make' command, but in that case the installation might take 
                more than two to four hours]

sudo make install

[This installations takes a lot of time and you should not have the laptop (when using ssh) to go to sleep mode, do not 
keep the installation unattended, press the 'spacebar' or 'enter' key sometimes while the installation is going on]

After installation is complete, the default installation path is '/usr/local/lib' and '/usr/local/include/opencv2'.
Hence you need to add the line '/usr/local/lib/' to the file '/etc/ld.so.conf.d/opencv.conf'.
If this file 'opencv.conf' is not there in this folder '/etc/ld.so.conf.d', then create this file 'opencv.conf' inside 
this folder and write this line '/usr/local/lib' (without the quotes). Then type the command 

sudo ldconfig

in the terminal and run it.
Or you can also add this line '/usr/local/lib' to the LD_LIBRARY_PATH environment variable; then you are done.

[If you dont do this then, you haven't put the shared library in a location where the loader can find it. look inside 
the '/usr/local/lib' folder and see if either of them contains any shared libraries (files beginning in lib and usually 
ending in .so). when you find them. create a file called /etc/ld.so.conf.d/opencv.conf and write to it the paths to the 
folders where the libraries are stored, one per line.]

Note If the size of the created library is a critical issue (like in case of an Android build) you can use the 
install/strip command to get the smallest size as possible. The stripped version appears to be twice as small. 
However, we do not recommend using this unless those extra megabytes do really matter.

---------------------------------------------
Once the opencv has been installed, you can try to import it in a python script and see if it works properly.
To check if the opencv contrib modules are installed or not, you can try running the following statements in a python script.

recognizer = cv2.face.LBPHFaceRecognizer_create()
detector = cv2.CascadeClassifier("haarcascade_frontalface_default.xml")
---------------------------------------------

==============================================================

Running a C++ code for opencv written in a .cpp file by gcc compiler in linux on BBB or laptop:
===============================================================================================
1. Make sure that the opencv is installed properly as per the above steps.
2. After the C++ program is written, assume that the name of the program file is 'test.cpp' and you want the the 
executable name to be 'test.exe'.
3. To compile it go inside the directory where the test.cpp file is.
4. Type  the following in the terminal: 
	g++ `pkg-config --cflags --libs opencv` -o test test.cpp
	g++ `[this is the left quote symbol, the one with the ~ key] pkg-config --cflags --libs opencv` [name of the libraries] -o test [name of the .exe file that you want to create] test.cpp [name of the program file]

if this does not work then try this out:
	g++ -o test test.cpp `pkg-config --cflags --libs opencv`
	g++ -o test [name of the .exe file that you want to create] test.cpp [name of the program file] `[this is the left quote symbol, the one with the ~ key] pkg-config --cflags --libs opencv` [name of the libraries]

This is because some of these libraries may be archived libraries and so if you have to put the files that need them 
(like 'test.cpp') in front of these libraries to meke them work

===============================================================================================

OpenCV2.4 installation instruction in Ubuntu 14.04 LTS on BBB or laptop:
========================================================================
http://www.samontab.com/web/2014/06/installing-opencv-2-4-9-in-ubuntu-14-04-lts/
[same procedure for laptop and BBB]

(did not work on BBB failed in the make step)

===============================================================================================

Installing zbar tools for qr codes and barcode detection:
===============================================================================================
(https://www.howtoinstall.co/en/ubuntu/trusty/zbar-tools)
(https://github.com/ZBar/ZBar)

sudo apt-get update
sudo apt-get install zbar-tools libzbar-dev

To run the code detection via command line, first save an image that has a qr code (e.g. sampleQR.png), the run the following command.
zbarimg sampleQR.png

This will give a message like 

QR-Code:http://adafru.it/qr
scanned 1 barcode symbols from 1 images in 0.02 seconds

The adafruit is the site from where the image is downloaded.
This only works for png files, not jpg files.
Check if the zbar.h file is included in the /usr/include folder.
If that is included, then you can include in the opencv code as "#include<zbar.h>". You also have to use the "using namespace zbar;" command and while compiling the code you have to use the 
following command
g++ -o sift_pattern_4 sift_pattern_4.cpp `pkg-config --cflags --libs opencv` -lzbar 
===============================================================================================

