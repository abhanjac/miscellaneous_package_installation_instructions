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
sudo apt-get install libgstreamer0.10-dev libgstreamer-plugins-base0.10-dev libqt4-dev libmp3lame-dev libgtk2.0-dev
```

#### Getting OpenCV Source Code:
```
git clone -b 3.4 --single-branch https://github.com/opencv/opencv.git
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
git clone -b 3.4 --single-branch https://github.com/opencv/opencv_contrib.git
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
sudo cmake -D CMAKE_BUILD_TYPE=RELEASE -D WITH_OPENNI=ON -D WITH_QT=ON -D WITH_TBB=ON -D BUILD_TBB=ON -D WITH_V4L=ON -D ENABLE_VFPV3=ON -D ENABLE_NEON=ON -D WITH_GSTREAMER_0_10=ON -D CMAKE_INSTALL_PREFIX=/usr/local ..
```

To also activate **OpenGL ( for Laptop )**:

[ **NOTE:** OPENGL is used inside OpenCV for activating the OpenGL package for graphics. ]

Add the option `-D WITH_OPENGL=ON`

```
sudo cmake -D CMAKE_BUILD_TYPE=RELEASE -D WITH_OPENNI=ON -D WITH_QT=ON -D WITH_TBB=ON -D BUILD_TBB=ON -D WITH_OPENGL=ON -D WITH_V4L=ON -D ENABLE_VFPV3=ON -D WITH_GSTREAMER_0_10=ON -D ENABLE_NEON=ON -D CMAKE_INSTALL_PREFIX=/usr/local ..
```

To activate  **OpenGL ( for Odroid XU4 )**:

Add the option `-D WITH_OPENGL-ES=ON`.  **ES** meaning for Embedded system.

```
sudo cmake -D CMAKE_BUILD_TYPE=RELEASE -D WITH_OPENNI=ON -D WITH_QT=ON -D WITH_TBB=ON -D BUILD_TBB=ON -D WITH_OPENGL-ES=ON -D WITH_V4L=ON -D ENABLE_VFPV3=ON -D WITH_GSTREAMER_0_10=ON -D ENABLE_NEON=ON -D CMAKE_INSTALL_PREFIX=/usr/local ..
```

If some of these options throws errors, then remove those options.

#### Building OpenCV from Source Using CMake with Contrib packages:
[ If you also want to enable the opecv_contrib packages, then you have to add the following line as well.

`-D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules ../`

You have to provide the path to the **opencv_contrib/modules** folder. Since at the time of installation we will; be inside the **opencv/release** folder, and the 
**opencv_contrib** folder is inside the same directory as the **opencv** folder (which may be the home directory), that is why we keep the path as `../../opencv_contrib/modules`. 


# Installing Anaconda:

Sometimes the Opencv and Tensorflow (which we will be installing later) does not work with the latest version of Anaconda.
Hence we install the version 3.5 for now.

In case you have installed another version and want to remove it, then you have to do the following:

#### Remove the previous version of anaconda (if any) directory, and the hidden directories.

```
sudo rm -r anaconda3    [ or whatever the name of the installation directory of the anaconda was ]
sudo rm -r .conda       [ this is a hidden folder created in the home directory ]
sudo rm .condarc        [ this is a hidden file in the home directory which sometimes is create and sometimes not depending on the version of Anaconda ]
```

Also try to remove the path which is written by the anaconda in the .bashrc file in the home directory.

The older versions of anaconda are found in the following [link](https://repo.anaconda.com/archive/).

[ Tensorflow was not getting installed in the anaconda version 3.7 which is available as latest on the anaconda website.
Hence the anaconda was uninstalled and the 3.5 version of anaconda was installed again from the archive as suggested by this [link](http://docs.anaconda.com/anaconda/user-guide/faq/#how-do-i-get-the-latest-anaconda-with-python-3-5)]

#### Download the '.sh' file and run it like this:

```
./Anaconda3-5.0.0-Linux-x86_64.sh       [ dont use sudo here ]
```

#### This will make the anaconda the default python version.

```
source .bashrc
```

### Install opencv:

```
conda install -c conda-forge opencv
pip install opencv-contrib-python
```

Once the opencv has been installed, you can try to import it in a python script and see if it works properly.
To check if the opencv contrib modules are installed or not, you can try running the following statements in a python script.

```
python
>>> import cv2
>>>
>>> recognizer = cv2.face.LBPHFaceRecognizer_create()
>>> detector = cv2.CascadeClassifier("haarcascade_frontalface_default.xml")
```

### Install Tensorflow cpu version:

```
pip install tensorflow
```

#### Also had to install msgpack:

```
pip install msgpack
```

### Install Pytorch and Torchvision:

```
conda install pytorch-cpu torchvision-cpu -c pytorch
```
