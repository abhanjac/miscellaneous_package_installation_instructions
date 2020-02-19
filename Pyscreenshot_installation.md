# Installing Pyscreenshot package for Ubuntu:

The pyscreenshot package helps to capture parts of the screen of the computer into the python code as an array. Then you can apply different kinds of image processing to this.
This is equivalent to the ImageGrab package of python that is only applicable to the Mac and Windows version of python.

Some details are also available in this [link](https://stackoverflow.com/questions/43520757/imagegrab-alternative-in-linux).

A very good documentation is available [here](https://pyscreenshot.readthedocs.io/en/latest/)

This is the download [Link](https://github.com/ponty/pyscreenshot) for pyscreenshot.

#### Dependencies:

install pip
install PIL or Pillow

**optional back-ends**

`sudo apt-get install scrot imagemagick python-gtk2 python-qt4 python-wxgtk2.8 python-pyside`

#### Pyscreenshot installation:

```
pip install pyscreenshot
```

Or using apt-get:

```
sudo apt-get install python-pip
sudo apt-get install python-pil
sudo pip install pyscreenshot
```

#### For Uninstallation:

pip uninstall pyscreenshot

#### How to use:

```
>>> import pyscreenshot as ImageGrab
>>> im = ImageGrab.grab()       # This will behave as a PIL image. So format is RGB nor BGR.
>>> im2 = np.asanyarray(im)     # Convert to numpy array.
>>>
>>> im = ImageGrab.grab(bbox=(10, 10, 510, 510))  # X1,Y1,X2,Y2     # Grab and show the part of the screen.
>>>
```

You can also make the program smart to know whether it is running on linux or windows and import the appropriate package as per that like the following.

```
>>> import sys
>>>
>>> if sys.platform == 'linux':
>>>    import pyscreenshot as ImageGrab
>>>    print( 'system is linux' )
>>>
>>> elif sys.platform == 'win32':
>>>    from PIL import ImageGrab
>>>    print( 'system is win32' )
```



