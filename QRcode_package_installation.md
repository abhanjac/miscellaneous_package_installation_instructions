# Installing QRcode package for the Odroid XU4 and Laptop:

The qr code generating package of the python is downloaded from the following [site](https://pypi.python.org/pypi/qrcode#downloads).

It will be downloaded as a **.gz** file.
Extract the file in the desired directory by the command

`tar <destination_directory> -xvf <the qrcode .gz filepath>`

This will create a new directory **<qrcode_directory>**.
Then change the permission of the directory so that the scripts inside can write new files inside.

`sudo chmod a+rwx <qrcode_directory>`

Then inside the directory there is a script called setup.py which you have to run by the following command.

`python setup.py -v build`

This will create a new directory inside the <qrcode_directory> folder called build.
Then type the command

`python setup.py -v install`

This will install the package.

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

The border parameter controls how many boxes thick the border should be (the default is 4, which is the minimum according to the specs).





