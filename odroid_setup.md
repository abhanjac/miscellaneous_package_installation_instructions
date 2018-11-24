# Ubuntu 16.04 Install on Odroid XU4
* Reference: [Odroid XU4 Official Wiki](https://wiki.odroid.com/odroid-xu4/odroid-xu4)

## SD/eMMC Card Preparation
### Download & Flash Ubuntu 16 image
1. Format SD/eMMC card (10 - 15 minutes)
* Insert SD/eMMC card into Laptop/PC
* Erase & Format SD/eMMC card
  * **Windows 10**
    * Right-click SD/eMMC card drive and select **Format**
  * **Mac**
    * Open **Disk Utility**
    * Select the SD/eMMC card and click **Erase**
    * Set new format to FAT and leave other options as defaults
* Navigate to the [Odroid Ubuntu Image Archive](https://odroid.in/ubuntu_16.04lts/)
  * Download [ubuntu-16.04.3-4.9-mate-odroid-xu4-20171025.img.xz](https://odroid.in/ubuntu_16.04lts/ubuntu-16.04.3-4.9-mate-odroid-xu4-20171025.img.xz)
  * Download and install [Etcher](https://etcher.io/)
  * Insert SD/eMMC card
  * Open Etcher
  * Click **Select Image** and browse to the ***.img.xz** downloaded above
  * Select the SD/eMMC card (should be automatically selected)
  * Click **Flash**

### Resize the Root Partition to Fill the Whole SD/eMMC Card
1. Insert SD/eMMC card into Odroid XU4
* Connect Odroid to monitor via HDMI, and connect a keyboard and mouse
* Plug in the Odroid 5VDC power supply
* (If Necessary) Log in to the LUbuntu with the following credentials:
  * User: odroid
  * Password: odroid
* Type `df -h` in a terminal to check the size of the partition
  * If the total size isn’t 15/16G:
    * `sudo reboot` and it should auto-resize
    * Otherwise, resize the partition as follows:
      * `df -h` 
      * OUTPUT: Shows the partition sizes. Verify that partition size matches card capacity. If partition size does NOT match (note the sizes down for comparison, then do the following to expand the memory to full capacity):
      * `ls /dev` (to get name of partition, e.g. mmcblk0, mmcblk1) 
      * `sudo fdisk /dev/mmcblk0`
      * OUTPUT: List of options to modify partitions (type 'm' for help)
      * type **p** to see the partition table, type **d** to delete partition
      * When prompted for partition number, select 2nd partition by pressing 2
      * type **p** to verify 2nd partition is deleted
      * type **n** to create a new partition
      * type **p** to make a primary partition
      * type **ENTER** through the defaults until return to main menu
      * type **w** to write these changes
      * `sudo reboot`
      * `sudo resize2fs /dev/mmcblk0p2`
      * `df -h`
      * OUTPUT: Partition size should now be ~15 GB.
      
## Configure Applications & Install Kernel
### Install basic applications 
1. ```
  sudo apt update
  sudo apt install git kate chrony screen unzip tightvncserver v4l-utils htop
  ```
* Configure **Chrony**
  * This application will synchronize the clocks between multiple machines on the same network. This is necessary since the Time class in ROS uses the system clock for timing, and the system clock may differ between two machines, even if they are both using the same ntp server. Thus making it impossible to accurately compare timestamps between topics sent from different machines.
  * References: [1](https://chrony.tuxfamily.org/documentation.html), [2](https://wiki.archlinux.org/index.php/Chrony), [3](http://cmumrsdproject.wikispaces.com/file/view/ROS_MultipleMachines_Wiki_namank_fi_dshroff.pdf), [4](http://wiki.ros.org/ROS/NetworkSetup), [5](https://github.com/pr2-debs/pr2/blob/master/pr2-chrony/root/unionfs/overlay/etc/chrony/chrony.conf), [6](https://www.digitalocean.com/community/tutorials/how-to-set-up-time-synchronization-on-ubuntu-16-04)
  * To check the time difference between machines, use:
    * `ntpdate -q <IP address of other machine>` 
  * Turn off **timesyncd**
    * `sudo timedatectl set-ntp no` 
  * Configure Chrony (make these changes in **/etc/chrony/chrony.conf**)
    * Add Purdue’s ntp servers
      * ```
      sudo sed -i 's/pool 2.debian.pool.ntp.org offline iburst$/pool 2.debian.pool.ntp.org offline iburst\nserver ntp3.itap.purdue.edu offline iburst\nserver ntp4.itap.purdue.edu offline iburst/' /etc/chrony/chrony.conf
      ```
  * Set up chrony server for other devices
    * ```
    sudo sed -i 's/#local stratum 10$/local stratum 10/' /etc/chrony/chrony.conf
    sudo sed -i 's~#allow ::/0 (allow access by any IPv6 node)$~#allow ::/0 (allow access by any IPv6 node)\nallow 192.168~' /etc/chrony/chrony.conf
    ```
  * Allow for step time changes at startup (for initial time setup)
    * ```
    su
    echo -e “# In first three updates step the system clock instead of slew\n# if the adjustment is larger than 10 seconds\nmakestep 10 3” | tee -a /etc/chrony/chrony.conf > /dev/null
    exit
    ```
  * Set timezone
    * Get the exact name of the desired timezone (**America/Indiana/Indianapolis**)
      * `timedatectl list-timezones`
    * `sudo timedatectl set-timezone America/Indiana/Indianapolis`
    * `date` 
  * Restart chrony to apply changes
    * `sudo systemctl restart chrony` 
* Configure **Kate**
  * Top Menu --> Settings --> Configure Kate
    * Application
      * Sessions --> Check **Load last used session**
    * Editor Component
      * Appearance --> Borders --> Check **Show line numbers**
      * Fonts & Colors --> Font --> **Size: 10**
      * Editing --> General --> Check **Show static word wrap marker**
      * Editing --> General --> Check **Enable automatic brackets**
      * Editing --> Indentation --> **Tab width: 4 characters**
      * Editing --> Indentation --> **Keep extra spaces**
  * Click **Apply** then **OK** 
* Configure **Terminal**
  * Top Menu --> Edit --> Profile Preferences
    * Scrolling --> Check **Unlimited**
* Configure **File Manager**
  * System --> Preferences --> Personal --> File Management
    * Views --> View new folders using: **List View**

### Configure Autologin and VNC server
1. **Automatic Login**
  * Add the following lines to ‘etc/lightdm/lightdm.conf’
  ```
  [SeatDefaults]
  autologin-user=odroid
  autologin-user-timeout=0
  ```
* Setup **TightVNCServer**
  * References: [1](https://ubuntu-mate.community/t/accessing-rpi2-remotely/3074/7), [2](https://askubuntu.com/questions/506580/how-to-configure-a-password-for-lightdm-vnc-connection)
  * Set up the vnc password: `vncpasswd`
    * Password: **odroid**
    * View-only password: **No**
  * Configure the VNC server to work with Ubuntu MATE
    * `sudo nano ~/.vnc/xstartup`
    * Add the following lines:
      * `unset DBUS_SESSION_BUS_ADDRESS` (at top, just after #!...)
      * `/usr/bin/mate-session &` (at the end of the file)
  * To run the server: `vncserver`
    * This will output:
    `New 'X' desktop is odroid:1`
    * Calling `vncserver` again will start new servers at `:2`, `:3`, etc.
    * View server at `<IP Address>:5901`
      * **NOTE:** `5901` is for `:1`, `5902` for `:2`, etc.
    * [Viewer application (Java)](https://www.tightvnc.com/download/2.8.3/tvnjviewer-2.8.3-bin-gnugpl.zip)
  * To kill the server: `vncserver -kill :1`
    * **NOTE:** `:1` above indicates index number (could be `:1`, `:2`...etc.)
    
### Install Python Packages
1. **Pip**
  * ```
  sudo apt install python-pip
  pip install --upgrade pip
  ```
* **PySerial** (for Python access to serial ports)
  * `python -m pip install pyserial`

### Download and Install latest 4.9 Kernel (and Headers)
  * References: [Kernel Compliation](https://debian-handbook.info/browse/stable/sect.kernel-compilation.html), [Kernel Installation](https://debian-handbook.info/browse/stable/sect.kernel-installation.html)

1. `sudo apt update`
* `sudo apt install git kate chrony screen unzip`
  * **Note:** **kernel-package** is a large package > 1GB, and may take some time to install
  * **Note:** make-kpkg (referenced in many forums) is obsolete, changed to make deb-pkg which automatically takes care of initrd and version naming… requires `sudo apt install kernel-package` (takes ~6 min. to install)
* OPTION 1 (Use pre-built `.deb` files to install Kernel)
  1. Copy the following files onto the Desktop (from [./OdroidLinuxDebians/](./OdroidLinuxDebians/))
    * `linux-image-4.9.61+_4.9.61+-2_armhf.deb`
    * `linux-headers-4.9.61+_4.9.61+-2_armhf.deb`
  * ```
  cd ~/Desktop
  sudo dpkg -i linux-image-4.9.61+_4.9.61+-2_armhf.deb
  sudo dpkg -i linux-headers-4.9.61+_4.9.61+-2_armhf.deb
  sudo reboot
  ```
* OPTION 2 (Build/Install Kernel from source)
  1. Download the Kernel source code
    ```
    cd ~/Desktop
    git clone --depth 1 https://github.com/hardkernel/linux -b odroidxu4-4.9.y
    cd ~/Desktop/linux
    ```
  * Modify the Odroid XU4 defconfig so it automatically builds necessary drivers
    * **ath9k_htc Driver** (for Netgear WNA1100 WiFi adapter)
      ```
      sed -i 's/# CONFIG_ATH9K_HTC is not set/CONFIG_ATH9K_HTC=m/' ./arch/arm/configs/odroidxu4_defconfig
      ```
    * **RTL8812AU Driver** (for Netgear AC600 WiFi adapter)
      ```
      sed -i 's/# CONFIG_RTL8812AU is not set/CONFIG_RTL8812AU=m/' ./arch/arm/configs/odroidxu4_defconfig
      ```
    * **FTDI USB Driver** 
      ```
      sed -i 's/# CONFIG_USB_SERIAL is not set/CONFIG_USB_SERIAL=m\nCONFIG_USB_SERIAL_FTDI_SIO=m/' ./arch/arm/configs/odroidxu4_defconfig
      ``` 
    * **Webcam Drivers** (UVC: USB Video Class)
    ```
    sed -i 's/# CONFIG_MEDIA_CAMERA_SUPPORT is not set/CONFIG_MEDIA_CAMERA_SUPPORT=y/' ./arch/arm/configs/odroidxu4_defconfig
    sed -i 's/# CONFIG_MEDIA_USB_SUPPORT is not set/CONFIG_MEDIA_USB_SUPPORT=y\nCONFIG_USB_VIDEO_CLASS=m\nCONFIG_USB_GSPCA=y\nCONFIG_USB_GSPCA_VC032X=m/' ./arch/arm/configs/odroidxu4_defconfig
    ```
  
    ```
    make odroidxu4_defconfig
    sudo make -j8 deb-pkg
    cd ~/Desktop
    sudo dpkg -i linux-image-4.14.55+_4.14.55+-1armhf.deb
    sudo dpkg -i linux-headers-4.14.55+_4.14.55+-1armhf.deb
    sudo rm -r linux
    ```
    
### Install Driver for Netgear AC600 USB-WiFi adapter
1. Download the driver
  * ```
  cd ~ 
  git clone https://github.com/scrivy/rtl8812AU_8821AU_linux.git  
  cd rtl8812AU_8821AU_linux
  ```
* Configure Makefile for Odroid (ARM platform, similar to Raspberry Pi)
  * Change `CONFIG_PLATFORM_I386_PC = y` to `CONFIG_PLATFORM_I386_PC = n`
  * Change `CONFIG_PLATFORM_ARM_RPI = n` to `CONFIG_PLATFORM_ARM_RPI = y`
* Build and install
  * ```
  make -j8
  sudo make install 
  sudo modprobe rtl8812au 
  ```
  
### Install Kinect Driver (OpenKinect/libfreenect)
1. Install the driver: `sudo apt install freenect`
* Download the udev rules
  ```
  cd /etc/udev/rules.d/
  sudo wget https://github.com/OpenKinect/libfreenect/blob/master/platform/linux/udev/51-kinect.rules
  ```
* Download & Install SensorKinect Source
  ```
  cd ~/
  git clone https://github.com/OpenKinect/libfreenect
  cd libfreenect
  mkdir build
  cd build
  cmake -L -DCMAKE_BUILD_PYTHON=ON ..
  make -j8
  sudo make install
  sudo ldconfig /usr/local/lib
  ```

## Install OpenCV 3.4
  * Reference: [OpenCV Docs](https://docs.opencv.org/3.4.3/d7/d9f/tutorial_linux_install.html)

1. Install Dependencies
  ```
  sudo apt install build-essential cmake git libgtk2.0-dev pkg-config libavcodec-dev \
       libavformat-dev libswscale-dev python-dev python-numpy libtbb2 libtbb-dev \
       libjpeg-dev libjasper-dev libdc1394-22-dev checkinstall yasm libxine2-dev \
       libv4l-dev libav-tools unzip libdc1394-22 libpng12-dev libtiff5-dev tar dtrx \
       libopencore-amrnb-dev libopencore-amrwb-dev libtheora-dev libvorbis-dev \
       libxvidcore-dev x264 libgstreamer0.10-dev libgstreamer-plugins-base0.10-dev \
       libqt4-dev libmp3lame-dev
  ```
* Download OpenCV and OpenCV contrib source code
  ```
  cd ~
  mkdir OpenCV
  cd OpenCV
  git clone -b 3.4 --single-branch https://github.com/opencv/opencv.git
  git clone -b 3.4 --single-branch https://github.com/opencv/opencv_contrib.git
  ```
  
* Build from source (~40 minutes)
  ```
  cd ~/opencv
  mkdir release
  cd release
  sudo cmake -D CMAKE_BUILD_TYPE=RELEASE -D BUILD_NEW_PYTHON_SUPPORT=ON -D INSTALL_C_EXAMPLES=ON -D INSTALL_PYTHON_EXAMPLES=ON -D BUILD_EXAMPLES=ON -D WITH_GTK=ON -D WITH_QT=ON -D WITH_GSTREAMER_0_10=ON -D WITH_TBB=ON -D BUILD_TBB=ON -D WITH_OPENGL-ES=ON -D WITH_V4L=ON -D ENABLE_VFPV3=ON -D ENABLE_NEON=ON -D CMAKE_INSTALL_PREFIX=/usr/local -D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules ..
  sudo make -j8
  sudo make install
  ```
* Create config file (if it doesn't exist)
  * `sudo nano /etc/ld.so.conf.d/opencv.conf`
  * Add `/usr/local/lib/` to the file
  * `sudo ldconfig`
* Test installation with Python
  ```
  python
  import cv2
  recognizer = cv2.face.LBPHFaceRecognizer_create()
  detector = cv2.CascadeClassifier("haarcascade_frontalface_default.xml")
  ```
* Remove OpenCV source to free up SSD storage
  * `sudo rm -r ~/OpenCV`

## Install ROS Kinetic (~20 minutes)
  * Reference: [ROS Wiki](http://wiki.ros.org/kinetic/Installation/Ubuntu)

1. Set up
  ```
  sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
  sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116
  sudo apt update
  ```
  
* Install ROS desktop version and additional packages
  ```
  sudo apt install ros-kinetic-desktop
  sudo apt install python-rosdep
  rosdep init
  rosdep update
  sudo chown -R odroid:odroid /opt/ros
  sudo apt install python-rosinstall ros-kinetic-mavros ros-kinetic-vision-opencv
  ```
  
* Create Catkin workspace
  ```
  source /opt/ros/kinetic/setup.bash
  mkdir -p ~/ros_ws/src
  cd ~/ros_ws/
  catkin_make
  ```
  
* Set up ROS Environment (auto-load in bashrc)
  ```
  su
  echo "# ROS Settings" | tee -a /home/odroid/.bashrc > /dev/null
  echo "source /opt/ros/kinetic/setup.bash" | tee -a /home/odroid/.bashrc > /dev/null
  echo "source /home/odroid/ros_ws/devel/setup.bash" | tee -a /home/odroid/.bashrc > /dev/null
  echo "export ROS_MASTER_URI=http://localhost:11311" | tee -a /home/odroid/.bashrc > /dev/null
  echo "#export ROS_MASTER_URI=http://192.168.1.88:11311" | tee -a /home/odroid/.bashrc > /dev/null
  echo "#export ROS_IP=192.168.1.106" | tee -a /home/odroid/.bashrc > /dev/null
  exit
  ```
  
* Set up for Turtlebot 2
  * Reference: [Turtlebot ROS Wiki](http://wiki.ros.org/turtlebot/Tutorials/indigo/Turtlebot%20Installation)
  * Install Turtlebot packages
    ```
    sudo apt install ros-kinetic-turtlebot ros-kinetic-turtlebot-apps \
      ros-kinetic-turtlebot-interactions ros-kinetic-turtlebot-simulator \
      ros-kinetic-kobuki-ftdi ros-kinetic-ar-track-alvar-msgs
    ```
  * Prepare **udev rules** for the Kobuki base
    ```
    . /opt/ros/kinetic/setup.bash
    rosrun kobuki_ftdi create_udev_rules
    ```
    
  * Identify the Kinect (or R200) as the 3D sensor
    * Add the following ABOVE the **"source .../setup.bash"** lines in **~/.bashrc**
    * ```
      export TURTLEBOT_3D_SENSOR=kinect
      #export TURTLEBOT_3D_SENSOR=r200
      ```
      
* Set up for 3D Mapping and Path-Planning
  * Install [OctoMap](https://www.ros.org/wiki/octomap_server)
    * `sudo apt install ros-kinetic-octomap-server`
  * Install [rtabmap_ros](http://wiki.ros.org/rtabmap_ros)
    * `sudo apt install ros-kinetic-rtabmap-ros`
      
## Install librealsense (for Intel RealSense R200)
  * Reference: [librealsense GitHub](https://github.com/IntelRealSense/librealsense/blob/v1.12.1/doc/installation.md)
  * Note: To make the R200 work, the **librealsense** driver and the **pyrealsense** python package must both be installed.

1. Install Dependencies
```
sudo apt update
sudo apt install libusb-1.0-0-dev pkg-config libgtk-3-dev libglfw3-dev cython
```

* Download source code from: `./librealsense`
```
cd ~
<copy librealsense folder to root directory>
cd librealsense
```

* Configure installation of librealsense:
  * Open the file **CMakeLists.txt** in the **librealsense** directory
  * Toggle the options as shown below:
  ```
  option(BUILD_UNIT_TESTS "Build realsense unit tests." OFF)
  option(BUILD_EXAMPLES "Build realsense examples and tools." OFF)
  ```
  
* Build and Install
```
mkdir build
cd build
cmake ../
make -j8 
sudo make install 
```

  * **NOTE:** to uninstall, use: `sudo make uninstall && make clean`
  
* Update the **PYTHONPATH** env. variable (add path to pyrealsense lib)
  * Add the following line to **~/.bashrc**
  * `export PYTHONPATH=$PYTHONPATH:/usr/local/lib`
  
* Add the **udev rules** for the RealSense
  ```
  cd ..
  sudo cp config/99-realsense-libusb.rules /etc/udev/rules.d/ 
  sudo udevadm control --reload-rules 
  sudo udevadm trigger 
  sudo apt-get install libssl-dev 
  ./scripts/patch-realsense-ubuntu-xenial.sh 
  ```

* Verify install 
  * dmesg log should indicate a new uvcvideo driver has been registered
  * `sudo dmesg | tail -n 50`

* Install the **pyrealsense** Python package
  * Download the zipped code: `./pyrealsense-2.2.tar.gz`
  ```
  tar -xzvf pyrealsense-2.2.tar.gz
  cd pyrealsense-2.2
  sudo python setup.py install
  ```
  
* Simple Run and Check
  * Check if pyrealsense can be imported into python.
  ```
  python
  >>> import pyrealsense as pyrs
  >>>
  ```
  
* Full test of R200 color and depth streams
  * Reference: [link](http://pyrealsense.readthedocs.io/en/master/pyrealsense.html#module-pyrealsense.core)
  
  ```
  #!/usr/bin/env python

  import cv2, numpy as np, pyrealsense as pyrs

  ## start the service - also available as context manager
  serv = pyrs.Service()

  ## create a device from device id and streams of interest
  Streams = [ pyrs.stream.ColorStream( fps = 60, color_format = 'bgr' ), pyrs.stream.DepthStream( fps = 60 ) ]
  cam = serv.Device( device_id = 0, streams = Streams )

  ## retrieve frames.
  while True:
      cam.wait_for_frames()
      color = cam.color
      cv2.imshow( 'color', color )
    
      depth = cam.depth   
      # The depth data is an 16 bit integer. Each pixel value represents the depth in mm. 
      # The realsense r200 is good for depth between 0.5 m to 1.5 m. It can see upto 2 m but image is not good.
      depth1 = np.array( depth, dtype = np.uint8 ).reshape(( depth.shape[0], depth.shape[1], 1 ))     
      # To display it using colormap, it is converted into 8 bit, but it loses resolution.
      depth1 = cv2.applyColorMap( depth1, cv2.COLORMAP_JET )
      cv2.imshow( 'depth', depth1 )
    
      cv2.waitKey(1)

  ## stop camera and service
  cam.stop()
  serv.stop()
  ```

## Install Additional Python Packages
1. **SciKitLearn**
  * Install dependencies
    ```
    sudo apt install python-numpy python-scipy python-matplotlib ipython \
         ipython-notebook python-pandas python-sympy python-nose
    ```
  * Install Sklearn
    * `pip install -U scikit-learn`
    
* **SciKitImage**
  * Install dependencies
    ```
    sudo apt install python-matplotlib python-numpy python-pil python-scipy \
         build-essential cython
    ```
  * Install Skimage
    `pip install -U scikit-image`
    
* **QRcode**
  * Install qrcode
    ```
    sudo pip install qrcode
    ```
    
* **Zbarlight**
  * Install dependencies
    ```
    sudo apt install libzbar0 libzbar-dev
    ```
  * Install zbarlight
    ```
    git clone https://github.com/Polyconseil/zbarlight.git
    cd zbarlight
    sudo python setup.py install
    ```
    
* **Dronekit**
  * Install dependencies
    (These are optional, needed only if the installation fails.)
    ```
    sudo apt install python-dev libxml2-dev libxslt1-dev zlib1g-dev build-essential \
         autoconf libtool pkg-config python-opengl python-imaging python-pyrex \
         python-pyside.qtopengl idle-python2.7 qt4-dev-tools qt4-designer libqtgui4 \
         libqtcore4 libqt4-xml libqt4-test libqt4-script libqt4-network libqt4-dbus \
         python-qt4 python-qt4-gl libgle3 python-dev libssl-dev
    sudo pip install future greenlet gevent
    ```
  * Install dronekit
    ```
    git clone https://github.com/dronekit/dronekit-python.git
    cd ./dronekit-python
    sudo python setup.py install
    ```

* To uninstall pip packages, use:
  * `python setup.py install --record files.txt`
  * `files.txt` will list all files created by `setup.py install`, delete these files

## Install Pytorch
  * Reference: [Pytorch github](https://github.com/pytorch/pytorch#from-source)
  * Reference to install libffi-dev: [libffi-dev](https://github.com/markwal/OctoPrint-PolarCloud/issues/19)

1. Install Dependencies
  ```
  sudo apt install python-numpy cmake gcc libopenblas-dev cython libatlas-dev m4 \
                   libblas-dev libffi-dev
  sudo pip install pyyaml typing cffi
  sudo pip install -U pip setuptools
  sudo pip install leveldb==0.18
  ```
* Download Pytorch from source
  Go to [pytorch.org](pytorch.org)
  Select the options **Pytorch Build**: Stable, **OS**: Linux, **Package**: Source, **Language**: Python 2.7, **CUDA**: None
  Go to the github page suggested by **Run this command**.
  The most updated instructions are here.
  
  ```
  sudo git clone --recursive https://github.com/pytorch/pytorch.git
  ```
* Build from source (~60 minutes)
  ```
  cd pytorch
  sudo git submodule update --init
  export NO_CUDA=1
  export NO_DISTRIBUTED=1
  sudo -E MAX_JOBS=2 python setup.py install
  ```
  If this installation fails (may fail even at 99%) because of an error like the following:
  ```
  ../../lib/libtorch.so.1: undefined reference to `dlclose'
  ../../lib/libtorch.so.1: undefined reference to `dlsym'
  ../../lib/libtorch.so.1: undefined reference to `dlopen'
  ../../lib/libtorch.so.1: undefined reference to `dlerror'
  ```
  Then first clean the last build, and use the following command for installation.
  ```
  sudo python setup.py clean
  sudo -E MAX_JOBS=2 LDFLAGS="-Wl,--no-as-needed -ldl" python setup.py install
  ```
* Install torchvision
  (In separate directory, not inside pytorch source directory)
  ```
  cd ~
  git clone https://github.com/pytorch/vision.git
  cd vision
  sudo python setup.py install
  ```
* Test installation with Python
  (Get out of the source pytorch directory first)
  ```
  cd ~
  python
  import torch
  x = torch.rand(5, 3)
  print(x)
  ```

## Re-install pip and downgrade numpy from 1.15.2 to 1.11.0
  * If pip stops working (e.g., ImportError: cannot import main...), [reinstall pip](https://pip.pypa.io/en/stable/installing/)
  ```
  curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
  sudo python get-pip.py
  ```
  * Later versions of numpy have a bug which causes pyrealsense to break
    * e.g., "ValueError: '<P' is not a valid PEP 3118 buffer format string"
    * Reference: [bug](https://github.com/toinsson/pyrealsense/issues/82)
    * `sudo pip install numpy==1.11.0`

## General udev rule setup
  1. Obtain device info
    * References: [Debian Wiki: udev](https://wiki.debian.org/udev), [Overview](http://reactivated.net/writing_udev_rules.html#udevinfo)
    * `udevadm info --attribute-walk --name /dev/<device_name>`
  * Place udev rule files in: `/etc/udev/rules.d/`
    * Example file: `99-usb-serial.rules` 
    * ```
    SUBSYSTEM=="tty", ATTRS{idVendor}=="0403", ATTRS{idProduct}=="6001", ATTRS{serial}=="AI04XENV", SYMLINK+="ttyFTDI3_5"
    SUBSYSTEM=="tty", ATTRS{idVendor}=="0403", ATTRS{idProduct}=="6001", ATTRS{serial}=="AK05C4L0", SYMLINK+="ttyFTDI5"
    ```
    
## Video4Linux Webcam Driver Usage
  * Show available options for webcam 
    * `v4l2-ctl --list-formats-ext`  (shows options, with associated fps)
  * Set video output format
    * `v4l2-ctl --set-fmt-video=width=1920,height=1080,pixelformat=YUYV`
  * Show video capture options
    * `v4l2-ctl --help-vidcap (shows video capture options)`
  * Automatically configure webcam on plug-in
    * Create udev rule in `/etc/udev/rules.d/99-webcam.rules`
    * `udevadm info --attribute-walk --name /dev/video0`
    * `SUBSYSTEM=="video4linux", SUBSYSTEMS=="usb", ATTRS{idVendor}=="05a3", ATTRS{idProduct}=="9230", PROGRAM="/usr/bin/v4l2-ctl --set-fmt-video=width=640,height=480,pixelformat=MJPG --device /dev/%k"`