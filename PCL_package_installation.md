# Installing PCL package for point cloud processing:

This package is used to process the point clouds from depth sensors and lidars.
The first part of the installation guide is for installing the PCL library for C++.
At the very end there is the instructions for installing the PCL library for Python.

Some references for installation can also be found in this [link](https://askubuntu.com/questions/916260/how-to-install-point-cloud-library-v1-8-pcl-1-8-0-on-ubuntu-16-04-2-lts-for).
Some references to understand the structure of the **CMakeLists.txt** file (which will be created later) can be found in this [link](https://answers.ros.org/question/237494/fatal-error-rosrosh-no-such-file-or-directory/).

#### Install Oracle-java8-jdk:
```
sudo add-apt-repository -y ppa:webupd8team/java && sudo apt update && sudo apt -y install oracle-java8-installer
```
But if this installation does not happen and incurs problems due to some errors, then proceed to the next steps, move on.

#### Install universal pre-requisites:
```
sudo apt -y install g++ cmake cmake-gui doxygen mpi-default-dev openmpi-bin openmpi-common libusb-1.0-0-dev libqhull* libusb-dev libgtest-dev
sudo apt -y install git-core freeglut3-dev pkg-config build-essential libxmu-dev libxi-dev libphonon-dev libphonon-dev phonon-backend-gstreamer
sudo apt -y install phonon-backend-vlc graphviz mono-complete qt-sdk libflann-dev
```

#### Install PCL v1.8 for Ubuntu 16.04.2:
```
sudo apt -y install libflann1.8 libboost1.58-all-dev

If the above does not work, then use the following command:

sudo apt -y install libflann-dev libboost-all-dev

cd ~/Downloads
wget http://launchpadlibrarian.net/209530212/libeigen3-dev_3.2.5-4_all.deb
sudo dpkg -i libeigen3-dev_3.2.5-4_all.deb
sudo apt-mark hold libeigen3-dev

wget http://www.vtk.org/files/release/7.1/VTK-7.1.0.tar.gz
tar -xf VTK-7.1.0.tar.gz
cd VTK-7.1.0 && mkdir build && cd build
cmake ..
make -j4
sudo make install

cd ~/Downloads
wget https://github.com/PointCloudLibrary/pcl/archive/pcl-1.8.0.tar.gz
tar -xf pcl-1.8.0.tar.gz
cd pcl-pcl-1.8.0 && mkdir build && cd build
cmake ..
make -j4
sudo make install

cd ~/Downloads
rm libeigen3-dev_3.2.5-4_all.deb VTK-7.1.0.tar.gz pcl-1.8.0.tar.gz
sudo rm -r VTK-7.1.0 pcl-pcl-1.8.0
```

#### Install PCL v1.8.1 for Ubuntu 17.10:
```
sudo apt -y install libflann1.9 libboost1.63-all-dev libeigen3-dev

cd ~/Downloads
wget http://www.vtk.org/files/release/8.0/VTK-8.0.1.tar.gz
tar -xf VTK-8.0.1.tar.gz
cd VTK-8.0.1 && mkdir build && cd build
cmake ..
make -j4
sudo make install

cd ~/Downloads
wget https://github.com/PointCloudLibrary/pcl/archive/pcl-1.8.1.tar.gz
tar -xf pcl-1.8.1.tar.gz
cd pcl-pcl-1.8.1 && mkdir build && cd build
cmake ..
make -j4
sudo make install

cd ~/Downloads
rm VTK-8.0.1.tar.gz pcl-1.8.1.tar.gz
sudo rm -r VTK-8.0.1 pcl-pcl-1.8.1
```

#### Create CMakeLists.txt for testing:
Once the package has been installed, you can try doing the following to check if it is running.

```
cd ~
mkdir pcl-test && cd pcl-test
```

Now create a CMakeLists.txt file:

```
touch CMakeLists.txt
```

Now write the following in the CMakeLists.txt file.

```
cmake_minimum_required(VERSION 2.8 FATAL_ERROR)
project(pcl-test)

# For PCL.
find_package(PCL 1.2 REQUIRED)
include_directories(${PCL_INCLUDE_DIRS})
link_directories(${PCL_LIBRARY_DIRS})
add_definitions(${PCL_DEFINITIONS})

# Adding adding executable and linking libraries.
add_executable(pcl-test main.cpp)
target_link_libraries(pcl-test ${PCL_LIBRARIES})

SET(COMPILE_FLAGS "-std=c++11")
add_definitions(${COMPILE_FLAGS})
```

#### Create a main.cpp file for testing:
Now create a main.cpp file to test.

```
cd ~/pcl-test
touch main.cpp
```

Now write the following in the main.cpp file.

```
#include <iostream>

int main() {
    std::cout << "hello, world!" << std::endl;
    return (0);
}
```

#### Compile the main.cpp file and test:
```
mkdir build && cd build
cmake ..
make -j4

./pcl-test
```

The correct output in the terminal will be:

hello, world!

#### Adding ROS to the file:

This [link](https://answers.ros.org/question/237494/fatal-error-rosrosh-no-such-file-or-directory/) also talks about how to include ROS.

Modify the CMakeLists.txt file.

```
cmake_minimum_required(VERSION 2.8 FATAL_ERROR)
project(pcl-test)

# For PCL.
find_package(PCL 1.2 REQUIRED)
include_directories(${PCL_INCLUDE_DIRS})
link_directories(${PCL_LIBRARY_DIRS})
add_definitions(${PCL_DEFINITIONS})

# For ROS.
find_package(catkin REQUIRED COMPONENTS roscpp rospy std_msgs geometry_msgs message_generation)
include_directories(${catkin_INCLUDE_DIRS})

# Adding executable and link libraries.
add_executable(pcl-test main.cpp)
target_link_libraries(pcl-test ${PCL_LIBRARIES})
target_link_libraries(pcl-test ${catkin_LIBRARIES})

SET(COMPILE_FLAGS "-std=c++11")
add_definitions(${COMPILE_FLAGS})
```

Modify the main.cpp file.

```
#include <iostream>
#include "ros/ros.h"

int main() {
    std::cout << "hello, world!" << std::endl;
    std::cout << "ROS header file is included." << std::endl;
    return (0);
}
```

Now compile the main.cpp file and retest:

```
cd build
cmake ..
make -j4

./pcl-test
```

The correct output in the terminal will be:

hello, world!
ROS header file is included.

# Installing PCL package for python:

The installation guide is found in this [link](https://python-pcl-fork.readthedocs.io/en/rc_patches4/install.html).

Run the following command:

```
pip install python-pcl
```

If there are errors, then try to update the pip version first.

```
sudo pip install --upgrade pip
```

Then run the **pip install python-pcl** command.

There may be an error like the following with the package called **Cython**.

```
ERROR: Could not find a version that satisfies the requirement Cyththon (from versions: none)
ERROR: No matching distribution found for Cyththon
```

If that happens, then use the following solution as suggested in this [link](https://stackoverflow.com/questions/53807511/pip-cannot-uninstall-package-it-is-a-distutils-installed-project) by running the following command:

```
pip install --ignore-installed Cython
```

Then run the following if the above command succeeds.

```
pip install python-pcl
```

#### Run and check:

```
python

>>> import pcl
>>> 
```
