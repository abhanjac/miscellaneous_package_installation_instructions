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
