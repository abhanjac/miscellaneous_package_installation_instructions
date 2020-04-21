# Installing PCL package for point cloud processing:

This package is used to process the point clouds from depth sensors and lidars.

Some references for installation can also be found in this [link](https://askubuntu.com/questions/916260/how-to-install-point-cloud-library-v1-8-pcl-1-8-0-on-ubuntu-16-04-2-lts-for).

#### Install Oracle-java8-jdk:
```
sudo add-apt-repository -y ppa:webupd8team/java && sudo apt update && sudo apt -y install oracle-java8-installer
```
But if this installation does not happen and incurs problems due to some errors, then proceed to the next steps, move on.

#### Install universal pre-requisites:
```
git clone https://github.com/Polyconseil/zbarlight.git
cd zbarlight
sudo python setup.py install
```

#### Run and Check:
Once the package has been installed, you can try to import it in a python script and see if it works properly.
You can check if it works by reading a qrcode file (if available) using the following code.

```
python
>>> from PIL import Image
>>> import zbarlight
>>> from PIL import Image
>>> import zbarlight




