# Installing Zbar package for reading QRcodes:

The **qrcode decoder** package is obtained in this [link](https://pypi.python.org/pypi/zbarlight)

This package can decode the information inside a qrcode when it is fed into the functions in this package using a python script.
But it can only decode the image of a pure qrcode, i.e. the image should entirely contain only the qrcode. Qrcode in a part of an image cannot be decoded using zbarlight.

zbarlight is a simple wrapper for the zbar library. For now, it only allows to read QR codes but contributions, suggestions and pull requests are welcome.
zbarlight is compatible with Python 2 and Python 3.
zbarlight is hosted on Github at this [link](https://github.com/Polyconseil/zbarlight/).

#### Install Dependencies:
```
sudo apt-get install libzbar0 libzbar-dev
```

#### Install Zbarlight:
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
