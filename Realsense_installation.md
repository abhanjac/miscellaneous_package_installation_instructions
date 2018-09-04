# Realsense installation instruction in Ubuntu 16.04 LTS on laptop or Odroid XU4:

**These instruction is for using the Realsense R200.**

To make the Realsense R200 work, the **librealsense** and the python package **pyrealsense** packages must be installed.

Instructions can be found in [Instructions link](https://github.com/IntelRealSense/librealsense/blob/master/doc/installation.md).
But the following instructions also have added options for a more elaborate installation and how to cope with some errors while installing the packages.

#### Install Dependencies:
```
sudo apt-get update
sudo apt-get install libusb-1.0-0-dev pkg-config libgtk-3-dev libglfw3-dev
```

#### Clone repository:
```
git clone https://github.com/IntelRealSense/librealsense.git 
cd librealsense 
```

#### Installation of librealsense:

Now open the file **CMakeLists.txt** in the **librealsense** directory, and toggle option the following options to the format below:

```
option(BUILD_UNIT_TESTS "Build realsense unit tests." OFF)
option(BUILD_EXAMPLES "Build realsense examples and tools." ON)
option(ENFORCE_METADATA "Require WinSDK with Metadata support during compilation. Windows OS Only" OFF)
option(BUILD_PYTHON_BINDINGS "Build Python bindings" ON)
option(BUILD_CV_EXAMPLES "Build OpenCV examples" OFF)
option(BUILD_PCL_EXAMPLES "Build PCL examples" OFF)
option(BUILD_NODEJS_BINDINGS "Build Node.js bindings" OFF)
```

This prevents a compiler error with one of the unit tests during build process and activates the python wrapper.

```
mkdir build
cd build
cmake ../
make -j8 
sudo make install 
```

Update the **PYTHONPATH** environment variable to add the path to the pyrealsense library.

The following line has to be written to the bashrc file.
```
export PYTHONPATH=$PYTHONPATH:/usr/local/lib
```

Then do the following:

```
cd ..
sudo cp config/99-realsense-libusb.rules /etc/udev/rules.d/ 
sudo udevadm control --reload-rules 
sudo udevadm trigger 
sudo apt-get install libssl-dev 
./scripts/patch-realsense-ubuntu-xenial.sh 
```

Check dmsg to verify install (the log should indicate that a new uvcvideo driver has been registered).

```
sudo dmesg | tail -n 50 
```

#### Installation of pyrealsense package (Method 1):

This package will access the Realsense R200 camera.

Download the compressed file from the following [this](https://pypi.python.org/pypi/pyrealsense/2.2) location and decompress it and put it in the home directory.
Then, 

```
cd <pyrealsense directory>
sudo python setup.py install
```

[ **NOTE:** Basically this way you can install any package that is actually a pip package but for some reason the user does not want or cannot install using pip.
Here is a [link](https://stackoverflow.com/questions/13270877/how-to-manually-install-a-pypi-module-without-pip-easy-install) to how manually install pip package. ]

#### Installation of pyrealsense package (Method 2):

This package can also be installed using the pip install directly (by doing the following):

[ **NOTE:** This is the better way. But this way it gets installed in Python 3 inside anaconda, if the pip is installed in anaconda python 3. ]

```
sudo apt-get install cython
pip install pycparser
pip install pyrealsense
```

#### Run and Check:
Check if the pyrealsense can be imported into python.

```
python
>>> import pyrealsense as pyrs
>>>
```

Download and keep the complete set of source files by `git clone https://pypi.python.org/pypi/pyrealsense/2.2`.
This will contain all the source files and hence will have the details of the special classes and functions that are used to access the realsense camera. 

The example python code given in the above [link](https://pypi.python.org/pypi/pyrealsense/2.2) should also work when all the installations are over.

This code will show both the depth and the color images.
Documentation at this [link](http://pyrealsense.readthedocs.io/en/master/pyrealsense.html#module-pyrealsense.core).

#### Example code to check the working of the Realsense R200:

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


