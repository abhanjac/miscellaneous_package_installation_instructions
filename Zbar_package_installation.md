# Installing Zbar package for reading QRcodes:

The **qrcode decoder** package is obtained in this [link](https://pypi.python.org/pypi/zbarlight)

This package can decode the information inside a qrcode when it is fed into the functions in this package using a python script.
But it can only decode the image of a pure qrcode, i.e. the image should entirely contain only the qrcode. Qrcode in a part of an image cannot be decoded using zbarlight.

zbarlight is a simple wrapper for the zbar library. For now, it only allows to read QR codes but contributions, suggestions and pull requests are welcome.
zbarlight is compatible with Python 2 and Python 3.
zbarlight is hosted on Github at this [link](<https://github.com/Polyconseil/zbarlight/).

#### Install Dependencies:



#### Alternative installation command:

You can directly do a `sudo pip install qrcode` 

#### Run and Check:
Once the package has been installed, you can try to import it in a python script and see if it works properly.

```
python
>>> import qrcode
>>>
```

You can create a qrcode for any string by the command `img = qrcode.make('string')`. This will create a **pil** image in python.
You can display the image by `img.show()` and can save it as a **.png** or **.jpg** image by the command `img.save('name_of_file.extension')`.

#### Advanced Usage:

For more control, use the QRCode class. For example:

```
import qrcode
qr = qrcode.QRCode(
    version=1,
    error_correction=qrcode.constants.ERROR_CORRECT_L,
    box_size=10,
    border=4,
)
qr.add_data('Some data')
qr.make(fit=True)

img = qr.make_image()

```

The version parameter is an integer from **1** to **40** that controls the size of the QR Code (the smallest, version **1**, is a **21x21** matrix). 
Set to **None** and use the fit parameter when making the code to determine this automatically.

The **error_correction** parameter controls the error correction used for the QR Code. 

The following four constants are made available on the qrcode package:

`ERROR_CORRECT_L`:    About 7% or less errors can be corrected.
`ERROR_CORRECT_M (default)`:    About 15% or less errors can be corrected.
`ERROR_CORRECT_Q`:    About 25% or less errors can be corrected.
`ERROR_CORRECT_H`:    About 30% or less errors can be corrected.
The **box_size** parameter controls how many pixels each **box** (black squares) at the corner of the QR code is.

The **border** parameter controls how many boxes thick the border should be (the default is **4**, which is the minimum according to the specs).





